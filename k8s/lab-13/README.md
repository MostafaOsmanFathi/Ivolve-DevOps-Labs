# Lab 13 - Kubernetes Persistent Storage

## Overview

Setting up persistent storage for application logs using a PersistentVolume and PersistentVolumeClaim.

## Proof

### 1. Create the Persistent Volume
![Create the Persistent Volume](image.png)

*Defining a PersistentVolume with hostPath storage for application logs.*

### 2. Create the Persistent Volume Claim
![Create the Persistent Volume Claim](image-1.png)

*Requesting a PersistentVolumeClaim for the application logs.*

### 3. Apply the storage resources
![Apply the storage resources](image-2.png)

*Creating the PersistentVolume and PersistentVolumeClaim.*

### 4. Verify the storage
![Verify the storage](image-3.png)

*Checking that the storage resources are bound and ready.*
