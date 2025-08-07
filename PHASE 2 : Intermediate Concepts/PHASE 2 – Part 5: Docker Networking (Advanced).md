# ✅ PHASE 2 – Part 5: **Docker Networking (Advanced)**

---

## 🧠 Why Care About Networking?

Docker isolates containers by default. You need networking to:

- Connect services like `web ↔ db`
    
- Avoid hardcoding IPs
    
- Simulate production environments locally
    

> Understanding Docker networks = smoother development, debugging, and deployment.

---

## 🔌 Docker Networking Types

|Type|Description|
|---|---|
|`bridge` (default)|Custom bridge lets containers talk by name (ideal for most use cases)|
|`host`|Container shares host’s network stack (Linux only)|
|`none`|Fully isolated|
|`overlay`|Used in Docker Swarm (for multi-host communication)|

---

## 🕸️ 1. Bridge Networks (Default)

Every container is connected to the `bridge` network unless told otherwise.

But the **default bridge** doesn’t allow **name resolution** (i.e. `web` can’t talk to `db` by name).

---

### ✅ Solution: Create a **Custom Bridge Network**

bash

CopyEdit

`docker network create my-net`

Then launch containers with:

bash

CopyEdit

`docker run -d --name web --network my-net my-web-image docker run -d --name db --network my-net my-db-image`

Now `web` can connect to `db:5432` (or whatever port), by name — no IPs needed.

---

## ⚙️ In Docker Compose

Compose automatically creates a **custom bridge network** for all services unless you tell it otherwise.

yaml

CopyEdit

`services:   web:     build: .     depends_on:       - db   db:     image: postgres`

In this example:

- `web` can connect to `db:5432`
    
- No need to define the network manually
    

You can explicitly name the network:

yaml

CopyEdit

`networks:   default:     name: myapp-network`

---

## 🧪 Inspect Networks

bash

CopyEdit

`docker network ls          # list networks docker network inspect my-net   # see connected containers, subnet, etc.`

---

## 🔄 Connect/Disconnect Containers to Networks

You can connect a container to multiple networks:

bash

CopyEdit

`docker network connect my-net container-name docker network disconnect bridge container-name`

Useful if a service needs to talk to **multiple environments**, e.g., frontend ↔ API ↔ database.

---

## 🔒 Optional: Isolating Services

You can define multiple networks to isolate services:

yaml

CopyEdit

`networks:   frontend:   backend:  services:   frontend:     networks: [frontend]   api:     networks: [frontend, backend]   db:     networks: [backend]`

Now:

- Frontend ↔ API
    
- API ↔ DB
    
- But Frontend **can’t** talk directly to DB
    

---

## ✅ Summary

|Concept|Use for|
|---|---|
|Default bridge|Simple one-off containers|
|Custom bridge|Named containers (e.g., `db`, `web`) talk by name|
|Compose networking|Easy service-to-service comms|
|Multi-network setups|Isolate traffic (security, architecture)|

---

## 🧪 Challenge

1. Create two containers on a custom bridge: one runs an HTTP server, the other `curl`s it by name.
    
2. Add a second network and split services as shown above.
    

Let me know if you want help testing it!

---

### 🎉 **You’ve Now Completed Phase 2: Intermediate Docker**

You now know how to:

✅ Optimize Dockerfiles  
✅ Use multi-stage builds  
✅ Ignore files with `.dockerignore`  
✅ Run multi-container apps with Docker Compose  
✅ Check health with healthchecks  
✅ Connect services securely and smartly with Docker networking
