---
icon: sheet-plastic
---

# Docker Compose Command Cheat Sheet

{% hint style="info" %}
Docker Compose is a powerful tool for defining and running multi-container Docker applications. It's particularly useful for managing applications that consist of multiple services, like a web server, database, and caching layer, which need to work together seamlessly.
{% endhint %}

{% hint style="warning" %}
Before using `docker-compose`, ensure you are in the directory containing your `docker-compose.yml` file. This file defines the services, networks, and volumes your application will use.
{% endhint %}

#### 🚀 Basic Commands

* **Start Containers**:
  * `docker-compose up`: Start all services defined in the `docker-compose.yml` file. This command will create and start containers, networks, and volumes.
  * `docker-compose up -d`: Start services in detached mode, running them in the background.
  * `docker-compose up --build`: Build images before starting containers, useful if you've made changes to your Dockerfile.
  * `docker-compose up <service>`: Start a specific service by name.
* **Stop Containers**:
  * `docker-compose down`: Stop and remove containers, networks, and optionally, volumes and images created by `up`.
  * `docker-compose down -v`: Also remove named volumes declared in the `volumes` section of the Compose file.
  * `docker-compose stop`: Gracefully stop running containers without removing them.

#### 🏗️ Build Commands

* **Build Services**:
  * `docker-compose build`: Build or rebuild services.
  * `docker-compose build <service>`: Build a specific service.
  * `docker-compose build --no-cache`: Build images without using cache, ensuring that each layer is rebuilt.
  * `docker-compose build --pull`: Always attempt to pull a newer version of the image.

#### 📊 Status & Information

* **Container Status**:
  * `docker-compose ps`: List all containers with their status.
  * `docker-compose ps <service>`: List a specific service's container.
  * `docker-compose top`: Display running processes within containers.
  * `docker-compose port <service> <port>`: Show port mapping for a specific service.
* **Logs**:
  * `docker-compose logs`: View output from containers.
  * `docker-compose logs -f`: Follow log output live.
  * `docker-compose logs <service>`: View logs for a specific service.
  * `docker-compose logs --tail=100`: Show the last 100 lines of logs.

#### 🔄 Runtime Management

* **Restart Services**:
  * `docker-compose restart`: Restart all services.
  * `docker-compose restart <service>`: Restart a specific service.
* **Scale Services**:
  * `docker-compose scale <service>=<num>`: Scale a service to the specified number of instances.
* **Pause/Unpause**:
  * `docker-compose pause`: Pause all services.
  * `docker-compose pause <service>`: Pause a specific service.
  * `docker-compose unpause`: Unpause all services.
  * `docker-compose unpause <service>`: Unpause a specific service.

#### 🧹 Cleanup Commands

* **Remove Containers**:
  * `docker-compose rm`: Remove stopped service containers.
  * `docker-compose rm -f`: Force remove containers even if they're running.
  * `docker-compose rm -v`: Remove containers and associated volumes.
* **System Cleanup**:
  * `docker-compose down --rmi all`: Remove all images used by any service.
  * `docker-compose down --volumes`: Remove all volumes used by services.
  * `docker-compose down --remove-orphans`: Remove containers for services not defined in the `docker-compose.yml`.

#### 📝 Configuration

* **Config Validation**:
  * `docker-compose config`: Validate and view the effective configuration.
  * `docker-compose config -q`: Validate configuration quietly.
* **Using Different Compose Files**:
  * `docker-compose -f docker-compose.prod.yml up`: Specify a different Compose file for configuration.
  * `docker-compose -f docker-compose.yml -f docker-compose.override.yml up`: Combine multiple Compose files.

#### 🔍 Debugging & Execution

* **Execute Commands**:
  * `docker-compose exec <service> bash`: Run Bash inside a service container.
  * `docker-compose exec -T <service> ls`: Execute a command without TTY.
  * `docker-compose run <service> npm test`: Run a one-off command in a new container.
* **Debug**:
  * `docker-compose events`: Stream real-time events from containers.

#### 🌟 Advanced Usage

* **Environment Variables**:
  * `docker-compose --env-file .env.prod up`: Use a specific environment file.
* **Project Name**:
  * `docker-compose -p myproject up`: Set a project name, which prefixes container names.
* **Waiting**:
  * `docker-compose up -d && docker-compose wait`: Wait for services to be ready.

#### 💡 Tips

* Use `-d` for detached mode in production environments.
* Use `--build` when you've made changes to your Dockerfile to ensure the latest changes are included.
* Use `-f` to specify different Compose files for different environments or configurations.
* Use `--remove-orphans` to clean up containers not defined in the current `docker-compose.yml`.
* Use `--no-deps` to run a single service without starting linked services.
