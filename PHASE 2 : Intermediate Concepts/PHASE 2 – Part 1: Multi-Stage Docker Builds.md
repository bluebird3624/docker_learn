# ✅ PHASE 2 – Part 1: **Multi-Stage Docker Builds**

---

### 🧠 Why Use Multi-Stage Builds?

As your apps grow, your Docker images may:

- Get **huge** (unnecessary files, dev tools, source code).
    
- Contain **sensitive files** (e.g. `.env`, API keys).
    
- Take **long to build** and push.
    

**Multi-stage builds** let you:

- Use one image to build your app (with build tools),
    
- And a second (slim) image to run it — clean and lightweight.
    

> 🎯 Goal: **Build in one stage, copy only what you need to another.**

---

### 🧱 Basic Example: Go App (compiles to binary)

Let's say you have a Go app in `main.go`.

go

CopyEdit

`package main import "fmt" func main() { 	fmt.Println("Hello from Go!") }`

---

### 🧾 Multi-stage `Dockerfile`:

Dockerfile

CopyEdit

`# ---- First Stage: Build ---- FROM golang:1.20 AS builder WORKDIR /app COPY . . RUN go build -o app  # ---- Second Stage: Final ---- FROM debian:bullseye-slim WORKDIR /app COPY --from=builder /app/app .  CMD ["./app"]`

---

### 🛠️ Build & Run:

bash

CopyEdit

`docker build -t go-app . docker run --rm go-app`

You’ll get:

bash

CopyEdit

`Hello from Go!`

✅ The final image only contains the binary — not Go, not source code.

---

## 🧪 Let’s Try It with Python or Node.js

For interpreted languages like Python, multi-stage is useful for:

- Removing test files
    
- Skipping dev dependencies
    
- Building assets (e.g., frontend) separately
    

---

### 📦 Example: Node.js App with Production-Only Install

Dockerfile

CopyEdit

`# --- Build Stage --- FROM node:20 AS build WORKDIR /app COPY package*.json ./ RUN npm install COPY . .  # --- Production Stage --- FROM node:20-slim WORKDIR /app COPY --from=build /app ./ RUN npm prune --production  CMD ["node", "server.js"]`

This:

- Installs **all** deps in the build stage,
    
- Copies only what's needed into a minimal final image,
    
- Prunes away dev dependencies.
    

---

## 🧪 Optional Challenge

Take any Dockerfile you’ve written and split it into two stages:

1. One for installing/building.
    
2. One for running.
    

Let me know if you want help adapting it.

---

## ✅ Summary

|Benefit|Multi-Stage Builds|
|---|---|
|Smaller image size|✅|
|No build tools in prod|✅|
|Clear separation|✅|
|Better security|✅|
