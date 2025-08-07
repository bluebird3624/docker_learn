## ✅ PHASE 1 – Part 3: **Dockerfile — Building Your Own Images**

---

### 🧠 What is a Dockerfile?

A **Dockerfile** is a text file that contains a list of **instructions** Docker uses to build a **custom image**.

Think of it like a recipe — you define **what goes into your container**, step by step.

---

### 🧪 Let's Build a Simple Web App Image

We'll create a basic Dockerized Python web app using **Flask**.

---

### 📁 Step 1: Set Up Your Project

Create a new folder and files:

``` bash 
mkdir my-flask-app && cd my-flask-app 
# Create the app 
touch app.py requirements.txt Dockerfile
```

---

### ✍️ Step 2: Add Code to Each File

#### `app.py`

``` python 
from flask import Flask
app = Flask(__name__)
@app.route('/') 
def hello():
    return "Hello from Dockerized Flask!"  
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

#### `requirements.txt`

`flask`

---

### 📦 Step 3: Write the Dockerfile

#### `Dockerfile`

``` Dockerfile
    # Use the official Python image
    FROM python:3.10-slim  
    # Set the working directory 
    WORKDIR /app  
    # Copy the local code into the container 
    COPY requirements.txt . 
    RUN pip install --no-cache-dir -r requirements.txt  
    # Copy the rest of the app COPY . .  
    # Expose the port Flask runs on 
    EXPOSE 5000  
    # Define the command to run the app 
    CMD ["python", "app.py"]
