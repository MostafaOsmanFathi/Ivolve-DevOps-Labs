# Lab 23 - ArgoCD GitOps Deployment

## Overview

Reconfigure the Jenkins shared library and pipeline so that after a Docker image is pushed, it updates the GitOps repository, and ArgoCD reconciles and deploys the new image to the cluster.

## Proof

### 1. Before pipeline

![Before pipeline](image.png)

*Checking the GitOps setup before running the pipeline.*

![Check Jenkins job](image-1.png)

*Reviewing the Jenkins job before the pipeline runs.*

![Review pipeline config](image-3.png)

*Reviewing the pipeline configuration before execution.*

![Review shared library setup](image-2.png)

*Reviewing the shared library setup before execution.*

![Review shared library variables](image-4.png)

*Reviewing the shared library variables before execution.*

### 2. Reconfigure the Jenkins shared library and pipeline for ArgoCD

## Jenkins Shared Library

### `vars/updateArgoCD.groovy`

```groovy
// Shared library step: updateArgoCD
// After an image has been pushed, this step updates the image tag in the
// GitOps repository (IVolveArgoCD) and pushes the change, so ArgoCD can
// reconcile the new image into the cluster.
//
// Usage:
//   updateArgoCD()
//   updateArgoCD(image: 'org/app', tag: env.BUILD_NUMBER, gitCredentialsId: 'Github')
def call(Map config = [:]) {
    def image = config.image ?: env.DOCKER_IMAGE ?: 'mostafaosmanfathi/ivolve-nodejs-app'
    def tag = config.tag ?: env.BUILD_NUMBER ?: 'latest'
    def fullImage = "${image}:${tag}"
    def gitOpsRepoUrl = config.gitOpsRepoUrl ?: 'https://github.com/MostafaOsmanFathi/IVolveArgoCD.git'
    def gitOpsBranch = config.gitOpsBranch ?: 'main'
    def manifestPath = config.manifestPath ?: 'ArgoCD/deployment.yaml'
    def gitCredentialsId = config.gitCredentialsId ?: 'Github'
    def workDir = config.workDir ?: "ivolve-argocd-${tag}"

    echo "======== Update GitOps manifest for ArgoCD ========"
    sh "rm -rf ${workDir}"

    def repoPath = gitOpsRepoUrl.replace('https://', '')

    withCredentials([
        usernamePassword(
            credentialsId: gitCredentialsId,
            usernameVariable: 'GIT_USERNAME',
            passwordVariable: 'GIT_PASSWORD'
        )
    ]) {
        sh "git clone https://\${GIT_USERNAME}:\${GIT_PASSWORD}@${repoPath} ${workDir}"
    }

    sh "sed -i 's|${image}:.*|${fullImage}|' ${workDir}/${manifestPath}"

    withCredentials([
        usernamePassword(
            credentialsId: gitCredentialsId,
            usernameVariable: 'GIT_USERNAME',
            passwordVariable: 'GIT_PASSWORD'
        )
    ]) {
        sh """
        cd ${workDir}
        git config user.email "jenkins@ivolve.local"
        git config user.name "Jenkins"
        git add ${manifestPath}
        git commit -m "chore: update image to ${fullImage}" || echo "No changes to commit"
        git push https://\${GIT_USERNAME}:\${GIT_PASSWORD}@${repoPath} ${gitOpsBranch}
        """
    }

    sh "rm -rf ${workDir}"
    echo "GitOps manifest updated to ${fullImage}; ArgoCD will reconcile."
}
```

## Jenkins Pipeline

### New Jenkinsfile

```groovy
@Library('jenkins-shared-lib') _

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

        stage('Update GitOps manifest') {
            steps {
                updateArgoCD()
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

### 3. Run the fully automated pipeline

![Start pipeline](image-5.png)

*Starting the Jenkins pipeline for the ArgoCD update.*

![Run pipeline stage](image-6.png)

*Running the build stage.*

![Run pipeline stage](image-7.png)

*Running the Docker image build stage.*

![Run pipeline stage](image-8.png)

*Running the Docker image push stage.*

![Run pipeline stage](image-9.png)

*Running the GitOps manifest update stage.*

![Run pipeline stage](image-10.png)

*Running the ArgoCD reconciliation stage.*

![Run pipeline stage](image-11.png)

*Waiting for the ArgoCD deployment rollout.*

![Run pipeline stage](image-12.png)

*Completing the pipeline stage.*

![Pipeline result](image-13.png)

*Confirming the pipeline result.*

### 4. Testing the fully automated pipeline

![Check pipeline settings](image-14.png)

*Checking the pipeline settings for the fully automated test.*

![Review pipeline settings](image-15.png)

*Reviewing the pipeline settings before running the test.*

![Running the test](image-16.png)

*Running the fully automated pipeline test.*

![Running the test](image-17.png)

*Running the automated pipeline test stages.*

![Check ArgoCD deployment](image-18.png)

*Confirming the ArgoCD deployment after the automated run.*

![Check ArgoCD deployment](image-19.png)

*Verifying the updated ArgoCD deployment.*

![Check ArgoCD deployment](image-20.png)

*Verifying the deployed application.*

![Check ArgoCD deployment](image-21.png)

*Verifying the ArgoCD sync status.*

![Check ArgoCD deployment](image-22.png)

*Confirming the rollout completed.*

![Pipeline result](image-23.png)

*Confirming the fully automated pipeline test finished successfully.*
