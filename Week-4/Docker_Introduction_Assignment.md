
# Introduction to Containerization and Docker Fundamentals

## What is Containerization?

Containerization is a lightweight form of virtualization that allows you to package applications along with their dependencies into isolated environments called **containers**. This approach ensures that software runs consistently across different environments — development, testing, and production.

### Benefits of Containerization:
- Portability
- Scalability
- Consistency
- Resource efficiency
- Faster deployment

## Docker Fundamentals

**Docker** is a leading containerization platform that enables developers to easily create, deploy, and manage containers.

### Key Concepts:

- **Docker Image**: A read-only template with instructions for creating a Docker container. Images are the building blocks of containers.
- **Docker Container**: A runnable instance of a Docker image.
- **Dockerfile**: A script containing a series of instructions on how to build a Docker image.
- **Docker Hub**: A public registry where Docker images are stored and shared.
- **Docker Engine**: The core component that runs Docker containers.

## Installing Docker

Visit the official Docker website to download and install Docker for your operating system:

- [Docker for Windows](https://docs.docker.com/desktop/install/windows-install/)
- [Docker for macOS](https://docs.docker.com/desktop/install/mac-install/)
- [Docker for Linux](https://docs.docker.com/engine/install/)

## Basic Docker Commands

| Command | Description |
|--------|-------------|
| `docker pull <image>` | Download an image from Docker Hub |
| `docker run <image>` | Run a container from an image |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers (including stopped ones) |
| `docker stop <container_id>` | Stop a running container |
| `docker rm <container_id>` | Remove a stopped container |
| `docker rmi <image>` | Remove an image |
| `docker build -t <image_name> .` | Build an image from a Dockerfile |

## Hands-on Example

Let's run a simple Nginx web server container:

```bash
docker pull nginx
docker run -d -p 8080:80 nginx
```

Visit `http://localhost:8080` in your browser — you should see the Nginx welcome page!

## Conclusion

Docker and containerization have revolutionized how applications are developed, deployed, and managed. With a few simple commands, you can create consistent, portable, and scalable environments for any application.

Happy Dockering! 🐳
