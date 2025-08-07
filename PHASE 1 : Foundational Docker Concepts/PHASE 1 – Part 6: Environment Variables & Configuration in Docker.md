## ✅ PHASE 1 – Part 6: **Environment Variables & Configuration in Docker**

---

### 🧠 Why This Matters

Hardcoding things like:

- API keys
    
- Database credentials
    
- Environment-specific settings (e.g., dev vs prod)
    

…is a **bad idea**.

Instead, we pass them into containers using **environment variables** — a clean, secure, and scalable approach.

---

## ✅ 1. Pass Environment Variables via CLI

You can pass env vars directly when running a container:

bash

CopyEdit

`docker run -e ENV_NAME=value your-image`

🔧 Example:

bash

CopyEdit

`docker run -e GREETING="Hello from Docker!" busybox env`

This runs `env` inside the container and prints all environment variables — including the one you passed.

---

## ✅ 2. Read Env Vars in Your App

### Python Example:

python

CopyEdit

`import os greeting = os.getenv("GREETING", "Hello by default") print(greeting)`

Try it in a Dockerized app:

bash

CopyEdit

`docker run -e GREETING="Dockerized World" your-python-image`

It’ll print: `Dockerized World`.

---

## ✅ 3. Use `.env` Files (Docker Compose Friendly)

Create a file called `.env`:

ini

CopyEdit

`GREETING=Hello from .env SECRET_KEY=supersecret123`

Then use it with `--env-file`:

bash

CopyEdit

`docker run --env-file .env your-image`

Or, with **Docker Compose** (coming soon in your roadmap), `.env` is used automatically.

---

## ✅ 4. Set Default Environment Vars in Dockerfile (optional)

Inside your Dockerfile, you can define defaults:

Dockerfile

CopyEdit

`ENV PORT=5000 ENV ENVIRONMENT=development`

Then you can override at runtime.

---

## ✅ 5. Secret Management (Don't Do This in Production)

Sometimes, beginners accidentally **bake secrets into Docker images**:

Dockerfile

CopyEdit

`ENV DB_PASSWORD=hardcoded123  ❌`

Never do that — anyone with access to the image can extract that value.

Instead, use runtime env vars or a secret manager (e.g., AWS Secrets Manager, Docker Swarm secrets, HashiCorp Vault).

---

## 🧪 Hands-on Mini Project

Here’s a practical test:

1. Modify your **Flask app** from earlier to read a `GREETING` env var:
    

python

CopyEdit

`from flask import Flask import os  app = Flask(__name__) GREETING = os.getenv("GREETING", "Hello!")  @app.route('/') def hello():     return GREETING`

2. Build the Docker image:
    

bash

CopyEdit

`docker build -t flask-env-app .`

3. Run the container with a custom greeting:
    

bash

CopyEdit

`docker run -d -p 5000:5000 -e GREETING="Hi from env!" flask-env-app`

Visit: `http://localhost:5000`  
→ You’ll see: **Hi from env!**

---

## ✅ Summary

| Method              | Description                      |
| ------------------- | -------------------------------- |
| `-e` or `--env`     | Pass env var via CLI             |
| `--env-file`        | Load from file                   |
| `ENV` in Dockerfile | Set defaults (can be overridden) |
| `.env`              | Common practice in Compose       |
| Secret managers     | Use in production for security   |
