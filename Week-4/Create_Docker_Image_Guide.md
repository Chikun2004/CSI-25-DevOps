# Docker Image Creation 

This explains how to create Docker images using two different methods: via a Dockerfile and from running containers.

---

##  Method 1: Create Docker Image Using Dockerfile

### 1. Prerequisites
- Docker installed on your machine.
- A basic project with an application (e.g., Python app with `app.py` and `requirements.txt`).

### 2. Create Project Directory

```bash
mkdir my-docker-app
cd my-docker-app
```

Add your project files inside this directory.

### 3. Create a Dockerfile

```Dockerfile
# Use official Ubuntu image
FROM ubuntu:latest

# Maintainer Info
LABEL maintainer="Your Name <your.email@example.com>"

# Install Python
RUN apt-get update -y && \
    apt-get install -y python3 python3-pip

# Set working directory
WORKDIR /app

# Copy project files
COPY . /app

# Install dependencies
RUN pip3 install -r requirements.txt

# Expose app port
EXPOSE 5000

# Start application
CMD ["python3", "app.py"]
```

### 4. Build Docker Image

```bash
docker build -t my-python-app .
```

### 5. Run Docker Container

```bash
docker run -p 5000:5000 my-python-app
```

---

##  Method 2: Create Docker Image from Running Container

### 1. Run a Container Interactively

```bash
docker run -it ubuntu /bin/bash
```

Install software, update packages, configure the container as needed.

### 2. Commit the Container as an Image

```bash
docker ps -a  # Get container ID
docker commit <container_id> my-custom-image
```

### 3. Use the New Image

```bash
docker run -it my-custom-image
```

---

##  Clean Up

### Stop and Remove Containers

```bash
docker stop <container_id>
docker rm <container_id>
```

### Remove Images

```bash
docker rmi my-python-app my-custom-image
```

---

##  Optional: Push to Docker Hub

### 1. Tag the Image

```bash
docker tag my-python-app yourdockerhubusername/my-python-app
```

### 2. Login and Push

```bash
docker login
docker push yourdockerhubusername/my-python-app
```

---

## Summary

| Task | Command |
|------|---------|
| Build from Dockerfile | `docker build -t <image-name> .` |
| Run Container | `docker run -p <host-port>:<container-port> <image-name>` |
| Commit Running Container | `docker commit <container_id> <new-image-name>` |
| Push to Docker Hub | `docker push <username>/<image-name>` |

---
