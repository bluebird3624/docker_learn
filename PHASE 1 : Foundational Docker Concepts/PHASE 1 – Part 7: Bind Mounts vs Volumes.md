## PHASE 1 – Part 7: **Bind Mounts vs Volumes**

This is where you learn the difference between **shipping your app** vs **working on it live** using Docker.

---

### 🔁 Quick Recap:

|Feature|**Volume**|**Bind Mount**|
|---|---|---|
|Managed by|Docker|You (host path)|
|Use case|Persistent app data (e.g. DB)|Live code editing (dev)|
|Path|Docker decides|Exact host path you define|
|Portability|✅ portable|❌ host-dependent|

---

## 📦 1. **Volume Example (Repeat from earlier)**

bash

CopyEdit

`docker run -v myvolume:/data busybox`

- Creates a named volume `myvolume`
    
- Docker manages the storage location
    
- Best for storing app **data**
    

---

## 🗂️ 2. **Bind Mount Example (Live Code Sharing)**

This is ideal for local development.

### 🔧 Example:

Let’s say you have a Python project in:

swift

CopyEdit

`/home/you/projects/myapp`

Run a container that uses your local code:

bash

CopyEdit

`docker run -it --rm \   -v /home/you/projects/myapp:/app \   -w /app \   python:3.10 \   python app.py`

- `-v <host_path>:<container_path>`: Mounts your code into the container
    
- `-w /app`: Sets working directory inside container
    
- `--rm`: Deletes container after exit
    

Your **code stays on your machine**, but the container runs it.  
Any code changes are instantly reflected without rebuilding the image.

---

## 🧪 Test This: Live Flask App

### 1. Create a simple Flask app (`app.py`):

python

CopyEdit

`from flask import Flask app = Flask(__name__)  @app.route('/') def hello():     return "Live reload test"  if __name__ == '__main__':     app.run(debug=True, host='0.0.0.0')`

### 2. Run it using bind mount:

bash

CopyEdit

`docker run -it --rm \   -p 5000:5000 \   -v $(pwd):/app \   -w /app \   python:3.10 \   sh -c "pip install flask && python app.py"`

- Visit `http://localhost:5000`
    
- Modify `app.py` (change the message)
    
- Re-run container — the changes are **live from your local code**
    

> Want even smoother dev experience? Use [**hot reload tools**] inside containers (e.g., `flask run --reload` or [nodemon](https://www.npmjs.com/package/nodemon) for Node.js).

---

## 🚨 Watch Out

- Bind mounts can break if host paths don’t exist.
    
- Not good for production (host-path specific).
    
- Use volumes for **data**, bind mounts for **code** (during development).
    

---

## ✅ Summary

|Use case|Use this|
|---|---|
|Code sharing (local dev)|Bind Mounts|
|Persistent DB/data|Volumes|
|Production containers|Volumes or baked-in code|

---

## 🧪 Challenge (Optional):

- Set up a bind mount for a Node.js or Python app.
    
- Change the code locally and re-run the container.
    
- Prove to yourself that Docker is seeing live changes from your editor.
    

---

### 🎉 Congrats – You Finished **PHASE 1: Docker Fundamentals**!

You now know:

- What Docker is
    
- How to build/run images
    
- How to persist data
    
- How containers communicate
    
- How to inject config/env vars
    
- How to work with volumes and bind mounts