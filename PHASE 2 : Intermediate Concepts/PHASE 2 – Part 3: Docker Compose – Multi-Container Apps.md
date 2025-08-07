# ✅ PHASE 2 – Part 3: **Docker Compose – Multi-Container Apps**

---

## 🧠 What is Docker Compose?

Docker Compose lets you define **multi-container applications** in a single YAML file. You can:

- Build and run everything with **`docker compose up`**
    
- Easily manage services like **web, database, redis, etc.**
    
- Share your environment with teammates with **one file**
    

---

## 🛠️ Typical Use Case

You’re building a web app that uses:

- **Flask** (or Node, Django, etc.)
    
- **PostgreSQL** (or Redis, Mongo, MySQL)
    

You want to run both containers **together**, and link them with **networking + environment variables**.

---

## 📄 Example Project Structure:

CopyEdit

`myapp/ ├── app.py ├── requirements.txt ├── Dockerfile ├── docker-compose.yml`

---

## ✍️ Step-by-Step Example

### 🔧 1. `app.py` (Flask + PostgreSQL example):

python

CopyEdit

`from flask import Flask import os import psycopg2  app = Flask(__name__)  def get_db_connection():     return psycopg2.connect(         host=os.environ["DB_HOST"],         database="postgres",         user="postgres",         password=os.environ["DB_PASSWORD"]     )  @app.route('/') def index():     conn = get_db_connection()     cur = conn.cursor()     cur.execute("SELECT 'Hello from PostgreSQL!'")     result = cur.fetchone()     cur.close()     conn.close()     return result[0]`

---

### 📄 2. `requirements.txt`:

php

CopyEdit

`flask psycopg2-binary`

---

### 📄 3. `Dockerfile`:

Dockerfile

CopyEdit

`FROM python:3.10-slim WORKDIR /app COPY requirements.txt . RUN pip install --no-cache-dir -r requirements.txt COPY . . EXPOSE 5000 CMD ["python", "app.py"]`

---

### ⚙️ 4. `docker-compose.yml`:

yaml

CopyEdit

`version: "3.9"  services:   web:     build: .     ports:       - "5000:5000"     environment:       DB_HOST: db       DB_PASSWORD: example     depends_on:       - db    db:     image: postgres:15     environment:       POSTGRES_PASSWORD: example     volumes:       - pgdata:/var/lib/postgresql/data  volumes:   pgdata:`

---

## ▶️ Run It All

From the project directory:

bash

CopyEdit

`docker compose up --build`

Then visit: [http://localhost:5000](http://localhost:5000)  
You should see: **Hello from PostgreSQL!**

---

## 🛠️ Compose Commands You Should Know

|Command|What It Does|
|---|---|
|`docker compose up`|Starts all services|
|`docker compose up --build`|Builds then starts|
|`docker compose down`|Stops and removes everything|
|`docker compose ps`|Lists running services|
|`docker compose logs`|Shows logs|
|`docker compose exec web bash`|Open shell into a service container|

---

## ✅ Benefits of Docker Compose

|Benefit|Description|
|---|---|
|Unified configuration|Everything in one `docker-compose.yml`|
|Local dev environments|Easily reproduce full stacks|
|Service discovery|Containers talk to each other by name|
|Volumes, networks, secrets|Managed easily|
|Scales well|Can use for dev, staging, or production with overrides|

---

## 🧪 Challenge

1. Add a Redis service to your app
    
2. Pass `REDIS_HOST=redis` as an environment variable
    
3. Use `redis-py` to connect and store visit counts
    

Let me know if you want help scaffolding it — it’s a great real-world exercise.

---

### ✅ Summary

You now know how to:

- Define multiple services (app, db, etc.)
    
- Link them with Compose
    
- Use `docker compose` commands to build, run, and manage them



NB:// This is an often-overlooked feature, but **super valuable** in real-world Docker setups.