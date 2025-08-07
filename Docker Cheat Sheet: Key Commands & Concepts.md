# 🚀 **Docker Cheat Sheet: Key Commands & Concepts**

---

## 🐳 Container Lifecycle

|Command|Description|
|---|---|
|`docker build -t name:tag .`|Build image from Dockerfile|
|`docker run -d --name name image`|Run container in background|
|`docker ps`|List running containers|
|`docker ps -a`|List all containers (running + stopped)|
|`docker stop <container>`|Stop a running container|
|`docker rm <container>`|Remove container|
|`docker rmi <image>`|Remove image|

---

## 🔧 Working Inside Containers

|Command|Description|
|---|---|
|`docker exec -it <container> bash`|Open interactive bash shell inside container|
|`docker logs <container>`|View container logs|
|`docker logs -f <container>`|Follow container logs live|

---

## 🖼️ Images & Tags

|Command|Description|
|---|---|
|`docker images`|List images|
|`docker tag source target`|Tag image for repository or version|
|`docker push user/repo:tag`|Push image to Docker Hub|
|`docker pull user/repo:tag`|Pull image from Docker Hub|

---

## 📁 Dockerfile & Builds

|Directive|Description|
|---|---|
|`FROM`|Base image|
|`COPY` / `ADD`|Copy files into image|
|`RUN`|Run commands during build|
|`CMD`|Default command to run on container start|
|`ENTRYPOINT`|Entrypoint command for container|
|`WORKDIR`|Set working directory|
|`ENV`|Set environment variables|
|`HEALTHCHECK`|Define health check command|
|`.dockerignore`|Files to exclude from build context|

---

## 🛠️ Docker Compose

|Command|Description|
|---|---|
|`docker-compose up`|Build and start services defined in `docker-compose.yml`|
|`docker-compose down`|Stop and remove containers, networks|
|`docker-compose logs`|View logs for all services|
|`docker-compose exec service bash`|Open shell in a service container|

---

## 🌐 Networking

|Concept|Description|
|---|---|
|`bridge` network|Default network, containers communicate via IPs|
|Custom bridge network|Allows containers to resolve names (`db`, `web`)|
|`docker network create <name>`|Create custom network|
|`--network <name>`|Run container on specified network|
|`docker network inspect <name>`|Inspect network details|

---

## 📦 Docker Hub

|Command|Description|
|---|---|
|`docker login`|Log into Docker Hub|
|`docker push user/repo:tag`|Push image to Docker Hub|
|`docker pull user/repo:tag`|Pull image from Docker Hub|

---

## 🔍 Debugging & Monitoring

|Command|Description|
|---|---|
|`docker logs <container>`|View logs|
|`docker exec -it <container> bash`|Exec shell inside container|
|`docker inspect <container>`|Show detailed container info|
|`docker stats`|Show live resource usage stats|
|`docker events`|Monitor Docker daemon events|

---

## ✅ Tips & Best Practices

- Use `.dockerignore` to speed builds and reduce image size.
    
- Use **multi-stage builds** to create lean production images.
    
- Use **healthchecks** to detect and restart unhealthy containers.
    
- Tag images with versions, not just `latest`.
    
- Automate builds and pushes using CI/CD pipelines.
    
- Use custom Docker networks for service name resolution.
    
- Always clean up unused images/containers with `docker system prune`.