---
icon: sheet-plastic
---

# Docker Compose Command Cheat Sheet

### 🚀 Basic Commands

```bash
# Start containers
docker-compose up                # Start all services
docker-compose up -d             # Start in detached mode
docker-compose up --build        # Build images before starting
docker-compose up <service>      # Start specific service

# Stop containers
docker-compose down              # Stop and remove containers
docker-compose down -v           # Stop and remove containers + volumes
docker-compose stop              # Stop containers without removing
```

***

### 🏗️ Build Commands

```bash
docker-compose build                 # Build all services
docker-compose build <service>       # Build specific service
docker-compose build --no-cache      # Build without cache
docker-compose build --pull          # Always pull newer images
```

***

### 📊 Status & Information

```bash
docker-compose ps                    # List all containers
docker-compose ps <service>          # List specific service
docker-compose top                   # Display running processes
docker-compose port <service> <port> # Show port mapping

# Logs
docker-compose logs                  # View output from containers
docker-compose logs -f               # Follow log output
docker-compose logs <service>        # View specific service logs
docker-compose logs --tail=100       # Show last 100 lines
```

***

### 🔄 Runtime Management

```bash
# Restart services
docker-compose restart               # Restart all services
docker-compose restart <service>     # Restart specific service

# Scale services
docker-compose scale <service>=<num> # Scale service to specified number of instances

# Pause/Unpause
docker-compose pause                 # Pause all services
docker-compose pause <service>       # Pause specific service
docker-compose unpause               # Unpause all services
docker-compose unpause <service>     # Unpause specific service
```

***

### 🧹 Cleanup Commands

```bash
# Remove containers
docker-compose rm                    # Remove stopped containers
docker-compose rm -f                 # Force remove containers
docker-compose rm -v                 # Remove containers and volumes

# System cleanup
docker-compose down --rmi all        # Remove all images
docker-compose down --volumes        # Remove volumes
docker-compose down --remove-orphans # Remove orphaned containers
```

***

### 📝 Configuration

```bash
# Config validation
docker-compose config                # Validate and view compose file
docker-compose config -q             # Quiet mode, validate only

# Using different compose files
docker-compose -f docker-compose.prod.yml up    # Specify compose file
docker-compose -f docker-compose.yml -f docker-compose.override.yml up
```

***

### 🔍 Debugging & Execution

```bash
# Execute commands
docker-compose exec <service> bash   # Run bash in container
docker-compose exec -T <service> ls  # Run command without TTY
docker-compose run <service> npm test # Run one-off command

# Debug
docker-compose events                # Stream events
```

***

### 🌟 Advanced Usage

```bash
# Environment variables
docker-compose --env-file .env.prod up    # Specify env file

# Project name
docker-compose -p myproject up            # Set project name

# Waiting
docker-compose up -d && docker-compose wait  # Wait for services
```

***

### 💡 Tips

* Use `-d` for detached mode in production.
* Use `--build` when Dockerfile changes.
* Use `-f` to specify different compose files.
* Use `--remove-orphans` to clean up unused containers.
* Use `--no-deps` to run a single service without dependencies.
