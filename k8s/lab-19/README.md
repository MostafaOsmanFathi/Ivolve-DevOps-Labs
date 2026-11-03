# Lab 19 - Securing Kubernetes with RBAC and Service Accounts

## Overview

Secure the Kubernetes cluster by creating a Jenkins service account, token, deploy role, and role binding, then validate the role permissions.

## Proof

### 1. Create the Jenkins Service Account

![Create the Jenkins Service Account](image.png)

*Creating the jenkinsSA service account in the ivolve namespace.*

### 2. Create the service account token

![Create the service account token](image-1.png)

*Creating a token for the service account.*

### 3. Define the deploy role

![Define the deploy role](image-2.png)

*Defining the ivolveDeployRole with create/update permissions on deployments in the ivolve namespace.*

### 4. Create the role binding

![Create the role binding](image-3.png)

*Creating the ivolveDeployRoleBinding to bind the role to the jenkinsSA service account.*

### 5. Validate the permissions

![Validate the permissions](image-4.png)

*Validating that the role can only create/update deployments in the ivolve namespace.*
