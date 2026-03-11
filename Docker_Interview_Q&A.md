# 1️⃣ Basic Docker Interview Questions (1–10)

### 1. What is Docker?

Docker is a **containerization platform** that allows developers to package an application with its dependencies into a **container**, ensuring it runs the same across different environments.


👉 In simple words:
Docker = Package your application + dependencies → run anywhere without environment issues.

**Why Docker is Needed**

Before Docker, applications often failed when moved between environments.

Example:

Developer machine → Works

Testing server → Fails

Production server → Different configuration

This problem is called "**It works on my machine**" problem.

Docker solves this by packaging everything the app needs.

=======================================================================================
**Docker Architecture Main Components (Docker File, Docker Image, Docker Container, Docker Hub)**

### 2. What is a Docker container?

A **container** is a lightweight, standalone executable package that includes:

* Application code
* Runtime
* Libraries
* Dependencies
* System tools

Containers share the **host OS kernel**, making them lightweight.

=================================================================================

### 3. What is a Docker image?

A **Docker image** is a **read-only template** used to create containers.

Example:

```bash
docker run nginx
```

Here **nginx image** creates a container.

====================================================================

### 4. Difference between Docker Image and Container

| Docker Image              | Docker Container          |
| ------------------------- | ------------------------- |
| Blueprint of application  | Running instance of image |
| Read-only                 | Writable                  |
| Used to create containers | Executes application      |

================================================================================

### 5. What is Docker Hub?

Docker Hub is a **cloud-based repository** where Docker images are stored and shared.

Example:

```bash
docker pull nginx
```

It downloads image from Docker Hub.

====================================================================================

### 6. What is a Dockerfile?

A **Dockerfile** is a script containing instructions to build a Docker image.

Example:

```dockerfile
FROM ubuntu
RUN apt-get update
CMD ["echo", "Hello World"]
```

========================================================================================

### 7. Difference between Virtual Machines and Containers

| Virtual Machine        | Container      |
| ---------------------- | -------------- |
| Heavy                  | Lightweight    |
| Each VM has its own OS | Shares host OS |
| Slower startup         | Fast startup   |

==========================================================================

### 8. What is the Docker Engine?

Docker Engine is the **core runtime that builds and runs containers**.

It consists of:

* Docker Daemon
* REST API
* Docker CLI

================================================================================

### 9. Docker Architecture

Docker works using a client-server architecture. It mainly consists of 4 core components:

Docker Client

Docker Host (Docker Daemon)

Docker Registry

Docker Objects (Images, Containers, Volumes, Networks)

**1. Docker Client****

The Docker Client is the interface through which users interact with Docker.

When you run commands like:

docker build
docker pull
docker run

these commands are sent to the Docker Daemon.

Example
docker run nginx

Here:

docker → client command

Request goes to Docker daemon

Daemon creates the container

👉 The client can communicate with the daemon:

On the same machine

On a remote server

**2. Docker Daemon (dockerd)**

The Docker Daemon is the core engine of Docker.

It:

Builds Docker images

Runs containers

Manages networks

Manages volumes

It listens to Docker API requests from the Docker client.

Example Flow
docker run nginx
      ↓
Docker Client
      ↓
Docker Daemon
      ↓
Creates container from nginx image


**3. Docker Registry**

A Docker Registry is a storage location where Docker images are stored.

Example registries:

Docker Hub

Amazon Elastic Container Registry

Azure Container Registry

Example Command
docker pull nginx

What happens:

Docker checks locally

If image not found

It pulls from Docker Hub

**4. Docker Objects**

Docker works using objects like:

1️⃣ Docker Images

A read-only template

Used to create containers

Example:

docker pull nginx

Image downloaded → nginx image

2️⃣ Docker Containers

A running instance of an image.

Example:

docker run nginx

Now nginx runs inside a container.

3️⃣ Docker Volumes

Used for persistent storage.

Containers are temporary, so volumes store data permanently.

Example:

docker volume create mydata
4️⃣ Docker Networks

Allow containers to communicate with each other.

Example:

App container

Database container

Both communicate via Docker network.

**Docker Architecture Diagram**

        Developer
            │
            │ Docker Commands
            ▼
      Docker Client
            │
            │ REST API
            ▼
      Docker Daemon (dockerd)
        │      │       │
        │      │       │
    Images  Containers  Networks
        │
        ▼
   Docker Registry
   (Docker Hub / ECR / ACR)


