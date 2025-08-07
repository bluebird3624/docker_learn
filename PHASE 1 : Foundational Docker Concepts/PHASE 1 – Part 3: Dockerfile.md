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

bash

CopyEdit

`mkdir my-flask-app && cd my-flask-app  # Create the app touch app.py requirements.txt Dockerfile`

---

### ✍️ Step 2: Add Code to Each File

#### `app.py`

python

CopyEdit

`from flask import Flask app = Flask(__name__)  @app.route('/') def hello():     return "Hello from Dockerized Flask!"  if __name__ == '__main__':     app.run(host='0.0.0.0', port=5000)`

#### `requirements.txt`

nginx

CopyEdit

`flask`

---

### 📦 Step 3: Write the Dockerfile

#### `Dockerfile`

Dockerfile

CopyEdit

`# Use the official Python image FROM python:3.10-slim  # Set the working directory WORKDIR /app  # Copy the local code into the container COPY requirements.txt . RUN pip install --no-cache-dir -r requirements.txt  # Copy the rest of the app COPY . .  # Expose the port Flask runs on EXPOSE 5000  # Define the command to run the app CMD ["python", "app.py"]`

---

### 🛠️ Step 4: Build Your Docker Image

bash

CopyEdit

`docker build -t my-flask-app .`

- `-t` gives your image a name (`my-flask-app`)
    
- `.` means Dockerfile is in the current directory
    

---

### ▶️ Step 5: Run the Container

bash

CopyEdit

`docker run -d -p 5000:5000 --name flask-container my-flask-app`

- `-d`: detached mode (runs in background)
    
- `-p 5000:5000`: maps container port to host
    
- `--name`: name your container
    

Now go to: [http://localhost:5000](http://localhost:5000)

You should see: **"Hello from Dockerized Flask!"**

---

### 🔍 Step 6: Manage and Clean Up

bash

CopyEdit

`# See running containers docker ps  # Stop the container docker stop flask-container  # Remove it docker rm flask-container  # (Optional) Remove the image docker rmi my-flask-app`

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

---

### 🧪 Optional Challenge:

Dockerize a basic Node.js or PHP app using a Dockerfile.  
Let me know if you'd like a scaffolded example to try it out.