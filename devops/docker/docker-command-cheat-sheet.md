---
icon: clipboard-list
---

# Docker Command Cheat Sheet

{% hint style="info" %}
Docker is a platform for developing, shipping, and running applications in containers. It provides a lightweight, consistent, and isolated environment for applications, making it easier to develop, test, and deploy across different environments.
{% endhint %}

#### 🚀 Basic Container Commands

* **Managing Containers**:
  * `docker run <options> <image>`: Create and start a new container from an image.
    * Use `-d` to run in detached mode.
    * Use `--name <name>` to assign a name to the container.
    * Use `-p <host_port>:<container_port>` to map ports.
  * `docker start <container>`: Start a stopped container.
  * `docker stop <container>`: Gracefully stop a running container.
  * `docker restart <container>`: Restart a running or stopped container.
  * `docker pause <container>`: Pause all processes in the container.
  * `docker unpause <container>`: Unpause all processes in the container.
  * `docker rm <container>`: Remove a stopped container.
  * `docker kill <container>`: Forcefully stop a container.

#### 🏗️ Image Commands

* **Managing Images**:
  * `docker build -t <name>:<tag> <path>`: Build an image from a Dockerfile in the specified path.
    * Use `--no-cache` to build without cache.
    * Use `--pull` to always attempt to pull a newer version of the base image.
  * `docker images`: List all images.
  * `docker rmi <image>`: Remove an image.
  * `docker pull <image>`: Download an image from a registry.
  * `docker push <image>`: Upload an image to a registry.

#### 📊 Status & Information

* **Inspect & Logs**:
  * `docker ps`: List running containers.
  * `docker ps -a`: List all containers, including stopped ones.
  * `docker inspect <container|image>`: View detailed information about a container or image.
  * `docker logs <container>`: View logs from a container.
  * `docker logs -f <container>`: Follow container logs in real-time.

#### 🔄 Runtime Management

* **Execute Commands**:
  * `docker exec <container> <command>`: Run a command in a running container.
    * Use `-it` for interactive mode with a TTY, e.g., `docker exec -it <container> bash`.
  * `docker attach <container>`: Attach your terminal to a running container.

#### 🧹 Cleanup Commands

* **System Cleanup**:
  * `docker system prune`: Remove all unused containers, networks, images (both dangling and unreferenced).
  * `docker volume prune`: Remove all unused volumes.
  * `docker network prune`: Remove all unused networks.
  * `docker container prune`: Remove all stopped containers.
  * `docker image prune`: Remove unused images.

#### 🔌 Network Commands

* **Managing Networks**:
  * `docker network ls`: List all networks.
  * `docker network create <name>`: Create a new network.
  * `docker network rm <name>`: Remove a network.
  * `docker network inspect <network>`: Display detailed information about a network.

#### 📦 Volume Commands

* **Managing Volumes**:
  * `docker volume ls`: List all volumes.
  * `docker volume create <name>`: Create a new volume.
  * `docker volume rm <name>`: Remove a volume.
  * `docker volume inspect <volume>`: Display detailed information about a volume.

#### 🌟 Advanced Usage

* **Tagging & Pushing**:
  * `docker tag <image> <new_image>`: Tag an image with a new name.
  * `docker push <repository>/<image>:<tag>`: Push an image to a repository.
* **Security & Limits**:
  * `docker run --rm --cap-drop ALL <image>`: Run a container with all capabilities dropped.

#### 💡 Tips

* Use `-v <host_path>:<container_path>` in `docker run` to mount host directories into containers.
* Use `--network <network>` to connect a container to a specific network.
* Use `docker inspect <container> --format '{{ .NetworkSettings.IPAddress }}'` to get a container’s IP address.
