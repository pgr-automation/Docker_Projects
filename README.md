# Docker_Projects
# Docker Commands Cheat Sheet

This guide provides a comprehensive list of essential Docker commands with explanations. Perfect for beginners and experienced Docker users alike!

---

## Table of Contents

- [Docker Basics](#docker-basics)
- [Docker Images](#docker-images)
- [Docker Containers](#docker-containers)
- [Networking](#networking)
- [Docker Volumes (Data Persistence)](#docker-volumes-data-persistence)
- [Docker Compose](#docker-compose)
- [Docker System and Clean-Up](#docker-system-and-clean-up)

---

### Docker Basics

1. **`docker --version`**  
   Shows the installed version of Docker.

2. **`docker info`**  
   Displays detailed information about Docker, including container and image stats.

3. **`docker help`**  
   Provides a list of all available Docker commands with brief descriptions.

---

### Docker Images

1. **`docker pull <image_name>`**  
   Downloads a Docker image from Docker Hub or a specified repository.  
   *Example:* `docker pull ubuntu`

2. **`docker images`**  
   Lists all downloaded images on the local system.

3. **`docker rmi <image_name_or_id>`**  
   Removes a specified Docker image. Use `-f` to force-remove an image in use by containers.

4. **`docker tag <source_image> <target_image>`**  
   Tags an existing image with a new name, useful for renaming before pushing to a repository.

5. **`docker build -t <tag_name> <path>`**  
   Builds an image from a Dockerfile in the specified directory, tagging it with the given name.

---

### Docker Containers

1. **`docker run <options> <image_name>`**  
   Creates and starts a new container from an image. Common options include:
   - `-d` (detached mode)
   - `-it` (interactive mode)
   - `--name` (name the container)
   - `-p` (port mapping)  
   *Example:* `docker run -d -p 8080:80 nginx`

2. **`docker ps`**  
   Lists all currently running containers. Use `-a` to list all containers, including stopped ones.

3. **`docker start <container_id_or_name>`**  
   Starts a stopped container.

4. **`docker stop <container_id_or_name>`**  
   Stops a running container.

5. **`docker restart <container_id_or_name>`**  
   Stops and starts a container again, useful for applying changes.

6. **`docker rm <container_id_or_name>`**  
   Removes a stopped container. Use `-f` to force-remove a running container.

7. **`docker exec -it <container_id_or_name> <command>`**  
   Runs a command inside an existing container.  
   *Example:* `docker exec -it <container_id> /bin/bash`

8. **`docker logs <container_id_or_name>`**  
   Shows logs of a running container, useful for debugging.

9. **`docker inspect <container_id_or_name>`**  
   Displays detailed metadata and configuration of the container or image.

10. **`docker top <container_id_or_name>`**  
    Lists processes running within the container, similar to `top` on a regular system.

---

### Networking

1. **`docker network ls`**  
   Lists all available Docker networks.

2. **`docker network create <network_name>`**  
   Creates a new network for container communication.

3. **`docker network connect <network_name> <container_name>`**  
   Connects a container to a specified network.

4. **`docker network disconnect <network_name> <container_name>`**  
   Disconnects a container from a specified network.

---

### Docker Volumes (Data Persistence)

1. **`docker volume create <volume_name>`**  
   Creates a new volume for data storage.

2. **`docker volume ls`**  
   Lists all volumes on the Docker host.

3. **`docker volume rm <volume_name>`**  
   Removes a specified volume.

4. **`docker run -v <volume_name>:<path_in_container> <image_name>`**  
   Mounts a volume to a container for persistent storage.  
   *Example:* `docker run -v mydata:/data ubuntu` mounts the volume `mydata` at `/data` in the container.

---

### Docker Compose

1. **`docker-compose up`**  
   Starts containers as specified in the `docker-compose.yml` file. Add `-d` for detached mode.

2. **`docker-compose down`**  
   Stops and removes containers, networks, and volumes created by `docker-compose up`.

3. **`docker-compose build`**  
   Builds or rebuilds images defined in a `docker-compose.yml` file.

4. **`docker-compose logs`**  
   Displays logs from all containers defined in `docker-compose.yml`.

5. **`docker-compose ps`**  
   Lists containers started by Docker Compose.

---

### Docker System and Clean-Up

1. **`docker system df`**  
   Shows disk usage by Docker images, containers, and volumes.

2. **`docker system prune`**  
   Removes unused data, including stopped containers, dangling images, and unused networks. Use `-a` to remove all unused images.

3. **`docker stats`**  
   Shows real-time resource usage statistics for running containers.

---

Master these commands to manage Docker images, containers, networks, and volumes with ease. Happy Dockering!

```Dockerfile
# Use Ubuntu as the base image
FROM ubuntu:20.04

# Metadata about the image
LABEL maintainer="you@example.com"

# Set up an argument with default value
ARG NODE_VERSION=14

# Install necessary packages and clean up
RUN apt-get update && apt-get install -y nodejs=$NODE_VERSION

# Set an environment variable
ENV APP_HOME /app

# Set the working directory
WORKDIR $APP_HOME

# Copy the application code
COPY . .

# Expose the port the app runs on
EXPOSE 3000

# Define the command to start the app
CMD ["node", "server.js"]
```