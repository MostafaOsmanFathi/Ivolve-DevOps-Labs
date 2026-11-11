# Lab 28 - AWS EC2 with Ansible

## Overview

Use the Amazon AWS EC2 dynamic inventory plugin in Ansible to automatically discover and manage the running EC2 instances for the lab.

## Proof

### 1. Review the EC2 dynamic inventory

```yaml
---
plugin: amazon.aws.aws_ec2

regions:
  - us-east-1

filters:
  instance-state-name: running
  "tag:server": ivolve

hostnames:
  - public-ip-address

keyed_groups:
  - key: tags.server
    prefix: tag
```

### 2. Review the Ansible hosts inventory

```ini
[ec2]
54.158.85.64 ansible_user=ubuntu ansible_ssh_private_key_file=/home/mostafaosman/WORK/mostafa.pem
```

### 3. Run the playbook

![Run the playbook](image.png)

*Running the Ansible playbook against the discovered EC2 host.*

![Run the playbook](image-1.png)

*Waiting for the playbook to complete.*

![Run the playbook](image-2.png)

*Executing the playbook tasks.*

![Run the playbook](image-3.png)

*Verifying the playbook results.*

### 4. Verify the setup

![Check the setup status](image-6.png)

*Confirming the setup status.*

![Check the setup status](image-5.png)

*Verifying the playbook results.*

![Check the setup status](image-4.png)

*Confirming the deployment status.*
