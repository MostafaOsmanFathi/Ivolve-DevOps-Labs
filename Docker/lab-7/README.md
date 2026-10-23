# Lab 7 - Microservices with Custom Networks and Volumes

## Overview

Three services (frontend, backend, notification) built as separate images and connected with two bridge networks. The backend and notification use named volumes for their logs, and the backend also bind-mounts `bind-vol` for its data file. Service details are in `MicroserviceApp/README.md`.

## Proof

### 1. Build the frontend

`docker build -t frontend frontend/.`

![Build frontend](image.png)

### 2. Build the notification service

`docker build -t notification notification/.`

![Build notification](image-1.png)

### 3. Build the backend

`docker build -t backend backend/.`

![Build backend](image-2.png)

### 4. Create the two networks

`docker network create --driver bridge --subnet 192.168.15.0/24 frontend-backend` and `docker network create --driver bridge --subnet 192.168.16.0/24 backend-notification`.

![Create networks](image-3.png)

### 5. Create the log volumes

`docker volume create backend-logs` and `docker volume create notification-logs`.

![Create volumes](image-4.png)

### 6. Run the backend and connect both networks

`docker run -d -v backend-logs:/app/logs/ --network frontend-backend backend:latest`, then `docker network connect backend-notification <id>`.

![Run backend and connect networks](image-5.png)

### 7. Run backend with a bind mount

Backend run with `-v backend-logs:/app/logs/ -v "$(pwd)/bind-vol/:/app/data/" --name backend`, then connected to the notification network.

![Backend with bind mount](image-6.png)

### 8. Run the notification service

`docker run -d -v notification-logs:/app/logs/ --network backend-notification --name notification notification:latest`.

![Run notification](image-7.png)

### 9. Run the frontend and check containers

`docker run -d --network frontend-backend -p 8080:3000 --name frontend frontend:latest`; `docker ps` shows frontend on `8080->3000`, backend on 5000, and notification on 5001.

![Run frontend and docker ps](image-8.png)

### 10. The running app

The frontend page in the browser shows the stored messages (`11`, `mostafa osman`).

![Frontend page](image-9.png)

### 11. Data persisted to the host

`bind-vol/messages.json` on the host contains `["11", "mostafa osman"]`, confirming the data persisted through the bind mount.

![messages.json on host](image-10.png)
