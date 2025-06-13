
# Docker Registry, Docker Hub, and Multi-Stage Build

## 1. Docker Registry

A **Docker Registry** is a storage and distribution system for Docker images. It allows you to store your images in a centralized location and share them with others.

- **Public Registry**: Docker Hub (default)
- **Private Registry**: Self-hosted using Docker Registry or third-party services like AWS ECR, Azure ACR, Google GCR.

## 2. Docker Hub

Docker Hub is the default public registry provided by Docker Inc.

### Steps to Use Docker Hub

1. **Create an Account**: Sign up at [https://hub.docker.com](https://hub.docker.com)
2. **Login via CLI**:

```bash
docker login
```

3. **Tag your image**:

```bash
docker tag my-image username/my-image:tag
```

4. **Push to Docker Hub**:

```bash
docker push username/my-image:tag
```

5. **Pull the image from anywhere**:

```bash
docker pull username/my-image:tag
```

---

## 3. Multi-Stage Build

Multi-stage builds help optimize the size of the final Docker image by using multiple `FROM` statements in your Dockerfile.

### Example: Build a Go App

```dockerfile
# First stage: build the application
FROM golang:1.18 AS builder

WORKDIR /app

COPY . .

RUN go build -o myapp

# Second stage: create a minimal image
FROM alpine:latest

WORKDIR /root/

# Copy the binary from the builder stage
COPY --from=builder /app/myapp .

# Command to run the app
CMD ["./myapp"]
```

### Steps to Build and Run

1. Save the above Dockerfile in your project directory.
2. Build the multi-stage image:

```bash
docker build -t my-go-app .
```

3. Run the container:

```bash
docker run my-go-app
```

---

## Benefits of Multi-Stage Builds

- Smaller final image size
- More secure (only necessary binaries are included)
- Easier to manage build dependencies