```

---

### 🛠️ Step 4: Build Your Docker Image

``` bash 
docker build -t my-flask-app .
```

- `-t` gives your image a name (`my-flask-app`)
    
- `.` means Dockerfile is in the current directory
    

---

### ▶️ Step 5: Run the Container

```bash 
docker run -d -p 5000:5000 --name flask-container my-flask-app
```

- `-d`: detached mode (runs in background)
    
- `-p 5000:5000`: maps container port to host
    
- `--name`: name your container
    

Now go to: [http://localhost:5000](http://localhost:5000)

You should see: **"Hello from Dockerized Flask!"**

---

### 🔍 Step 6: Manage and Clean Up

```bash 
# See running containers 
docker ps  
# Stop the container 
docker stop flask-container  
# Remove it
docker rm flask-container  
# (Optional) Remove the image
docker rmi my-flask-app
```
---

### ✅ Recap: Key Dockerfile Instructions

|Command|Purpose|
|---|---|
|`FROM`|Base image to build from|
|`WORKDIR`|Sets the working directory inside the container|
|`COPY`|Copies files from your machine to the container|
|`RUN`|Runs a shell command at build time|
|`EXPOSE`|Documents the port the app uses|
|`CMD`|Command to run the container (entry point)|

## Basic Structure of a Dockerfile

Here's a sample Dockerfile, then we’ll go line by line:


```Dockerfile
# 1. Use an official base image
FROM python:3.11-slim  
# 2. Set environment variables
ENV PYTHONDONTWRITEBYTECODE=1 
ENV PYTHONUNBUFFERED=1  
# 3. Set working directory 
WORKDIR /app  
# 4. Install dependencies 
COPY requirements.txt . 
RUN pip install --no-cache-dir -r requirements.txt  
# 5. Copy application code 
COPY . .  
# 6. Run the application 
CMD ["python", "app.py"]
```

---

### 🔍 Let's break it all down with explanations and flags:

---

### 🧱 1. `FROM`


`FROM python:3.11-slim`

- Specifies the **base image** your app builds upon.
    
- This could be an OS (like `ubuntu`) or a language runtime (like `python`).
    
- `python:3.11-slim` is a **smaller version** of the official Python image → faster build, smaller size.
    

> ✅ **Best Practice**: Use a **slim** or **alpine** image unless you need full OS tools.

---

### 🧱 2. `ENV`

`ENV PYTHONDONTWRITEBYTECODE=1 ENV PYTHONUNBUFFERED=1`

- Sets environment variables inside the image.
    
- `PYTHONDONTWRITEBYTECODE=1` prevents `.pyc` files from being created.
    
- `PYTHONUNBUFFERED=1` sends output straight to terminal (no buffering) — great for logs.
    

> ✅ **Best Practice**: Use `ENV` to avoid writing repeat flags in multiple places.

---

### 🧱 3. `WORKDIR`

`WORKDIR /app`

- Sets the **working directory** inside the container.
    
- Any following commands like `COPY` or `RUN` will happen **inside this folder**.
    

> ✅ **Best Practice**: Use a clean and consistent working directory like `/app`.

---

### 🧱 4. `COPY`

`COPY requirements.txt .`

- Copies files from your **host machine** into the **container image**.
    
- Here: copying `requirements.txt` into the container’s `/app` directory.

`COPY . .`

- Copies the **entire project folder** into the container.
    

> ✅ **Best Practice**: Copy only what’s needed. Use `.dockerignore` to exclude clutter.

---

### 🧱 5. `RUN`

`RUN pip install --no-cache-dir -r requirements.txt`

- Runs a command **inside the image at build time**.
    
- Here: installing dependencies with `pip`.
    
- `--no-cache-dir`: avoids storing the pip cache → reduces image size.
    

> ✅ **Best Practice**: Combine `RUN` steps when possible to reduce layers.

---

### 🧱 6. `CMD`

`CMD ["python", "app.py"]`

- Specifies the **default command** to run when the container starts.
    
- Use **JSON array syntax** for better handling of spaces and arguments.
    

> ✅ **Best Practice**: Use `CMD` to define a default action. You can override it at runtime.

---

## 🧪 Process You Should Follow When Writing a Dockerfile

Here’s a step-by-step checklist:

---

### 🔁 REPEATABLE DOCKERFILE BUILDING PROCESS

1. **Start with a base image**  
    Choose one that fits your language/runtime needs (use `slim` or `alpine` if possible).
    
2. **Set environment variables**  
    Prevent bytecode, enable logging, set paths, etc.
    
3. **Define a working directory**  
    Keep all your files in one known place (e.g., `/app`).
    
4. **Copy only what's necessary**  
    Use `.dockerignore` to avoid copying `.git`, `__pycache__`, `node_modules`, etc.
    
5. **Install dependencies**
    
    - For Python: `pip install`
        
    - For Node: `npm install`
        
6. **Copy application files**
    
7. **Expose required ports (if needed)**  
    (e.g., `EXPOSE 8000`) → purely **documentational**, doesn't open the port automatically.
    
8. **Define the startup command**  
    With `CMD` or `ENTRYPOINT`.
    
9. **Test and optimize image size**  
    Use `docker images` to check size and refine your image.
    

---

## 💥 Common Mistakes People Make

|Mistake|Why It Happens|How to Fix|
|---|---|---|
|Using a full OS base image|New users default to `ubuntu`, which is big|Use `python:slim` or `alpine`|
|Not using `.dockerignore`|Results in huge, bloated images|Always add `.dockerignore` like `.git`, `venv`, etc.|
|Layer sprawl (too many `RUN` commands)|Each `RUN` creates a new image layer|Combine commands: `RUN apt update && apt install -y xyz`|
|Not pinning dependency versions|Builds can change over time|Use fixed versions in `requirements.txt` or `Dockerfile`|
|Confusing `CMD` vs `ENTRYPOINT`|Subtle differences in how args are passed|Use `CMD` unless you need strict control|

---

## 🧠 Easy Way to Remember Dockerfile Instructions:

|Instruction|Think of it as...|
|---|---|
|`FROM`|Your **starting point** (base image)|
|`ENV`|Your **container’s settings**|
|`WORKDIR`|Your container’s **"current folder"**|
|`COPY`|**Importing** files into the container|
|`RUN`|**Build-time commands** (set up)|
|`CMD`|The **startup command** when container runs|

---

## ✅ Final Tip: Test Your Image

Once your Dockerfile is ready:

`docker build -t myapp . docker run -it myapp`

- `-t myapp` → gives your image a name
    
- `.` → current directory has the Dockerfile

---

### 🧪 Optional Challenge:

Dockerize a basic Node.js or PHP app using a Dockerfile.  
Let me know if you'd like a scaffolded example to try it out.