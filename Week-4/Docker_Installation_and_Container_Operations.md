
# Docker Installation and Basic Container Operations

## 1. Docker Installation

- **Windows**: Download Docker Desktop from [Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-windows).
- **Mac**: Download Docker Desktop from [Docker Hub](https://hub.docker.com/editions/community/docker-ce-desktop-mac).
- **Linux**: Follow the instructions for your specific distribution from [Docker Docs](https://docs.docker.com/engine/install/).

## 2. Verify Installation

Open a terminal (Command Prompt or PowerShell for Windows):

```bash
docker --version
docker run hello-world
```

## 3. Basic Docker Commands

- `docker pull <image>`: Pull an image from Docker Hub.
- `docker images`: List all local images.
- `docker ps`: List running containers.
- `docker run <image>`: Run a container from an image.
- `docker stop <container>`: Stop a running container.
- `docker rm <container>`: Remove a container.
- `docker rmi <image>`: Remove an image.

## 4. Building an Image from Dockerfile

Create a `Dockerfile` in your project directory:

```dockerfile
# Use an official base image
FROM ubuntu:18.04

# Set the working directory
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip

# Install dependencies
RUN pip3 install -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python3", "app.py"]
```

## 5. Build the Docker Image

In the terminal, navigate to your project directory containing `Dockerfile`:

```bash
docker build -t my-python-app .
```

## 6. Run the Docker Container

```bash
docker run -p 4000:80 my-python-app
```

## 7. Access Your Application

Open a web browser and go to `http://localhost:4000` to see your Python application running.
