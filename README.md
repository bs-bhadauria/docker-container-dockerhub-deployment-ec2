# Docker Container Deployment on AWS EC2

## Project Overview

This project demonstrates a complete end-to-end Docker workflow by building, packaging, and deploying a containerized application across multiple environments using AWS EC2 and Docker Hub.

The objective of this project is to understand how containerization enables portability, consistency, and efficient deployment of applications.

---

## Key Concept

**"Build Once, Run Anywhere"**

The same Docker image is built on one EC2 instance, pushed to Docker Hub, and then pulled and executed on another EC2 instance — without any changes.

---

## Architecture


Developer Code
↓
GitHub Repository
↓
AWS EC2 Instance (Build Server)
↓
Docker Build
↓
Docker Image
↓
Docker Hub (Registry)
↓
AWS EC2 Instance (Deployment Server)
↓
Docker Pull & Run
↓
Application Running in Browser


---

## ⚙️ Tech Stack

- Docker
- AWS EC2
- Linux (Ubuntu)
- Docker Hub
- GitHub
- Python / Go (based on your application)

---

## Features

- Docker image creation from application source code
- Containerized application deployment on AWS EC2
- Docker Hub integration (push & pull images)
- Cross-instance deployment validation
- Multi-stage Docker builds for optimization
- Distroless images for minimal and secure containers
- Docker volumes for persistent storage
- Bind mounts for development workflow

---

## Step-by-Step Implementation

### 1️⃣ Launch EC2 Instance

- Create an AWS EC2 instance (Ubuntu)
- Connect using SSH

```bash
ssh -i key.pem ubuntu@<EC2-PUBLIC-IP>
2️⃣ Install Docker
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker

(Optional: run without sudo)

sudo usermod -aG docker $USER
3️⃣ Clone Application Repository
git clone <your-github-repo-url>
cd <project-folder>
4️⃣ Build Docker Image
docker build -t <your-dockerhub-username>/my-app .
5️⃣ Run Container Locally
docker run -d -p 8000:8000 <your-dockerhub-username>/my-app

Access in browser:

http://<EC2-PUBLIC-IP>:8000
6️⃣ Push Image to Docker Hub

Login:

docker login

Push image:

docker push <your-dockerhub-username>/my-app
7️⃣ Deploy on Second EC2 Instance
Create another EC2 instance
Install Docker

Pull image:

docker pull <your-dockerhub-username>/my-app

Run container:

docker run -d -p 8000:8000 <your-dockerhub-username>/my-app
8️⃣ Configure Security Group

Allow inbound traffic:

Port: 8000
Source: 0.0.0.0/0
📦 Docker Volume Implementation

Create volume:

docker volume create golang-volume

Run container with volume:

docker run -it \
--mount type=volume,source=golang-volume,target=/app \
ubuntu bash
Verify Data Persistence

Inside container:

cd /app
touch test.txt

Exit container and check:

sudo ls /var/lib/docker/volumes/golang-volume/_data

Delete container and verify data still exists.

🔍 Important Concepts Covered
🔹 Docker Image vs Container
Image = Blueprint
Container = Running instance
🔹 Multi-Stage Builds
Reduced image size significantly
Removed unnecessary build dependencies
🔹 Distroless Images
Minimal runtime images
Improved security
Very small size (~2MB)
🔹 Docker Volumes
Persistent storage
Data survives container deletion
🔹 Bind Mounts
Direct host ↔ container mapping
Useful for development
🔹 Container Filesystem
Read-only image layers
Writable container layer
Temporary storage (deleted with container)
⚠️ Errors Faced & Fixes
❌ Permission Denied (Docker)
permission denied while trying to connect to the Docker daemon

✅ Fix:

sudo usermod -aG docker $USER
❌ Access Denied (Docker Push)
denied: requested access to the resource is denied

✅ Fix:

Use correct Docker Hub username:
docker tag image username/image
docker push username/image
❌ Image Not Found
No such image

✅ Fix:

docker images

Use correct image name.

❌ Port Already Allocated
port is already allocated

✅ Fix:

Stop existing container OR use different port
docker stop <container_id>
❌ Container Not Running
container is not running

✅ Fix:

Run container in interactive mode:
docker run -it ubuntu bash
❌ Invalid Mount Syntax
invalid field must be key=value pair

✅ Fix:

--mount type=volume,source=volume_name,target=/path
📈 Key Learnings
Containers are lightweight and portable
Docker enables consistent environments across systems
Images can be reused and shared via registries
Multi-stage builds drastically reduce image size
Distroless images improve security and performance
Volumes ensure persistent storage in containers
Real-world deployment involves networking and security configuration
🚀 Future Improvements
Add CI/CD pipeline (GitHub Actions / Jenkins)
Use Docker Compose for multi-container apps
Add Nginx reverse proxy
Deploy using Kubernetes
👨‍💻 Author

Bhoopendra Singh Bhadauria
