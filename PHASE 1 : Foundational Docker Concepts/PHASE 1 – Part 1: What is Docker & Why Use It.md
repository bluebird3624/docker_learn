### 🚀 Let’s Begin: PHASE 1, Part 1

---

## 📌 1. What is Docker & Why Use It?

### 🧠 Concept:

Docker is a **containerization platform** that allows you to package an application and its dependencies into a **container**.

- Think of it like a lightweight VM.
    
- The same app runs the same way everywhere.
    
- Useful for dev, testing, CI, and production.
    

---

### 🧱 Key Terms:

|Term|Meaning|
|---|---|
|**Image**|A read-only blueprint of your application (like a class).|
|**Container**|A running instance of an image (like an object).|
|**Dockerfile**|A script that defines how to build an image.|
|**Docker Engine**|The runtime that runs your containers.|

---

### ✅ Why Use Docker?

- **Works on My Machine™** is eliminated.
    
- Run isolated services (DB, APIs, etc.) easily.
    
- Fast provisioning and clean environments.
    
- Simplifies deployment.
    

---

### 🚦 Your First Commands (Try These Now):

bash

CopyEdit

`# Check Docker is installed docker --version  # Pull an image from Docker Hub docker pull hello-world  # Run the image (runs a test container) docker run hello-world`

You should see a success message.

---

### 📝 Assignment 1:

Try running a containerized version of Nginx:

bash

CopyEdit

`docker run -d -p 8080:80 --name webserver nginx`

Then visit: `http://localhost:8080`

- Run: `docker ps` — see the container running
    
- Run: `docker stop webserver`
    
- Run: `docker rm webserver`