# Lab 18 - Node-Wide Pod Management with DaemonSet

## Overview

Deploy a monitoring stack in a dedicated namespace using a DaemonSet for the Prometheus node-exporter, then validate scheduling and metrics exposure, and configure Prometheus and Grafana.

## Proof

### 1. Create the monitoring namespace

![Create the monitoring namespace](image.png)

*Creating the monitoring namespace.*

### 2. Deploy Prometheus

![Deploy Prometheus](image-1.png)

*Deploying Prometheus in the monitoring namespace.*

### 3. Deploy Alert Manager

![Deploy Alert Manager](image-2.png)

*Deploying Alert Manager in the monitoring namespace.*

### 4. Deploy Grafana

![Deploy Grafana](image-3.png)

*Deploying Grafana in the monitoring namespace.*

### 5. Deploy the node-exporter DaemonSet

![Deploy the node-exporter DaemonSet](image-4.png)

*Deploying the Prometheus node-exporter DaemonSet.*

### 6. Validate the node-exporter pod

![Validate the node-exporter pod](image-5.png)

*Validating that a node-exporter pod is scheduled on each node.*

### 7. Confirm metrics exposure

![Confirm metrics exposure](image-6.png)

*Confirming metrics exposure on `<node_IP>:9100/metrics`.*

### 8. Configure Prometheus

![Configure Prometheus](image-8.png)

*Configuring Prometheus to collect node-exporter metrics.*

### 9. Configure Grafana dashboard

![Configure Grafana dashboard](image-9.png)

*Configuring the Grafana dashboard.*

### 10. Verify the monitoring stack

![Verify Prometheus and Grafana](image-7.png)

*Verifying Prometheus and Grafana are running.*

![Verify the monitoring stack](image-10.png)

*Verifying the monitoring stack is running.*

### 11. Check the final monitoring resources

![Check the final monitoring resources](image-11.png)

*Confirming the monitoring resources.*

### 12. Check the DaemonSet status

![Check the DaemonSet status](image-12.png)

*Confirming the DaemonSet status.*
