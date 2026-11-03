# Lab 20 - Jenkins Pipeline for Application Deployment

## Overview

Create a Jenkins pipeline that automates build, Docker image build, scan, push, delete local image, update deployment, and deploy to Kubernetes.

## Proof

### 1. Clone source code and Dockerfile

![Clone source code and Dockerfile](image.png)

*Cloning the source code and Dockerfile from GitHub.*

### 2. Create the pipeline

![Create the pipeline](image-1.png)

*Creating the Jenkins pipeline.*

### 3. Build app

![Build app](image-2.png)

*Building the app.*

### 4. Build Docker image

![Build Docker image](image-3.png)

*Building the Docker image.*

### 5. Scan Docker image

![Scan Docker image](image-4.png)

*Scanning the Docker image.*

### 6. Push image to DockerHub

![Push image to DockerHub](image-5.png)

*Pushing the image to DockerHub.*

### 7. Delete image locally

![Delete image locally](image-6.png)

*Deleting the image locally.*

### 8. Update image in deployment file

![Update image in deployment file](image-7.png)

*Updating the image in the deployment file.*

### 9. Deploy to Kubernetes cluster

![Deploy to Kubernetes cluster](image-8.png)

*Deploying to the Kubernetes cluster.*

### 10. Set post actions

![Set post actions](image-9.png)

*Setting the pipeline post actions (always, success, failure).*

### 11. Verify pipeline result

![Verify pipeline result](image-10.png)

*Verifying the pipeline result.*

### 12. Check DockerHub push

![Check DockerHub push](image-11.png)

*Checking the DockerHub push.*

### 13. Check deployment update

![Check deployment update](image-12.png)

*Checking the deployment update.*

### 14. Check final deployment

![Check final deployment](image-13.png)

*Confirming the final deployment.*
