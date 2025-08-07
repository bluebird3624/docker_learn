# ✅ PHASE 2 – Part 4: **Docker Healthchecks — Self-Aware Containers**

---

## 🧠 Why Healthchecks Matter

In complex applications, it’s not enough for a container to just be **running** — it also needs to be **working** (healthy).

For example:

- A database container is running, but hasn’t finished initializing.
    
- A web server container is live, but returning 500 errors.
    
- A Redis cache container is running, but unreachable due to config errors.
    

> Docker **healthchecks** let you detect these situations automatically.

---

## 🚦 What Is a Healthcheck?

A **healthcheck** is a command Docker runs **inside the container** on a regular interval.

If the check:

- returns `0` → **Healthy**
    
- returns non-zero → **Unhealthy**
    

Docker tracks the health and you can **delay dependent services** until everything is healthy.

---

## 🔍 Where to Define a Healthcheck

You can define it in:

- `Dockerfile`
    
- `docker-compose.yml`
    

Let’s see both.

---

## ✍️ Dockerfile Example (Node.js App):

Dockerfile

CopyEdit

`HEALTHCHECK --interval=30s --timeout=10s --start-period=10s --retries=3 \   CMD curl -f http://localhost:3000/health || exit 1`

This will:

- Start checking after 10 seconds.
    
- Run the check every 30 seconds.
    
- If it fails 3 times, marks the container as **unhealthy**.
    

---

## 🔧 Flask Healthcheck Example

In your Flask app:

python

CopyEdit

`@app.route('/health') def health():     return "OK", 200`

Then in Dockerfile:

Dockerfile

CopyEdit

`HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit 1`

---

## ✅ docker-compose.yml Example

You can define the same thing there:

yaml

CopyEdit

`services:   web:     build: .     ports:       - "5000:5000"     healthcheck:       test: ["CMD", "curl", "-f", "http://localhost:5000/health"]       interval: 30s       timeout: 10s       retries: 3       start_period: 10s`

---

## 🔍 Check Container Health

bash

CopyEdit

`docker inspect --format='{{json .State.Health}}' <container_name>`

Or use:

bash

CopyEdit

`docker ps`

You’ll see:

makefile

CopyEdit

`STATUS: Up 2 minutes (healthy)`

or

makefile

CopyEdit

`STATUS: Up 2 minutes (unhealthy)`

---

## ⚠️ Bonus: Use with `depends_on` in Compose

yaml

CopyEdit

`services:   web:     ...     depends_on:       db:         condition: service_healthy`

> So your web service won’t start until `db` is healthy — very useful!

---

## 🧪 Challenge

1. Add `/health` route to your Flask or Node.js app.
    
2. Add a `HEALTHCHECK` in Dockerfile or Compose.
    
3. Deliberately break the endpoint and see how Docker marks it unhealthy.
    

---

## ✅ Summary

|Feature|Purpose|
|---|---|
|`HEALTHCHECK`|Track if container is "working"|
|`docker ps`|See health status|
|`depends_on: condition: service_healthy`|Wait until service is ready|
