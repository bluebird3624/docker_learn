## PHASE 1 – Part 4: **Docker Volumes – Data Persistence**

---

### 🧠 Why You Need Volumes

By default, when a container stops or is deleted:

- **All data inside it is lost.**
    
- Containers are **ephemeral** (short-lived) by design.
    

But what if you're running:

- A **PostgreSQL** database?
    
- A **web app** that generates files or logs?
    
- Anything that needs **persistent storage**?
    

👉 That’s where **Docker volumes** come in.

---

## 📦 What is a Volume?

> A **volume** is a persistent storage area managed by Docker.  
> It lives **outside the container**, so it survives restarts and deletions.

---

## 🛠️ Two Types of Mounts

|Type|Description|
|---|---|
|**Volume**|Managed by Docker, lives in `/var/lib/docker/volumes/`.|
|**Bind Mount**|Links a specific path on your host machine to the container. Useful in dev.|

---

## 🚀 Let’s See It in Action (Simple Example)

We’ll use a container that writes data to a file.

---

### Step 1: Run a container with a volume

bash

CopyEdit

`docker run -d --name volume-test \   -v mydata:/data \   busybox sh -c "while true; do date >> /data/log.txt; sleep 1; done"`

- `-v mydata:/data`: This creates a **named volume** called `mydata` and mounts it at `/data` inside the container.
    
- `busybox` is a minimal Linux container.
    
- It writes the current date into `/data/log.txt` every second.
    

---

### Step 2: See What’s Going On

bash

CopyEdit

`# Check the volume docker volume ls  # Inspect it docker volume inspect mydata`

---

### Step 3: Look Inside the Volume

You can't directly browse it from the host easily, but you **can run another container** using the same volume:

bash

CopyEdit

`docker run -it --rm -v mydata:/data busybox cat /data/log.txt`

You’ll see the output like:

python-repl

CopyEdit

`Thu Aug  7 12:00:00 UTC 2025 Thu Aug  7 12:00:01 UTC 2025 ...`

---

### Step 4: Clean Up

bash

CopyEdit

`docker stop volume-test docker rm volume-test docker volume rm mydata`

---

## 📁 Bind Mounts (Great for Development)

Mount your **actual source code** into a container so changes reflect immediately.

bash

CopyEdit

`docker run -v $(pwd):/app -w /app python:3.10 python app.py`

This:

- Mounts your current directory (`$(pwd)`) into `/app` in the container.
    
- Runs `python app.py`.
    

**Perfect for dev environments** — no need to rebuild image after every code change.

---

## ✅ Summary

|Feature|Volumes|Bind Mounts|
|---|---|---|
|Managed by Docker|✅|❌|
|Good for dev|❌|✅|
|Secure & portable|✅|❌|
|Visible on host|Harder|Yes|

---

### 🧪 Challenge

Try running a PostgreSQL or MySQL container with a volume:

bash

CopyEdit

`docker run --name mydb -e POSTGRES_PASSWORD=secret \   -v pgdata:/var/lib/postgresql/data \   -d postgres`

Then delete and recreate the container — your DB stays intact.