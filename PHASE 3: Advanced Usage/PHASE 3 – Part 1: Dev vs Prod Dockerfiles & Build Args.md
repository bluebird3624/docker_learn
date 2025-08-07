# ✅ PHASE 3 – Part 1: **Dev vs Prod Dockerfiles & Build Args**

---

## 🧠 Why Different Dockerfiles for Dev and Prod?

- **Dev images** often include:
    
    - Debugging tools
        
    - Source code with live reload
        
    - Dev dependencies
        
- **Prod images** focus on:
    
    - Small size
        
    - Security (no secrets, no dev tools)
        
    - Performance optimizations
        

---

## 🛠️ Common Approaches

### 1. **Separate Dockerfiles**

- `Dockerfile.dev` for development
    
- `Dockerfile` (or `Dockerfile.prod`) for production
    

You choose which one to build:

bash

CopyEdit

`docker build -f Dockerfile.dev -t myapp:dev . docker build -f Dockerfile -t myapp:prod .`

---

### 2. **Single Dockerfile with Build Arguments**

You can also use build args to switch behavior:

Dockerfile

CopyEdit

`ARG ENV=prod  FROM node:20  WORKDIR /app  COPY package*.json ./  RUN if [ "$ENV" = "dev" ]; then npm install; else npm install --production; fi  COPY . .  CMD ["node", "server.js"]`

Build for dev:

bash

CopyEdit

`docker build --build-arg ENV=dev -t myapp:dev .`

Build for prod:

bash

CopyEdit

`docker build -t myapp:prod .`

---

## 🧪 Exercise

Try creating two Dockerfiles for a simple Node.js or Python app:

- One with dev dependencies and hot reload
    
- One slim for production
    

---

## ✅ Summary

|Approach|Pros|Cons|
|---|---|---|
|Separate Dockerfiles|Clear separation|More files to maintain|
|Build args in one file|DRY, flexible|More complex Dockerfile|
