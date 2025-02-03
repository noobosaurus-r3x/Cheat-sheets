# Docker Cheat Sheet  
*Containerization Made Simple*  

---

## **1. Core Concepts**  
- **Image**: Immutable template with app code, dependencies, and config.  
- **Container**: Isolated runtime instance of an image.  
- **Dockerfile**: Blueprint to automate image builds.  
- **Registry**: Hub for storing/sharing images (e.g., Docker Hub, ECR).  
- **Volume**: Persistent storage for containers.  
- **Network**: Communication channel between containers/host.  

---

## **2. Installation**  

### **Linux**  
```bash  
curl -fsSL https://get.docker.com | sh  
sudo usermod -aG docker $(whoami) && newgrp docker  
```  

### **macOS/Windows**  
- Install [Docker Desktop](https://www.docker.com/products/docker-desktop).  

### **Verify**  
```bash  
docker version          # Check client/server versions  
docker run hello-world  # Run a test container  
```  

---

## **3. Image Management**  

| **Command**                          | **Description**                                   |  
|--------------------------------------|---------------------------------------------------|  
| `docker images`                      | List local images                                 |  
| `docker pull <image>:<tag>`          | Download image from registry                      |  
| `docker rmi <image>`                 | Remove image                                      |  
| `docker build -t <name>:<tag> .`     | Build image from Dockerfile                       |  
| `docker image prune -a`              | Delete unused images                              |  

**Example**:  
```bash  
docker pull nginx:alpine  # Lightweight Nginx image  
docker build -t myapp:1.0 .  # Build from current directory's Dockerfile  
```  

**Note**:  
- Use `:<tag>` (e.g., `:latest`, `:alpine`) to specify image versions.  
- `docker image prune -a` removes **all** unused images, not just dangling ones.  

---

## **4. Container Lifecycle**  

| **Command**                          | **Description**                                   |  
|--------------------------------------|---------------------------------------------------|  
| `docker run -d --name <name> <image>`| Run container in background                       |  
| `docker start/stop/restart <name>`   | Manage container state                            |  
| `docker rm -f <name>`                | Force remove running container                    |  
| `docker exec -it <name> sh`          | Open interactive shell                            |  
| `docker logs -f <name>`              | Tail container logs                               |  
| `docker ps -a`                       | List all containers (including stopped)           |  

**Common Flags**:  
- `-p 8080:80`: Map host port 8080 → container port 80.  
- `-v /host/path:/container/path`: Bind mount volume.  
- `-e VAR=value`: Set environment variable.  
- `--network mynet`: Attach to custom network.  

**Example**:  
```bash  
docker run -d --name webserver -p 8080:80 -v ./html:/usr/share/nginx/html nginx:alpine  
```  

**Explanation**:  
- `-d`: Run in detached mode (background).  
- `-v ./html:/usr/share/nginx/html`: Mount the local `html` folder into the container’s Nginx directory.  

---

## **5. Dockerfile Essentials**  

### **Sample Dockerfile**  
```dockerfile  
FROM python:3.9-slim                 # Base image  
WORKDIR /app                         # Set working directory  
COPY requirements.txt .              # Copy dependencies file  
RUN pip install --no-cache-dir -r requirements.txt  # Install deps  
COPY . .                             # Copy app code  
EXPOSE 5000                          # Declare port  
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]  # Runtime command  
```  

### **Multi-Stage Build**  
```dockerfile  
# Build stage (heavy, includes build tools)  
FROM node:18 as builder  
WORKDIR /app  
COPY package*.json ./  
RUN npm install  
COPY . .  
RUN npm run build  # Compile app  

# Production stage (lightweight, only necessary files)  
FROM nginx:alpine  
COPY --from=builder /app/dist /usr/share/nginx/html  # Copy built files  
```  

**Build & Run**:  
```bash  
docker build -t myapp:1.0 .  
docker run -d -p 80:80 myapp:1.0  
```  

**Note**:  
- Multi-stage builds reduce image size by discarding build tools in the final image.  
- `--from=builder` copies files from the earlier build stage.  

---

## **6. Networking**  

| **Command**                          | **Description**                                   |  
|--------------------------------------|---------------------------------------------------|  
| `docker network ls`                  | List networks                                     |  
| `docker network create mynet`        | Create custom network                             |  
| `docker network inspect mynet`       | Show network details                              |  

**Example**:  
```bash  
docker network create app_network  
docker run -d --network app_network --name frontend nginx  
docker run -d --network app_network --name backend api:latest  
```  

**Explanation**:  
- Containers on the same network can communicate using their service names (e.g., `frontend` can ping `backend`).  

---

## **7. Volumes & Storage**  

| **Command**                          | **Description**                                   |  
|--------------------------------------|---------------------------------------------------|  
| `docker volume create myvol`         | Create named volume                               |  
| `docker volume ls`                   | List volumes                                      |  
| `docker run -v myvol:/data ...`      | Mount volume                                      |  

**Example**:  
```bash  
docker run -d --name db -v pgdata:/var/lib/postgresql/data postgres:15  
```  

**Note**:  
- Volumes persist data even if the container is deleted.  
- Use `-v pgdata:/var/lib/postgresql/data` to store PostgreSQL data permanently.  

---

## **8. Docker Compose**  

### **Sample `docker-compose.yml`**  
```yaml  
version: '3.8'  
services:  
  web:  
    image: nginx:alpine  
    ports:  
      - "80:80"  
    volumes:  
      - ./html:/usr/share/nginx/html  # Bind mount for static files  
  db:  
    image: postgres:15  
    environment:  
      POSTGRES_PASSWORD: secret  # Set DB password  
    volumes:  
      - pgdata:/var/lib/postgresql/data  # Persistent volume  

volumes:  
  pgdata:  # Define named volume  
```  

| **Command**                          | **Description**                                   |  
|--------------------------------------|---------------------------------------------------|  
| `docker compose up -d`               | Start services in background                      |  
| `docker compose down`                | Stop and remove containers/volumes                |  
| `docker compose logs -f`             | Follow service logs                               |  

**Explanation**:  
- `docker compose up -d` reads the `docker-compose.yml` file and starts all defined services.  
- Named volumes (e.g., `pgdata`) are managed automatically.  

---

## **9. Advanced Techniques**  

### **Resource Limits**  
```bash  
docker run -d --name app --memory=512m --cpus=1.5 myapp:1.0  
```  
**Note**:  
- Restrict memory (`--memory`) and CPU (`--cpus`) to prevent a container from hogging resources.  

### **Health Checks**  
```dockerfile  
HEALTHCHECK --interval=30s --timeout=3s \  
  CMD curl -f http://localhost:5000/health || exit 1  
```  
**Note**:  
- Docker periodically runs this check to verify the app is healthy.  
- Use `docker inspect <container>` to see health status.  

### **Security Best Practices**  
1. Avoid `root` user in containers:  
   ```dockerfile  
   FROM node:18  
   USER node  # Run as non-root "node" user  
   ```  
2. Scan for vulnerabilities:  
   ```bash  
   docker scan myapp:1.0  # Uses Snyk to find CVEs  
   ```  

---

## **10. Maintenance & Optimization**  

| **Command**                          | **Description**                                   |  
|--------------------------------------|---------------------------------------------------|  
| `docker system prune -a --volumes`   | Remove unused images/containers/volumes           |  
| `docker stats`                       | Live resource usage monitor                       |  
| `docker history <image>`             | Inspect image layers                              |  

**Tips**:  
- Use `.dockerignore` to exclude unnecessary files (e.g., `node_modules`, `.git`).  
- Tag images semantically: `myapp:1.0-prod`.  

---

## **11. Troubleshooting**  

| **Issue**                | **Solution**                                      |  
|--------------------------|---------------------------------------------------|  
| Port conflicts           | Check running containers: `docker ps`             |  
| Permission denied        | Use `-v $(pwd):/data:ro` for read-only mounts     |  
| Container won’t start    | Inspect logs: `docker logs <name>`                |  
| Image too large          | Use multi-stage builds and Alpine-based images    |  

---

## **12. References**  
- [Docker Documentation](https://docs.docker.com/)  
- [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)  

---
