

# ************************************

# **01 — SETUP & INSTALLATION**

# ************************************

## **01_02 — Tools to Install**

### 1. **Docker Desktop**

URL: [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
👉 This is the main application that lets you run Docker containers.

### 2. **VS Code**

URL: [https://code.visualstudio.com/download](https://code.visualstudio.com/download)
👉 Code editor for writing Dockerfiles, docker-compose files, and development.

---

## **01_03 — Git Commands**

### **Clone the course project**

```sh
cd c:\projects\docker-your-first-project-4485003
git clone https://github.com/LinkedInLearning/docker-your-first-project-4485003.git
```

### **Switch to specific branch (movie chapter)**

```sh
git checkout CHAPTER#_MOVIE#
```

### **See your branches**

```sh
git branch
```

👉 These are standard Git workflow commands.

---

# ************************************

# **02 — DOCKER BUILD BASICS**

# ************************************

## **02_01 — Dockerfile code**

The video's code is inside the Dockerfile.
No commands needed here.

---

## **02_02 — docker build**

### **Command Format**

```sh
docker build [OPTIONS] PATH | URL | -
```

### **Build image in current folder**

```sh
docker build .
```

👉 Looks for a Dockerfile in current directory and builds an image.

### **Specify Dockerfile**

```sh
docker build -f MyDockerfile .
```

### **Force remove intermediate containers**

```sh
docker build --force-rm=true .
```

### **Remove temporary containers after build**

```sh
docker build --rm=true .
```

### **Build without using cache**

```sh
docker build --no-cache .
```

👉 Useful when Docker reuses old layers but you want a fresh build.

### **Help**

```sh
docker build --help
```

---

# ************************************

# **03 — WORKING WITH IMAGES**

# ************************************

## **03_01 — Searching and pulling images**

### **Search for image**

```sh
docker search python
```

### **Filter official images**

```sh
docker search --filter is-official=true python
```

### **Filter by stars**

```sh
docker search --filter stars=100 python
```

### **Filter automated builds**

```sh
docker search --filter is-automated=true python
```

### **Combine filters**

```sh
docker search --filter is-official=true --filter stars=10 --filter is-automated=false python
```

### **Limit results**

```sh
docker search --limit 4 python
```

### **Show full description**

```sh
docker search --no-trunc python
```

### **Custom format**

```sh
docker search --format "{{.Name}}: {{.Description}}" python
```

### **Pull image**

```sh
docker image pull python
```

### **Pull specific tag**

```sh
docker image pull python:3.12-rc-bookworm
```

---

## **03_02 — Listing images**

### **Show all images**

```sh
docker image ls --all
```

### **Show long IDs**

```sh
docker image ls --no-trunc
```

### **Show only IDs**

```sh
docker image ls --quiet
```

### **Show digests**

```sh
docker image ls --digests
```

### **Filter by image name**

```sh
docker image ls --filter reference=python*
```

### **Filter before**

```sh
docker image ls --filter before=python
```

### **Filter since**

```sh
docker image ls --filter since=python
```

### **Filter dangling (unused) images**

```sh
docker image ls --filter dangling=true
```

### **Filter by label**

```sh
docker image ls --filter label=python*
```

### **Custom format table**

```sh
docker image ls --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
```

---

## **03_03 — Tagging + labels**

### **Build with a tag**

```sh
docker build -t big-star-collectibles .
```

### **Build with multiple tags**

```sh
docker build --no-cache -t big-star-collectibles:v1 -t big-star-collectibles .
```

### **Add tag to existing image**

```sh
docker tag big-star-collectibles:v1 big-star-collectibles:v1-python
```

### **Tag using image ID**

```sh
docker tag IMAGE_ID big-star-collectibles:v1
```

### **Add labels inside Dockerfile**

```
LABEL key=value key=value
```

### **Filter images by label**

```sh
docker image ls --filter label="com.example.vendor"="Big Star Collectibles"
```

---

## **03_04 — Push to Docker Hub**

### **Login**

```sh
docker login
```

### **Tag image for Docker Hub**

```sh
docker tag big-star-collectibles:v2 username/repo:tag
```

### **Push**

```sh
docker push username/repo:tag
```

### **Pull**

```sh
docker pull username/repo:tag
```

---

## **03_05 — Inspect image**

### **Show raw config**

```sh
docker image inspect IMAGE_ID
```

### **Format (labels only)**

```sh
docker image inspect --format='{{json .Config.Labels}}' image_name
```

---

## **03_06 — Remove images**

### **Delete single image**

```sh
docker rmi big-star-collectibles:v2
```

### **Delete by ID**

```sh
docker rmi IMAGE_ID
```

### **Delete by digest**

```sh
docker rmi username/repo@DIGEST
```

---

# ************************************

# **04 — WORKING WITH CONTAINERS**

# ************************************

## **04_01 — Run / Start / Stop / Kill**

### **Start existing container**

```sh
docker start CONTAINER
```

### **Run new container**

```sh
docker run -it -d -p 5000:5000 -v ${PWD}:/app/code big-star-collectibles
```

**Meaning:**

* `-it` → interactive terminal
* `-d` → run in background
* `-p` → port mapping
* `-v` → mount folder

### **Open in browser**

[http://localhost:5000](http://localhost:5000)

### **Stop running container**

```sh
docker stop CONTAINER
```

### **Force kill container**

```sh
docker kill CONTAINER
```

---

## **04_02 — docker ps**

### **Show running containers**

```sh
docker ps
```

### **Show all containers**

```sh
docker ps -a
```

### **Show last container**

```sh
docker ps -n 1
```

### **Only IDs**

```sh
docker ps -q
```

### **Show sizes**

```sh
docker ps -s
```

### **Show latest created**

```sh
docker ps -l
```

### **No truncation**

```sh
docker ps --no-trunc
```

### **Filter by label**

```sh
docker ps --filter label="vendor=Big Star Cookies"
```

### **Filter by exited status**

```sh
docker ps -a --filter 'exited=0'
```

### **Filter by image**

```sh
docker ps --filter ancestor=big-star-collectibles
```

---

### ⭐ **Common Docker Exit Codes**

| Code | Meaning                 |
| ---- | ----------------------- |
| 0    | Clean exit              |
| 1    | Application error       |
| 125  | Container failed to run |
| 126  | Command cannot run      |
| 127  | File not found          |
| 128  | Invalid argument        |
| 134  | Abnormal termination    |
| 137  | SIGKILL                 |
| 139  | Segmentation fault      |
| 143  | SIGTERM                 |
| 255  | Out of range            |

---

## **04_03 — docker inspect**

### **Inspect container**

```sh
docker inspect NAME
```

### **Get container image**

```sh
docker inspect --format='{{.Config.Image}}' NAME
```

### **Get container ID**

```sh
docker inspect --format='{{.Id}}' NAME
```

### **Get log file path**

```sh
docker inspect --format='{{.LogPath}}' NAME
```

### **Show full config JSON**

```sh
docker inspect --format='{{json .Config}}' NAME
```

---

## **04_04 — docker logs**

### **Show logs**

```sh
docker logs CONTAINER
```

### **Follow logs**

```sh
docker logs -f CONTAINER
```

### **Show last 1000 lines**

```sh
docker logs --tail 1000 -f CONTAINER
```

### **With details**

```sh
docker logs --details CONTAINER
```

### **Using timestamps**

```sh
docker logs --timestamps CONTAINER
```

### **Logs since datetime**

```sh
docker logs --since 2023-12-01T01:00:00Z
```

### **Relative time**

```sh
docker logs --since 60
```

---

## **04_05 — Volumes**

### **Create volume**

```sh
docker volume create data-vol
```

### **List volumes**

```sh
docker volume ls
```

### **Inspect volume**

```sh
docker volume inspect data-vol
```

### **Use volume**

```sh
docker run -v data-vol:/app/data IMAGE
```

### **Remove volume**

```sh
docker volume rm data-vol
```

### **Bind mount (folder from host)**

```sh
docker run -v "$(PWD)/test":/app/test IMAGE
```

### **Mount with --mount**

```sh
docker run --mount type=bind,source="$(PWD)/test",target=/app/test IMAGE
```

---

## **04_06 — Pruning (Clean up)**

### **Remove unused images**

```sh
docker image prune
```

### **Remove ALL unused images**

```sh
docker image prune -a -f
```

### **Prune by label**

```sh
docker image prune -a -f --filter label="vendor=Big Star"
```

### **Remove unused containers**

```sh
docker container prune
```

### **Remove unused volumes**

```sh
docker volume prune -f
```

### **Remove EVERYTHING**

```sh
docker system prune --volumes -f
```

---