===========================================================================

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


========================================================================================


### 20. What is a multi-stage Docker build?

Multi-stage builds reduce image size by using multiple `FROM`.

Example:

```dockerfile
FROM node:18 as build
RUN npm install

FROM nginx
COPY --from=build /app /usr/share/nginx/html
```

### Problem with a Normal Dockerfile

In a **traditional Dockerfile**, we install **all tools and dependencies in one image**, including:

* Build tools (Maven, Gradle, gcc, npm, etc.)
* Source code
* Compiled application
* Runtime environment

Because of this, the **final Docker image becomes very large and insecure**.

---

## Example: Traditional Dockerfile

Suppose we build a Java application.

```dockerfile
FROM maven:3.8-openjdk-17
WORKDIR /app
COPY . .
RUN mvn package
CMD ["java","-jar","target/app.jar"]
```

### Problems with this Dockerfile

1️⃣ **Large Image Size**

* Includes:

  * Maven
  * Build dependencies
  * Source code
  * Compiled JAR

Even though Maven is **only needed during build**, it still exists in the final image.

Example:

```
Image Size ≈ 900MB
```

---

2️⃣ **Security Risk 🔐**

Unnecessary tools like:

* compilers
* package managers

remain inside the container, which **increases attack surface**.

---

3️⃣ **Slower Deployment**

Larger images take longer to:

* push to registry
* pull from registry
* deploy

---

4️⃣ **Inefficient for Production**

Production only needs:

```
Application + Runtime
```

not the **build tools**.

---

# Solution: Multi-Stage Docker Build

A **multi-stage build** uses **multiple stages in one Dockerfile**.

Each stage uses a different base image.

This allows us to:

* build the application in one stage
* copy only the final artifact to a smaller image

---

## Example: Multi-Stage Dockerfile

```dockerfile
# Stage 1: Build stage
FROM maven:3.8-openjdk-17 AS build
WORKDIR /app
COPY . .
RUN mvn package

# Stage 2: Runtime stage
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/target/app.jar app.jar
CMD ["java","-jar","app.jar"]
```

---

# How Multi-Stage Build Solves the Problem

### Step 1: Build Stage

```
maven image
   ↓
Compile code
   ↓
Generate app.jar
```

This stage contains **all build tools**.

---

### Step 2: Runtime Stage

Only copy the **final JAR file**.

```
app.jar
+ lightweight java runtime
```

Build tools **are not included**.

---

# Result

| Feature              | Normal Dockerfile | Multi-stage Dockerfile |
| -------------------- | ----------------- | ---------------------- |
| Image size           | Large             | Small                  |
| Build tools included | Yes               | No                     |
| Security             | Lower             | Higher                 |
| Deployment speed     | Slow              | Faster                 |

Example:

```
Before: 900MB
After: 200MB
```

---

# Visual Workflow

```
Stage 1 (Build)
----------------
Maven + Source Code
        ↓
    Build app.jar


Stage 2 (Production)
--------------------
Lightweight Java Image
        ↓
Copy app.jar
        ↓
Run Application
```

---

# Why DevOps Engineers Use Multi-Stage Builds 🚀

Benefits:

✔ Smaller images
✔ Faster CI/CD pipelines
✔ Better security
✔ Clean production containers

It is widely used in pipelines using tools like:

* Jenkins
* GitHub Actions
* Kubernetes



# Interview One-Line Answer

**A traditional Dockerfile creates large images because build tools and dependencies remain in the final image. Multi-stage builds solve this by separating the build and runtime stages, copying only the final artifact into a smaller production image.**



### Short Answer

❌ **No, it does NOT create two final Docker images.**
✅ It creates **only one final image**, but it **uses multiple intermediate build stages internally**.

---

## How Multi-Stage Build Actually Works

Consider this Dockerfile again:

```dockerfile
# Stage 1: Build stage
FROM maven:3.8-openjdk-17 AS build
WORKDIR /app
COPY . .
RUN mvn package

# Stage 2: Runtime stage
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/target/app.jar app.jar
CMD ["java","-jar","app.jar"]
```

### Step-by-Step Execution

#### 1️⃣ Stage 1 – Build Stage

Docker temporarily creates a **build image** using:

```
maven:3.8-openjdk-17
```

