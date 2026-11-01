# Lab 17 - Control Pod-to-Pod Traffic via Network Policy

## Overview

Control pod-to-pod traffic with a Kubernetes Network Policy that allows the MySQL pod to receive traffic only from the application pods on port 3306.

## Proof

### 1. Create the Network Policy

Create the `ivolveNP` Network Policy to allow MySQL traffic from the application pods only.

![Create the Network Policy](image.png)

*Creating the Network Policy to allow MySQL traffic from the app pods only.*

### 2. Verify the Network Policy

![Verify the Network Policy](image-1.png)

*Checking that the Network Policy is applied to the mysql pod.*

### 3. Check the enforced traffic

![Check the enforced traffic](image-2.png)

*Verifying that only the app pods can reach MySQL on port 3306.*

### 4. Confirm the policy status

![Confirm the policy status](image-3.png)

*Confirming the Network Policy is active and enforced.*
