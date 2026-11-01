# Lab 16 - Pod Resource Management with CPU and Memory Requests and Limits

## Overview

Update the existing `ivolveApp` Deployment with CPU and memory requests and limits, then verify them with `kubectl describe pod` and monitor real-time usage with `kubectl top pod` (the metric server must be running).

## Proof

### 1. Update the Deployment resources

Update the `ivolveApp` Deployment with resource requests and limits.

![Update the Deployment resources](image.png)

*Adding CPU and memory requests and limits to the ivolveApp Deployment.*

### 2. Verify the resources

![Verify the resources](image-1.png)

*Checking the pod resource requests and limits with `kubectl describe pod`.*

### 3. Monitor real-time usage

![Monitor real-time usage](image-2.png)

*Monitoring real-time CPU and memory usage with `kubectl top pod`.*
