# 🐳 Containerizing a Node.js App Using GitHub Copilot

> **Role:** DevOps Engineer  
> **Goal:** Use GitHub Copilot to generate, understand, build, and run a Dockerized Node.js application.

---

## 🎯 Objectives

By the end of this lab, you will be able to:

- Generate a `Dockerfile` using GitHub Copilot
- Understand each Dockerfile instruction
- Build and run a containerized Node.js application
- Enhance the Dockerfile with `.dockerignore`, multi-stage builds, and environment variables

---

## ✅ Prerequisites

| Requirement | Details |
|---|---|
| VS Code | Installed and running |
| GitHub Copilot | Extension enabled |
| Docker | Installed and Docker Desktop running |
| Node.js Knowledge | Basic understanding |

---

## 📁 Step 1: Create the Sample Application

### 1.1 — Create the project folder

```bash
mkdir copilot-docker-lab
cd copilot-docker-lab
```

### 1.2 — Create `app.js`

```js
const http = require('http');

const hostname = '0.0.0.0';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello from Copilot Docker Lab!\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

### 1.3 — Create `package.json`

```json
{
  "name": "copilot-docker-lab",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  }
}
```

---

## 🤖 Step 2: Generate Dockerfile Using Copilot

1. Create a new file named **`Dockerfile`** (no extension) in your project folder.
2. Type the following comment at the top and wait for Copilot to suggest the full Dockerfile:

```dockerfile
# Create a Dockerfile for Node.js app using best practices, small image, expose port 3000
```

3. Press **`Tab`** to accept the Copilot suggestion.

### 🧾 Expected Output (Copilot-Generated Dockerfile)

```dockerfile
# Use official Node.js image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy app source
COPY . .

# Expose port
EXPOSE 3000

# Start application
CMD ["npm", "start"]
```

---

## 🔍 Step 3: Understand Each Dockerfile Instruction

| Instruction | Purpose |
|---|---|
| `FROM` | Specifies the base image to build from |
| `WORKDIR` | Sets the working directory inside the container |
| `COPY` | Copies files from the host into the container |
| `RUN` | Executes a command during the image build |
| `EXPOSE` | Documents the port the container listens on |
| `CMD` | Defines the default command to run the container |

> 💡 **Why `node:18-alpine`?**  
> Alpine Linux is a minimal base image (~5 MB), making your final image significantly smaller and more secure than using `node:18` (~1 GB).

---

## 🛠️ Step 4: Build the Docker Image

Run the following command from inside the `copilot-docker-lab` folder:

```bash
docker build -t copilot-node-app:v1 .
```

**What this does:**
- `docker build` — builds an image from the Dockerfile
- `-t copilot-node-app:v1` — tags the image with a name and version
- `.` — uses the current directory as the build context

### ✅ Expected Output

```
[+] Building ...
 => [1/5] FROM docker.io/library/node:18-alpine
 => [2/5] WORKDIR /app
 => [3/5] COPY package*.json ./
 => [4/5] RUN npm install
 => [5/5] COPY . .
 => exporting to image
```

---

## 🚀 Step 5: Run the Container

Verify Image
```bash
docker image ls
```

```bash
docker run -d -p 3000:3000 copilot-node-app:v1
```

**Flags explained:**
| Flag | Meaning |
|---|---|
| `-d` | Run in detached (background) mode |
| `-p 3000:3000` | Map host port 3000 → container port 3000 |

---

## 🌐 Step 6: Verify the Application

```bash
docker ps -a
```

Open your browser and navigate to:

```
http://localhost:3000
```

### ✅ Expected Output

```
Hello from Copilot Docker Lab!
```

---

## 🧪 Step 7: Hands-on Enhancement Tasks

### Task 1 — Add a `.dockerignore` File

Create a new file named **`.dockerignore`** and type this Copilot prompt:

```
# Create a .dockerignore file for node project
```

**Expected `.dockerignore`:**

```
node_modules
npm-debug.log
.git
.gitignore
.env
*.md
```

> 💡 This prevents unnecessary files from being copied into the image, keeping it lean and avoiding accidental secrets leakage.

---

### Task 2 — Optimize with Multi-Stage Build

Create a new file named **`Dockerfile.prod`** and type this Copilot prompt:

```
# Optimize Dockerfile using multi-stage build for production
```

**Expected `Dockerfile.prod`:**

```dockerfile
# ─── Stage 1: Builder ────────────────────────────────────────────────────────
FROM node:18-alpine AS builder

