## ✅ PHASE 1 – Part 2: **Docker Images vs Containers**

---

### 🧠 **Conceptual Difference**

|**Images**|**Containers**|
|---|---|
|Think of it like a **class** in programming.|Think of it like an **object**—an instance of the class.|
|A **blueprint** of the application.|A **running process** created from an image.|
|Static and **read-only**.|Dynamic and **read-write**.|
|You build it.|You run it.|

---

### 🔧 Example:

bash

CopyEdit

`# Pull the official Python image docker pull python:3.10  # Run a container from that image docker run -it python:3.10`

You’ll now be inside a running container with the Python shell open. Try typing:

python

CopyEdit

`print("Hello from inside Docker!")`

Then type `exit()` to leave the container.

---

### 🔍 Inspecting What's Happening

#### Check images you have:

bash

CopyEdit

`docker images`

#### Check running containers:

bash

CopyEdit

`docker ps`

#### Check **all** containers (even stopped ones):

bash

CopyEdit

`docker ps -a`

---

### 🛠️ Useful Commands Recap

|Command|What It Does|
|---|---|
|`docker images`|List all downloaded images|
|`docker ps`|List running containers|
|`docker ps -a`|List all containers (running or not)|
|`docker run`|Run a new container from an image|
|`docker stop <container>`|Stop a running container|
|`docker rm <container>`|Remove a stopped container|
|`docker rmi <image>`|Remove an image|

---

### 📦 Image vs Container in Action

bash

CopyEdit

`# Pull the Alpine Linux image docker pull alpine  # Run a container from Alpine docker run -it alpine sh`

Now you're in a minimal shell inside a container. Try:

sh

CopyEdit

`echo "Hello from Alpine"`

Then exit with `exit`.

---

### 🧪 Optional Challenge (5–10 mins):

1. Pull the `ubuntu` image.
    
2. Run it in interactive mode with a terminal.
    
3. Install a tool inside the container (`apt update && apt install curl`).
    
4. Try running `curl google.com`.
    

---

### ✅ Summary:

- **Images** are the blueprint.
    
- **Containers** are running (or stopped) instances of those images.
    
- You pull/build images, and you run containers from them.