This stage:

* Copies source code
* Runs `mvn package`
* Generates `app.jar`

But this stage is **only used for building**.

---

#### 2️⃣ Stage 2 – Runtime Stage

Docker starts a **new stage** using:

```
openjdk:17-jdk-slim
```

Then this command copies the artifact:

```bash
COPY --from=build /app/target/app.jar app.jar
```

So only the **JAR file** is copied from the build stage.

---

## Final Result

After the build completes:

```
Final Image
-----------
openjdk:17-jdk-slim
+ app.jar
```

The **Maven image and build tools are discarded**.

---

## Visualization

```
Stage 1 (Temporary)
-------------------
maven image
+ source code
+ build tools
        │
        │ creates
        ▼
     app.jar
        │
        │ copied
        ▼
Stage 2 (Final Image)
---------------------
openjdk slim
+ app.jar
```

---

## What Happens Internally

Docker internally creates:

```
Intermediate image (build stage)
Final image (runtime stage)
```

But when you check images:

```bash
docker images
```

You will typically see **only the final tagged image**.

---

## Example

Build command:

```bash
docker build -t myapp .
```

Output image list:

```
REPOSITORY   TAG       SIZE
myapp        latest    180MB
```

You **won’t see the Maven build stage as a final image**.

---

## Exception (Advanced)

If you explicitly build a specific stage:

```bash
docker build --target build -t myapp-build .
```

Then Docker **will create an image for that stage**.

---

✅ **Interview Answer**

> Multi-stage Docker builds do not create multiple final images. They create temporary intermediate stages during the build process, but only the last stage becomes the final Docker image unless a specific stage is explicitly targeted.


**Multi-Stage Build vs Docker Layer Caching (why Docker builds become faster)** — this is asked very frequently in **DevOps interviews.**


After writing the **multi-stage Dockerfile**, the process is the same as any normal Docker build:

1️⃣ Build the image
2️⃣ Tag the image
3️⃣ Login to registry
4️⃣ Push the image

I'll show a **complete example using** Docker Hub.

---

# 1️⃣ Build the Docker Image

Navigate to the folder where the **Dockerfile** exists and run:

```bash
docker build -t myapp:1.0 .
```

### Explanation

| Part           | Meaning                                 |
| -------------- | --------------------------------------- |
| `docker build` | Builds a Docker image                   |
| `-t`           | Tag name                                |
| `myapp`        | Image name                              |
| `1.0`          | Image version                           |
| `.`            | Current directory containing Dockerfile |

After this:

```bash
docker images
```

Example output:

```
REPOSITORY   TAG    SIZE
myapp        1.0    180MB
```

---

# 2️⃣ Tag the Image for Docker Registry

Before pushing, the image must include your **Docker Hub username**.

```bash
docker tag myapp:1.0 <dockerhub-username>/myapp:1.0
```

Example:

```bash
docker tag myapp:1.0 kamalinty/myapp:1.0
```

Now check:

```bash
docker images
```

```
REPOSITORY           TAG
kamalinty/myapp      1.0
myapp                1.0
```

---

# 3️⃣ Login to Docker Registry

Login to **Docker Hub**.

```bash
docker login
```

It will ask:

```
Username
Password
```

---

# 4️⃣ Push Image to Docker Registry

Now push the image.

```bash
docker push <dockerhub-username>/myapp:1.0
```

Example:

```bash
docker push kamalinty/myapp:1.0
```

Now the image is available in Docker Hub.

---

# 5️⃣ Verify in Docker Hub

Go to:

[https://hub.docker.com](https://hub.docker.com)

You will see:

```
Repository → kamalinty/myapp
Tag → 1.0
```

---

# 6️⃣ Pull Image from Another Server

On another machine:

```bash
docker pull kamalinty/myapp:1.0
```

Run container:

```bash
docker run -p 8080:8080 kamalinty/myapp:1.0
```

---

# Complete Flow (DevOps Pipeline)

```
Dockerfile
     ↓
docker build
     ↓
docker tag
     ↓
docker login
     ↓
docker push
     ↓
Docker Registry
```

Registries commonly used:

* Docker Hub
* Amazon Elastic Container Registry
* Azure Container Registry

---

✅ **Short Interview Answer**

```
docker build -t username/app:version .
docker login
docker push username/app:version
```

---




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
