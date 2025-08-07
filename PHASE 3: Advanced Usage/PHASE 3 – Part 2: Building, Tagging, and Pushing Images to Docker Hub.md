# ✅ PHASE 3 – Part 2: **Building, Tagging, and Pushing Images to Docker Hub**

---

## 🧠 What is Docker Hub?

- A **public registry** for Docker images.
    
- Like GitHub for containers — share, store, and deploy images.
    
- You can create **private repositories** too (paid).
    

---

## 🛠️ Step-by-Step Guide

### 1. **Create a Docker Hub Account**

Go to hub.docker.com and sign up.

---

### 2. **Login from CLI**

bash

CopyEdit

`docker login`

Enter your Docker Hub username and password.

---

### 3. **Build your Docker image**

Example:

bash

CopyEdit

`docker build -t myapp:latest .`

---

### 4. **Tag the image**

Docker Hub repos use this format:

php-template

CopyEdit

`<dockerhub-username>/<repository>:<tag>`

Example:

bash

CopyEdit

`docker tag myapp:latest yourusername/myapp:latest`

---

### 5. **Push the image**

bash

CopyEdit

`docker push yourusername/myapp:latest`

You’ll see upload progress.

---

### 6. **Pull the image anywhere**

bash

CopyEdit

`docker pull yourusername/myapp:latest`

Great for deployment or sharing!

---

## 🔎 Check Your Images Online

Go to your Docker Hub profile and find the repo.

---

## 🧪 Exercise

- Build and tag a simple image (like your Flask or Node app).
    
- Push it to Docker Hub.
    
- Try pulling it on another machine or VM.
    

---

## ✅ Best Practices for Tagging

|Tag|Use for|
|---|---|
|`latest`|Default/latest stable version|
|Semantic versioning|e.g., `v1.0.0`, `v1.2.3`|
|Branch or commit tags|e.g., `dev`, `feature-x`, commit hashes|
