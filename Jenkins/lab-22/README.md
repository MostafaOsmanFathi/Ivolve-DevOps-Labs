# Lab 22 - Jenkins Shared Library Pipeline

## Overview

Use a Jenkins shared library to define reusable pipeline steps for building the app, building/pushing the Docker image, and deploying to Kubernetes, then verify the agent setup and pipeline run.

## Proof

### 1. Before pipeline

![Before pipeline](image.png)

*Checking the Jenkins job before running the pipeline.*

![Check Jenkins job](image-1.png)

*Reviewing the Jenkins job before the pipeline runs.*

![Review pipeline config](image-3.png)

*Reviewing the pipeline configuration before execution.*

![Review shared library setup](image-2.png)

*Reviewing the shared library setup before execution.*

![Review shared library variables](image-4.png)

*Reviewing the shared library variables before execution.*

### 2. Agent install on controller

![Install controller agent](image-5.png)

*Installing the Jenkins agent on the controller.*

![Verify controller agent](image-6.png)

*Verifying the controller agent installation.*

![Check controller agent](image-7.png)

*Checking the controller agent status.*

### 3. Agent install on slave

![Install slave agent](image-8.png)

*Installing the Jenkins agent on the slave.*

![Verify slave agent](image-15.png)

*Verifying the slave agent installation.*

![Check slave agent](image-10.png)

*Checking the slave agent status.*

![Test agent](image-11.png)

*Testing the slave agent connection.*

![Run pipeline on slave](image-12.png)

*Running the pipeline on the slave agent.*

### 4. Testing agent

![Test agent](image-13.png)

*Testing the agent for pipeline execution.*

![Pipeline result](image-14.png)

*Confirming the pipeline result.*

## Jenkinsfile

```groovy
@Library('shared-lib') _

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'mostafaosmanfathi/ivolve-nodejs-app'
    }

    stages {
        stage('Build app') {
            steps {
                buildApp()
            }
        }

        stage('Build and push image') {
            steps {
                buildAndPushImage()
            }
        }

        stage('Deploy to K8s') {
            steps {
                deployToK8s()
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo '======== pipeline execution failed ========'
        }
    }
}
```

## Shared Library Variables

### `vars/buildAndPushImage.groovy`

```groovy
// Shared library step: buildAndPushImage
// Builds the Docker image, scans it, logs in to the registry, pushes it,
// and removes the local copy afterwards.
//
// Usage:
//   buildAndPushImage()
//   buildAndPushImage(image: 'org/app', tag: env.BUILD_NUMBER, credentialsId: 'DockerHub')
def call(Map config = [:]) {
    def image = config.image ?: env.DOCKER_IMAGE ?: 'mostafaosmanfathi/ivolve-nodejs-app'
    def tag = config.tag ?: env.BUILD_NUMBER ?: 'latest'
    def credentialsId = config.credentialsId ?: 'DockerHub'
    def scanSeverity = config.scanSeverity ?: 'HIGH,CRITICAL'
    def fullImage = "${image}:${tag}"

    echo "======== Build Docker Image ========"
    sh "docker build -t ${fullImage} ."

    echo "======== Scan Docker Image ========"
    sh "trivy image --exit-code 0 --severity ${scanSeverity} --no-progress ${fullImage}"

    echo "======== Docker Login & Push ========"
    try {
        withCredentials([
            usernamePassword(
                credentialsId: credentialsId,
                usernameVariable: 'DOCKER_USERNAME',
                passwordVariable: 'DOCKER_PASSWORD'
            )
        ]) {
            sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
            sh "docker push ${fullImage}"
        }
    } finally {
        sh 'docker logout || true'
    }

    echo "======== Delete image locally ========"
    sh "docker rmi ${fullImage} || true"
}
```

### `vars/buildApp.groovy`

```groovy
// Shared library step: buildApp
// Builds the Node.js application (install dependencies and syntax-check entrypoint).
//
// Usage:
//   buildApp()
//   buildApp(installCommand: 'npm ci', entrypoint: 'server.js')
def call(Map config = [:]) {
    def installCommand = config.installCommand ?: 'npm install'
    def entrypoint = config.entrypoint ?: 'server.js'

    echo "======== Build app ========"
    sh "${installCommand}"
    sh "node --check ${entrypoint}"
}
```

### `vars/deployToK8s.groovy`

```groovy
// Shared library step: deployToK8s
// Updates the image tag in the Kubernetes deployment manifest, configures
// kubectl, creates the image pull secret, applies the deployment and waits
// for the rollout to finish.
//
// Usage:
//   deployToK8s(image: 'org/app', tag: env.BUILD_NUMBER, namespace: 'ivolve')
def call(Map config = [:]) {
    def image = config.image ?: env.DOCKER_IMAGE ?: 'mostafaosmanfathi/ivolve-nodejs-app'
    def tag = config.tag ?: env.BUILD_NUMBER ?: 'latest'
    def fullImage = "${image}:${tag}"
    def deploymentFile = config.deploymentFile ?: 'k8s-project/deployment-nodjs.yaml'
    def namespace = config.namespace ?: 'ivolve'
    def deploymentName = config.deploymentName ?: 'ivolveapp'
    def server = config.server ?: 'https://192.168.122.54:6443'
    def k8sTokenCredentialsId = config.k8sTokenCredentialsId ?: 'k8s-jenkins-token'
    def dockerCredentialsId = config.dockerCredentialsId ?: 'DockerHub'

    echo "======== Update image in deployment file ========"
    sh "sed -i 's|${image}:.*|${fullImage}|' ${deploymentFile}"

    echo "======== Configure kubectl ========"
    withCredentials([
        string(
            credentialsId: k8sTokenCredentialsId,
            variable: 'K8S_TOKEN'
        )
    ]) {
        sh """
        kubectl config set-cluster k8s-cluster \
        --server=${server} \\


        kubectl config set-credentials jenkins \
        --token="\$K8S_TOKEN"

        kubectl config set-context jenkins \
        --cluster=k8s-cluster \
        --user=jenkins \
        --namespace=${namespace}

        kubectl config use-context jenkins
        """
    }

    echo "======== Create image pull secret ========"
    withCredentials([
        usernamePassword(
            credentialsId: dockerCredentialsId,
            usernameVariable: 'DOCKER_USERNAME',
            passwordVariable: 'DOCKER_PASSWORD'
        )
    ]) {
        sh """
        kubectl create secret docker-registry dockerhub-secret \
          --docker-server=https://index.docker.io/v1/ \
          --docker-username="\$DOCKER_USERNAME" \
          --docker-password="\$DOCKER_PASSWORD" \
          --namespace=${namespace} \
          --dry-run=client -o yaml | kubectl apply -f -
        """
    }

    echo "======== Apply deployment ========"
    sh "kubectl apply -f ${deploymentFile}"

    echo "======== Rollout status ========"
    sh "kubectl rollout status deployment/${deploymentName} -n ${namespace} --timeout=600s"
}
```

### `vars/buildApp.groovy`

```groovy
// Shared library step: buildApp
// Builds the Node.js application (install dependencies and syntax-check entrypoint).
//
// Usage:
//   buildApp()
//   buildApp(installCommand: 'npm ci', entrypoint: 'server.js')
def call(Map config = [:]) {
    def installCommand = config.installCommand ?: 'npm install'
    def entrypoint = config.entrypoint ?: 'server.js'

    echo "======== Build app ========"
    sh "${installCommand}"
    sh "node --check ${entrypoint}"
}
```
