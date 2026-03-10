Here are **30 Docker interview questions** commonly asked in **DevOps interviews**. I arranged them from **basic → intermediate → advanced**, which is helpful for someone learning DevOps like you.

---

# 1️⃣ Basic Docker Interview Questions (1–10)

### 1. What is Docker?

Docker is a **containerization platform** that allows developers to package an application with its dependencies into a **container**, ensuring it runs the same across different environments.

---

### 2. What is a Docker container?

A **container** is a lightweight, standalone executable package that includes:

* Application code
* Runtime
* Libraries
* Dependencies
* System tools

Containers share the **host OS kernel**, making them lightweight.

---

### 3. What is a Docker image?

A **Docker image** is a **read-only template** used to create containers.

Example:

```bash
docker run nginx
```

Here **nginx image** creates a container.

---

### 4. Difference between Docker Image and Container

| Docker Image              | Docker Container          |
| ------------------------- | ------------------------- |
| Blueprint of application  | Running instance of image |
| Read-only                 | Writable                  |
| Used to create containers | Executes application      |

---

### 5. What is Docker Hub?

Docker Hub is a **cloud-based repository** where Docker images are stored and shared.

Example:

```bash
docker pull nginx
```

It downloads image from Docker Hub.

---

### 6. What is a Dockerfile?

A **Dockerfile** is a script containing instructions to build a Docker image.

Example:

```dockerfile
FROM ubuntu
RUN apt-get update
CMD ["echo", "Hello World"]
```

---

### 7. Difference between Virtual Machines and Containers

| Virtual Machine        | Container      |
| ---------------------- | -------------- |
| Heavy                  | Lightweight    |
| Each VM has its own OS | Shares host OS |
| Slower startup         | Fast startup   |

---

### 8. What is the Docker Engine?

Docker Engine is the **core runtime that builds and runs containers**.

It consists of:

* Docker Daemon
* REST API
* Docker CLI

---

### 9. What is the Docker daemon?

Docker daemon (`dockerd`) is the background service that manages:

* Images
* Containers
* Networks
* Volumes

---

### 10. What command is used to check Docker version?

```bash
docker --version
```

---

# 2️⃣ Docker Commands Interview Questions (11–15)

### 11. How do you list running containers?

```bash
docker ps
```

---

### 12. How do you list all containers?

```bash
docker ps -a
```

---

### 13. How do you stop a container?

```bash
docker stop <container_id>
```

---

### 14. How do you remove a container?

```bash
docker rm <container_id>
```

---

### 15. How do you remove an image?

```bash
docker rmi <image_id>
```

---

# 3️⃣ Dockerfile Interview Questions (16–20)

### 16. What are common Dockerfile instructions?

Common instructions:

* `FROM`
* `RUN`
* `CMD`
* `COPY`
* `ADD`
* `WORKDIR`
* `EXPOSE`
* `ENV`

Example:

```dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```

---

### 17. Difference between RUN, CMD, and ENTRYPOINT

| Instruction | Purpose                       |
| ----------- | ----------------------------- |
| RUN         | Executes during image build   |
| CMD         | Default command for container |
| ENTRYPOINT  | Main executable               |

---

### 18. What is the difference between ADD and COPY?

| ADD                   | COPY             |
| --------------------- | ---------------- |
| Can download URLs     | Only local files |
| Can extract tar files | Cannot extract   |

---

### 19. What is Docker layer caching?

Each Dockerfile instruction creates a **layer**.

If nothing changes, Docker reuses previous layers to **speed up builds**.

---

### 20. What is a multi-stage Docker build?

Multi-stage builds reduce image size by using multiple `FROM`.

Example:

```dockerfile
FROM node:18 as build
RUN npm install

FROM nginx
COPY --from=build /app /usr/share/nginx/html
```

---

# 4️⃣ Docker Networking Questions (21–24)

### 21. What are different Docker network types?

Types:

* Bridge
* Host
* None
* Overlay
* Macvlan

---

### 22. What is bridge network?

Default network used by Docker containers on a single host.

Containers communicate via **internal IPs**.

---

### 23. What is host network?

Container shares **host network directly**.

No network isolation.

---

### 24. What is overlay network?

Overlay network connects containers across **multiple Docker hosts**, often used in **Docker Swarm**.

---

# 5️⃣ Docker Storage Questions (25–27)

### 25. What are Docker volumes?

Volumes are used for **persistent data storage**.

Example:

```bash
docker volume create myvolume
```

---

### 26. Difference between bind mount and volume

| Volume                     | Bind Mount           |
| -------------------------- | -------------------- |
| Managed by Docker          | Managed by host      |
| Stored in Docker directory | Uses host filesystem |

---

### 27. Why are volumes important?

Because containers are **ephemeral** (data lost when container is removed).
Volumes persist data.

---

# 6️⃣ Advanced Docker Interview Questions (28–30)

### 28. What is Docker Compose?

Docker Compose is a tool used to **define and run multi-container applications**.

Example `docker-compose.yml`

```yaml
version: "3"
services:
  web:
    image: nginx
    ports:
      - "80:80"
```

Run with:

```bash
docker-compose up
```

---

### 29. What is the difference between Docker and Kubernetes?

| Docker            | Kubernetes              |
| ----------------- | ----------------------- |
| Container runtime | Container orchestration |
| Runs containers   | Manages clusters        |

---

### 30. How do you optimize Docker images?

Best practices:

* Use **small base images** (Alpine)
* Use **multi-stage builds**
* Reduce number of layers
* Remove unnecessary packages
* Use `.dockerignore`

---

# ⭐ 5 Most Important Docker Interview Questions

These are **frequently asked in DevOps interviews**:

1️⃣ What is Docker and why is it used?
2️⃣ Difference between **Docker image and container**
3️⃣ Difference between **VM and Container**
4️⃣ What is a **Dockerfile**?
5️⃣ Difference between **CMD and ENTRYPOINT**

---

✅ If you want, I can also give **30 Kubernetes interview questions** (very important for DevOps interviews).
