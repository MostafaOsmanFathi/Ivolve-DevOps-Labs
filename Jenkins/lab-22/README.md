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
