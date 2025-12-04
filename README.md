# 🐳 Docker Commands Cheat Sheet
A comprehensive reference guide for essential Docker commands organized by category with practical use cases.

---

## 🚀 1. Container Lifecycle
Commands to create, run, inspect, and remove containers.

### Run Containers
```bash
# Run interactively - Use for: Debugging, testing, development shells
docker run -it --name my-ubuntu ubuntu /bin/bash

# Detached with port mapping - Use for: Running web servers, APIs, databases
docker run -d -p 8080:80 --name my-nginx nginx

# With environment variables - Use for: Passing configuration to apps
docker run -d -e MYSQL_ROOT_PASSWORD=secret -p 3306:3306 --name my-db mysql

# With volume mount - Use for: Persisting data, sharing files with host
docker run -d -v /host/data:/var/lib/mysql -p 3306:3306 mysql
```

### Container Status
```bash
# Running containers - Use for: Quick status check of active services
docker ps

# All containers - Use for: Finding stopped containers, cleanup audits
docker ps -a

# With formatting - Use for: Custom output, scripting, monitoring
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
```

### Start / Stop / Restart
```bash
# Start - Use for: Resuming stopped containers without recreating them
docker start my-container

# Stop - Use for: Graceful shutdown (sends SIGTERM then SIGKILL)
docker stop my-container

# Restart - Use for: Applying configuration changes, fixing hung containers
docker restart my-container
```

### Logs
```bash
# View logs - Use for: Debugging errors, checking application output
docker logs my-container

# Follow logs - Use for: Real-time monitoring, watching deployments
docker logs -f my-container

# Last N lines - Use for: Quick recent error checks
docker logs --tail 50 my-container

# With timestamps - Use for: Correlating events, debugging time-sensitive issues
docker logs -t my-container
```

### Exec Into Container
```bash
# Bash shell - Use for: Debugging, inspecting files, running commands
docker exec -it my-container /bin/bash

# Sh shell - Use for: Alpine or minimal images without bash
docker exec -it my-alpine sh

# Run single command - Use for: Quick checks, database queries, file operations
docker exec my-container ls /app
docker exec my-db mysql -u root -p -e "SHOW DATABASES;"
```

### Attach & Rename
```bash
# Attach - Use for: Viewing live output, interacting with foreground process
docker attach my-container

# Rename - Use for: Fixing naming mistakes, improving organization
docker rename old-name new-descriptive-name
```

### Kill / Remove
```bash
# Kill - Use for: Force stopping unresponsive containers (sends SIGKILL)
docker kill my-hung-container

# Remove - Use for: Cleaning up stopped containers
docker rm my-container

# Force remove running - Use for: Emergency cleanup, stuck containers
docker rm -f my-container

# Remove all stopped - Use for: Bulk cleanup after testing
docker rm $(docker ps -a -q -f status=exited)
```

---

## 📦 2. Image Management
Commands to build, pull, inspect, and remove Docker images.

### List & Pull
```bash
# List images - Use for: Checking what's available locally, disk usage audit
docker images

# Detailed list - Use for: Seeing image IDs, creation dates, sizes
docker image ls

# Pull specific version - Use for: Getting exact image version for reproducibility
docker pull nginx:1.24-alpine

# Pull all tags - Use for: Getting multiple versions for testing
docker pull --all-tags alpine
```

### Build & Commit
```bash
# Build from Dockerfile - Use for: Creating custom images for your applications
docker build -t myapp:v1.0 .

# Build with build args - Use for: Parameterized builds, CI/CD pipelines
docker build --build-arg VERSION=1.0 -t myapp:v1.0 .

# Build without cache - Use for: Fresh builds after dependency updates
docker build --no-cache -t myapp:v1.0 .

# Commit container changes - Use for: Creating image from modified container (not recommended for production)
docker commit my-container myapp:debug-version
```

### Tag & Push
```bash
# Tag for registry - Use for: Preparing images for Docker Hub or private registry
docker tag myapp:v1.0 username/myapp:v1.0

# Tag as latest - Use for: Marking stable release
docker tag myapp:v1.0 username/myapp:latest

# Push to registry - Use for: Sharing images, deployment to servers
docker push username/myapp:v1.0

# Push to private registry - Use for: Corporate environments, self-hosted registries
docker tag myapp:v1.0 registry.company.com/myapp:v1.0
docker push registry.company.com/myapp:v1.0
```

### Inspect & History
```bash
# Inspect - Use for: Viewing configuration, debugging networking, checking volumes
docker inspect my-container

# Get specific field - Use for: Scripting, extracting IP address or other details
docker inspect -f '{{.NetworkSettings.IPAddress}}' my-container

# Image history - Use for: Understanding image layers, optimizing Dockerfile
docker history nginx:alpine
```

### Remove / Save / Load
```bash
# Remove image - Use for: Freeing disk space, cleaning old versions
docker rmi myapp:old-version

# Force remove - Use for: Removing images with multiple tags or in use
docker image rm -f myapp:v1.0

# Save to tar - Use for: Offline transfer, backups, air-gapped systems
docker save -o myapp.tar myapp:v1.0

# Load from tar - Use for: Restoring backups, importing to different machine
docker load -i myapp.tar

# Export container filesystem - Use for: Creating rootfs, migration
docker export my-container > container-backup.tar
```

