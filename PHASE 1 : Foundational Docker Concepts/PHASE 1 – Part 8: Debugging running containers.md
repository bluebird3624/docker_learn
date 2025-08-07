# 🐳 **Debugging Running Containers** (Phase 1 – Addendum)

---

## 🧠 Why It Matters

When your app is not working as expected inside a container, you need to figure out _why_ — and fast. Docker gives you several built-in tools for this.

---

## ✅ The 4 Pillars of Container Debugging

---

### 1. **View Logs**

`docker logs <container_name>`

- Shows stdout/stderr from the container.
    
- Use `-f` to follow live:
    

`docker logs -f <container_name>`

👉 Use this **first** if your app won't start, crashed, or behaves oddly.

---

### 2. **Exec into the Container**

`docker exec -it <container_name> bash`

- Opens a shell inside the container.
    
- Use `sh` if `bash` is unavailable (common in Alpine-based images):
    

`docker exec -it <container_name> sh`

Once inside, you can:

- Inspect files
    
- Run app-specific commands (`curl`, `ps`, `top`, etc.)
    
- Check environment variables:
    

`env`

- Check config files or logs within the container’s filesystem
    

---

### 3. **Inspect the Container Metadata**

`docker inspect <container_name>`

- Dumps low-level JSON info.
    
- Helpful for:
    
    - IP addresses
        
    - Mounts
        
    - Environment variables
        
    - Healthcheck status
        

You can extract specific info using `--format`:

`docker inspect --format '{{ .Config.Env }}' <container_name>`

---

### 4. **Live Resource Monitoring**

`docker stats`

- Shows CPU/memory usage for all running containers.
    
- Helpful if your container is crashing or getting killed due to OOM (Out of Memory).
    

---

## 🔍 Extra Debugging Tips

- Use **`docker-compose logs`** to get logs from all services.
    
- Temporarily override `CMD` or `ENTRYPOINT` to open a shell instead of starting the app:
    

`docker run -it --entrypoint bash myapp`

- Mount the local app directory to test code changes live (good for dev):
    

`docker run -v $(pwd):/app -w /app node:20 bash`

---

## 🧪 Debugging Checklist

|What’s wrong?|Where to look|
|---|---|
|App crashes or exits early|`docker logs` or `CMD` in Dockerfile|
|Can't connect to service|`docker exec` + ping/curl inside container|
|Config not loading|`docker inspect` + check ENV variables|
|Not enough memory/CPU|`docker stats`|
|Healthcheck failing|`docker inspect` → check `.State.Health`|

---

## ✅ Summary

|Tool/Command|Purpose|
|---|---|
|`docker logs`|See container stdout/stderr|
|`docker exec -it <name> bash`|Shell into the container|
|`docker inspect <name>`|Get metadata like ENV, IP, mounts|
|`docker stats`|Live performance monitoring|

---

Now you’ve got a full set of debugging tools to confidently troubleshoot any Docker container. 🛠️

Let me know if you want to simulate a broken container and debug it together.


---

# 🐞 Simulating a Broken Container

We’ll create a simple **Node.js app** that crashes on startup.

---

## 🔧 Step 1: Set Up the Broken App

Create a folder:

`mkdir broken-app && cd broken-app`

Create a file called `app.js`:

`// app.js console.log("Starting app..."); throw new Error("Oops! Something went wrong.");`

Create a `package.json`:

`{   "name": "broken-app",   "version": "1.0.0",   "main": "app.js",   "scripts": {     "start": "node app.js"   } }`

---

## 🐳 Step 2: Create a Dockerfile



`# Dockerfile FROM node:20  WORKDIR /app  COPY . .  RUN npm install  CMD ["npm", "start"]`

---

## 🧱 Step 3: Build the Image

`docker build -t broken-app .`

---

## 🧪 Step 4: Run the Container

`docker run --name test-broken broken-app`

It will crash immediately. Now we debug.

---

# 🧰 Phase 1: Debugging

---

### ✅ 1. Check Logs

`docker logs test-broken`

Output:

`Starting app... /app/app.js:2 throw new Error("Oops! Something went wrong.");       ^ Error: Oops! Something went wrong.`

➡️ **We found the error**: an unhandled exception in `app.js`.

---

### ✅ 2. Exec into Container?

You **can’t** — because the container exited. Try this:

`docker exec -it test-broken bash`

Error:

`Error response from daemon: Container test-broken is not running`

➡️ So we’ll need to start it in interactive mode instead.

---

### ✅ 3. Run with Bash Instead of App

`docker run -it --entrypoint bash broken-app`

Now you’re inside the container **before** the app runs.

Try running it manually:

`node app.js`

➡️ Boom. You see the crash again — same output.

You can now modify files or debug further from inside the container.

---

### ✅ 4. Inspect the Container (optional)

If you're unsure why it exited:

`docker inspect test-broken`

You can also extract the exit code:

`docker inspect test-broken --format='{{.State.ExitCode}}'`

Exit code `1` confirms an error on startup.

---

## 🧼 Cleanup

`docker rm test-broken docker rmi broken-app`

---

## ✅ Summary: What You Just Learned

|Step|Tool/Command|
|---|---|
|Get logs|`docker logs <name>`|
|Check container status|`docker ps -a`|
|Enter container manually|`--entrypoint bash` before app runs|
|Investigate exit reason|`docker inspect --format`|