WORKDIR /app

# Install dependencies first (layer cache friendly)
COPY package*.json ./

# Install the dependencies
RUN npm install 

# Copy application source
COPY . .

# ─── Stage 2: Production ─────────────────────────────────────────────────────
FROM node:18-alpine AS production

# Run as non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

# Copy only the app code from builder (no external dependencies)
COPY --from=builder --chown=appuser:appgroup /app/package*.json ./
COPY --from=builder --chown=appuser:appgroup /app/app.js ./

USER appuser

EXPOSE 3000

CMD ["node", "app.js"]

```

**Build and run the production image:**

```bash
docker build -f Dockerfile.prod -t copilot-node-app:prod .
```
```bash
docker run -d -p 3000:3000 copilot-node-app:prod
```

> 💡 **Why multi-stage?**  
> The final image only contains what's needed to **run** the app — no build tools, no source extras. This reduces attack surface and image size.

---

### Task 3 — Add an Environment Variable

In your `Dockerfile` or `Dockerfile.prod`, type this Copilot prompt:

```
# Add environment variable NODE_ENV=production
```

**Expected addition:**

```dockerfile
ENV NODE_ENV=production
```

Place it after the `WORKDIR` instruction:

```dockerfile
WORKDIR /app
ENV NODE_ENV=production
```

---

## ☁️ Step 8: Push Image to DockerHub

Share your image with the world/Team by publishing it to your DockerHub registry.

### 8.1 — Log in to DockerHub

```bash
docker login --username <your-dockerhub-username>
```

> 💡 You can also use a **Personal Access Token (PAT)** instead of your password for better security.  
> Create one at: https://app.docker.com/settings

### 8.2 — Tag the Image

Docker images must be tagged with your DockerHub username before pushing:

```bash
docker tag copilot-node-app:v1 sirin07ali/copilot-node-app:v1
docker tag copilot-node-app:prod sirin07ali/copilot-node-app:prod
```

**Tag format explained:**

```
<dockerhub-username>/<image-name>:<tag>
      sirin07ali    /copilot-node-app:v1
```

### 8.3 — Push the Images

```bash
docker push sirin07ali/copilot-node-app:v1
docker push sirin07ali/copilot-node-app:prod
```

### ✅ Expected Output

```
The push refers to repository [docker.io/sirin07ali/copilot-node-app]
v1: digest: sha256:... size: ...
```

### 8.4 — Verify on DockerHub

Open your browser and visit:

```
https://hub.docker.com/r/sirin07ali/copilot-node-app
```

You should see both tags — `v1` and `prod` — listed under your repository.

### 8.5 — Pull & Run from Anywhere

Anyone can now pull and run your image with:

```bash
docker pull sirin07ali/copilot-node-app:v1
```
```bash
docker run -d -p 3000:3000 sirin07ali/copilot-node-app:v1
```

---

## 📊 Summary

| Step | Action | Command |
|---|---|---|
| 1 | Create app files | — |
| 2 | Generate Dockerfile with Copilot | — |
| 3 | Understand Dockerfile instructions | — |
| 4 | Build image | `docker build -t copilot-node-app:v1 .` |
| 5 | Run container | `docker run -d -p 3000:3000 copilot-node-app:v1` |
| 6 | Verify in browser | `http://localhost:3000` |
| 7a | Add `.dockerignore` | — |
| 7b | Multi-stage production build | `docker build -f Dockerfile.prod -t copilot-node-app:prod .` |
| 7c | Add `NODE_ENV` env variable | — |
| 8a | Login to DockerHub | `docker login --username sirin07ali` |
| 8b | Tag images | `docker tag copilot-node-app:v1 sirin07ali/copilot-node-app:v1` |
| 8c | Push to DockerHub | `docker push sirin07ali/copilot-node-app:v1` |

---

## 🧹 Cleanup

Stop and remove all running containers:

```bash
docker ps -q | ForEach-Object { docker stop $_ }
docker container prune -f
```

Remove the images:

```bash
docker rmi copilot-node-app:v1 copilot-node-app:prod
```

---

> **Lab complete!** You've successfully used GitHub Copilot to containerize a Node.js app, build a production-ready Docker image, and apply Docker best practices. 🎉