---

## 📁 3. Volumes & Networks
Used for persistent data and container communication.

### 🗄️ Volumes
```bash
# List volumes - Use for: Audit data storage, finding orphaned volumes
docker volume ls

# Create named volume - Use for: Database data, shared application state
docker volume create postgres-data

# Inspect volume - Use for: Finding mount point, checking driver options
docker volume inspect postgres-data

# Remove volume - Use for: Cleanup, freeing disk space (careful with data!)
docker volume rm postgres-data

# Remove unused volumes - Use for: Bulk cleanup of orphaned volumes
docker volume prune
```

**Mounts:**
```bash
# Bind mount - Use for: Development (live code updates), sharing config files
docker run -v /home/user/app:/app myapp

# Named volume - Use for: Production databases, persistent application data
docker run -v postgres-data:/var/lib/postgresql/data postgres

# Read-only mount - Use for: Sharing static files, preventing accidental modifications
docker run -v /host/config:/etc/config:ro myapp

# tmpfs mount - Use for: Sensitive data, temporary caches
docker run --tmpfs /app/tmp myapp
```

### 🌐 Networks
```bash
# List networks - Use for: Understanding container connectivity, troubleshooting
docker network ls

# Create custom network - Use for: Isolating application stacks, DNS resolution
docker network create my-app-network

# Create with subnet - Use for: Specific IP requirements, network planning
docker network create --subnet=172.18.0.0/16 my-network

# Inspect network - Use for: Seeing connected containers, IP assignments
docker network inspect my-app-network

# Connect container - Use for: Adding container to network after creation
docker network connect my-app-network my-container

# Disconnect container - Use for: Isolating container, network changes
docker network disconnect my-app-network my-container

# Remove network - Use for: Cleanup after testing, reconfiguring
docker network rm my-app-network
```

---

## 🧹 4. Cleanup Commands
Remove unused containers, images, networks, and volumes.

```bash
# Check disk usage - Use for: Monitoring storage, planning cleanup
docker system df

# System prune - Use for: Weekly cleanup, freeing space safely (keeps tagged images)
docker system prune

# Aggressive prune - Use for: Major cleanup, freeing maximum space (removes all unused images)
docker system prune -a

# Prune with time filter - Use for: Removing old resources, keeping recent ones
docker system prune -a --filter "until=168h"  # Older than 7 days

# Container prune - Use for: Removing only stopped containers
docker container prune

# Image prune - Use for: Removing dangling images (untagged intermediate layers)
docker image prune

# Volume prune - Use for: Removing volumes not used by any container (CAREFUL!)
docker volume prune

# Network prune - Use for: Removing networks not used by any container
docker network prune
```

---

## 🧱 5. Docker Compose (v2)
Commands for multi-container applications using `docker-compose.yml`.

```bash
# Start services - Use for: Development, launching entire application stack
docker compose up

# Detached mode - Use for: Background services, production deployments
docker compose up -d

# Build and start - Use for: After Dockerfile changes, CI/CD pipelines
docker compose up --build

# Start specific service - Use for: Testing individual services, debugging
docker compose up db redis

# Stop & remove - Use for: Cleanup, stopping development environment
docker compose down

# Down with volumes - Use for: Fresh start, removing all data (CAREFUL!)
docker compose down -v

# List containers - Use for: Checking service status, finding container names
docker compose ps

# Follow logs - Use for: Real-time monitoring, debugging all services
docker compose logs -f

# Follow specific service - Use for: Debugging individual service issues
docker compose logs -f web

# Build services - Use for: Rebuilding after code changes without starting
docker compose build

# No-cache build - Use for: Fresh builds after dependency updates
docker compose build --no-cache

# Exec into service - Use for: Running commands in service containers
docker compose exec web /bin/bash

# Run one-off command - Use for: Database migrations, running scripts
docker compose run web python manage.py migrate

# Scale services - Use for: Load testing, horizontal scaling
docker compose up -d --scale web=3

# View config - Use for: Validating compose file, troubleshooting
docker compose config

# Restart services - Use for: Applying configuration changes
docker compose restart

# Pull images - Use for: Updating to latest images before starting
docker compose pull
```

---

## 🎯 Common Use Case Workflows

### Development Workflow
```bash
# 1. Start development environment
docker compose up -d

# 2. Watch logs during development
docker compose logs -f web

# 3. Run tests
docker compose exec web npm test

# 4. Stop when done
docker compose down
```

### Database Management
```bash
# 1. Start database with persistent volume
docker run -d -v db-data:/var/lib/postgresql/data -p 5432:5432 postgres

# 2. Create backup
docker exec my-db pg_dump -U postgres mydb > backup.sql

# 3. Restore backup
docker exec -i my-db psql -U postgres mydb < backup.sql
```

### Production Deployment
```bash
# 1. Pull latest images
docker compose pull

# 2. Start with restart policy
docker compose up -d --remove-orphans

# 3. Check status
docker compose ps

# 4. Monitor logs
docker compose logs -f --tail=100
```

---

## 📝 Notes
- Replace `<container>`, `<image>`, `<name>`, etc. with actual values
- Use `-f` flag to force operations when needed
- Always check running containers before cleanup operations
- Use `--help` with any command for detailed options
- Named volumes are preferred over bind mounts for production databases

---
