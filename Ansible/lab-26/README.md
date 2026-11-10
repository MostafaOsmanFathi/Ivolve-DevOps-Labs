# Lab 26 - Ansible Roles for Cluster Setup

## Overview

Organize the Ansible setup into reusable roles for Docker, Kubernetes and Jenkins under the `roles/` directory.

## Proof

### 1. Review the Ansible structure

```sh
ansible-lab/
│
├── inventory
├── site.yml
│
└── roles/
    ├── docker/
    │   └── tasks/
    │       └── main.yml
    │
    ├── kubectl/
    │   └── tasks/
    │       └── main.yml
    │
    └── jenkins/
        └── tasks/
            └── main.yml
```

### 2. Review the roles

![Review the Ansible structure](image.png)

*Reviewing the Ansible directory structure.*

![Review the docker role](image-1.png)

*Reviewing the Docker role tasks.*

![Review the kubectl role](image-2.png)

*Reviewing the Kubernetes role tasks.*

### 3. Run the playbook

![Run the playbook](image-3.png)

*Running the playbook with the new roles.*

![Run the playbook](image-4.png)

*Waiting for the playbook to complete.*

![Run the playbook](image-5.png)

*Verifying the playbook results.*

![Run the playbook](image-6.png)

*Confirming the setup status.*
