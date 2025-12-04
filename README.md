# 🐳 Docker Commands Cheat Sheet

A comprehensive reference guide for essential Docker commands organized by category.

---

## 🚀 1. Container Lifecycle

Commands to create, run, inspect, and remove containers.

### Run Containers
```bash
docker run -it --name <name> <image> [command]                          # Run interactively
docker run -d -p <host_port>:<container_port> --name <name> <image>     # Detached with port mapping
```

### Container Status
```bash
docker ps                   # Running containers
docker ps -a                # All containers
```

### Start / Stop / Restart
```bash
docker start <container>
docker stop <container>
docker restart <container>
```

### Logs
```bash
docker logs <container>
docker logs -f <container>  # Follow logs
```

### Exec Into Container
```bash
docker exec -it <container> /bin/bash
docker exec -it <container> sh
```

### Attach & Rename
```bash
docker attach <container>
docker rename <old_name> <new_name>
```

### Kill / Remove
```bash
docker kill <container>
docker rm <container>
docker rm -f <container>    # Force remove running container
```

---

## 📦 2. Image Management

Commands to build, pull, inspect, and remove Docker images.

### List & Pull
```bash
docker images
docker image ls
docker pull <image>[:tag]
```

### Build & Commit
```bash
docker build -t <name>:<tag> .
docker commit <container> <image_name>:<tag>
```

### Tag & Push
```bash
docker tag <source_image>:<tag> <target_image>:<tag>
docker push <image>:<tag>
```

### Inspect & History
```bash
docker inspect <image_or_container>
docker history <image>
```

### Remove / Save / Load
```bash
docker rmi <image>
docker image rm <image>
docker save -o <file>.tar <image>:<tag>
docker load -i <file>.tar
```

---

## 📁 3. Volumes & Networks

Used for persistent data and container communication.

### 🗄️ Volumes
```bash
docker volume ls
docker volume create <name>
docker volume inspect <name>
docker volume rm <name>
```

**Mounts:**
```bash
docker run -v <host_path>:<container_path> <image>       # Bind mount
docker run -v <volume_name>:<container_path> <image>     # Named volume
```

### 🌐 Networks
```bash
docker network ls
docker network create <name>
docker network inspect <name>
docker network connect <network> <container>
docker network disconnect <network> <container>
docker network rm <name>
```

---

## 🧹 4. Cleanup Commands

Remove unused containers, images, networks, and volumes.
```bash
docker system df
docker system prune
docker system prune -a      # Removes unused images too
docker container prune
docker image prune
docker volume prune
docker network prune
```

---

## 🧱 5. Docker Compose (v2)

Commands for multi-container applications using `docker-compose.yml`.
```bash
docker compose up               # Start services
docker compose up -d            # Detached mode
docker compose down             # Stop & remove resources
docker compose ps               # List containers
docker compose logs -f          # Follow logs
docker compose build            # Build or rebuild services
docker compose exec <service> sh
```

---

## 📝 Notes

- Replace `<container>`, `<image>`, `<name>`, etc. with actual values
- Use `-f` flag to force operations when needed
- Always check running containers before cleanup operations

---

## 🤝 Contributing

Feel free to submit issues or pull requests to improve this cheat sheet!

## 📄 License

Open source - feel free to use and modify as needed.
