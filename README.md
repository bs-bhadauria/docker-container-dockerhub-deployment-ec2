# 🚀 Docker Container Deployment on AWS EC2

## 📌 Project Overview

This project demonstrates a complete end-to-end Docker workflow by building, packaging, and deploying a containerized application across multiple environments using AWS EC2 and Docker Hub.

The objective of this project is to understand how containerization enables portability, consistency, and efficient deployment of applications.

---

## 🧠 Key Concept

**Build Once, Run Anywhere**

A Docker image is built on one EC2 instance, pushed to Docker Hub, and then pulled and executed on another EC2 instance without any changes.

---

## 🏗️ Architecture

```
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
```

---

## ⚙️ Tech Stack

- Docker  
- AWS EC2  
- Linux (Ubuntu)  
- Docker Hub  
- GitHub  
- Python / Go  

---

## 🔥 Features

- Docker image creation from application source code  
- Containerized deployment on AWS EC2  
- Docker Hub integration (push & pull)  
- Cross-instance deployment  
- Multi-stage Docker builds  
- Distroless images  
- Docker volumes (persistent storage)  
- Bind mounts  

---

## 🛠️ Step-by-Step Implementation

### 1️⃣ Launch EC2 Instance

```bash
ssh -i key.pem ubuntu@<EC2-PUBLIC-IP>
```

---

### 2️⃣ Install Docker

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

(Optional)

```bash
sudo usermod -aG docker $USER
```

---

### 3️⃣ Clone Repository

```bash
git clone <your-github-repo-url>
cd <project-folder>
```

---

### 4️⃣ Build Docker Image

```bash
docker build -t <your-dockerhub-username>/my-app .
```

---

### 5️⃣ Run Container

```bash
docker run -d -p 8000:8000 <your-dockerhub-username>/my-app
```

Access:

```
http://<EC2-PUBLIC-IP>:8000
```

---

### 6️⃣ Push to Docker Hub

```bash
docker login
docker push <your-dockerhub-username>/my-app
```

---

### 7️⃣ Deploy on Second EC2

```bash
docker pull <your-dockerhub-username>/my-app
docker run -d -p 8000:8000 <your-dockerhub-username>/my-app
```

---

### 8️⃣ Security Group

Allow:

- Port: 8000  
- Source: 0.0.0.0/0  

---

## 📦 Docker Volume

```bash
docker volume create golang-volume
```

```bash
docker run -it \
--mount type=volume,source=golang-volume,target=/app \
ubuntu bash
```

Inside container:

```bash
cd /app
touch test.txt
```

Check on host:

```bash
sudo ls /var/lib/docker/volumes/golang-volume/_data
```

---

## 🔍 Concepts Covered

- Image vs Container  
- Multi-stage builds  
- Distroless images  
- Volumes vs Bind mounts  
- Container filesystem  

---

## ⚠️ Errors & Fixes

### Permission Denied

```bash
sudo usermod -aG docker $USER
```

---

### Access Denied (Push)

```bash
docker tag image username/image
docker push username/image
```

---

### Port Already Used

```bash
docker stop <container_id>
```

---

### Container Not Running

```bash
docker run -it ubuntu bash
```

---

### Invalid Mount

```bash
--mount type=volume,source=volume_name,target=/path
```

---

## 📈 Key Learnings

- Containers are lightweight  
- Docker ensures consistency  
- Images are reusable  
- Multi-stage reduces size  
- Distroless improves security  
- Volumes provide persistence  

---

## 🚀 Future Improvements

- CI/CD (GitHub Actions / Jenkins)  
- Docker Compose  
- Nginx  
- Kubernetes  

---

## 👨‍💻 Author

Bhoopendra Singh Bhadauria

---

## ⭐ Conclusion

This project demonstrates a real-world Docker workflow from build to deployment across multiple environments.
