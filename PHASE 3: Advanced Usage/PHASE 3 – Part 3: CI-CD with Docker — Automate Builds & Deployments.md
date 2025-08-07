# ✅ PHASE 3 – Part 3: **CI/CD with Docker — Automate Builds & Deployments**

---

## 🧠 Why CI/CD with Docker?

- Automate image builds on code push
    
- Run tests inside containers
    
- Push images to registries (Docker Hub, ECR, GCR)
    
- Deploy to staging/production with minimal manual steps
    

---

## 🛠️ Common CI/CD Tools & Docker Integration

- **GitHub Actions** (free tiers, super popular)
    
- **GitLab CI/CD**
    
- **Jenkins**
    
- **CircleCI**
    
- **Travis CI**
    

---

## 📦 Example: GitHub Actions Workflow to Build & Push Docker Image

Create `.github/workflows/docker.yml` in your repo:

yaml

CopyEdit

`name: Build and Push Docker Image  on:   push:     branches:       - main  jobs:   build:     runs-on: ubuntu-latest      steps:       - name: Checkout code         uses: actions/checkout@v3        - name: Log in to Docker Hub         uses: docker/login-action@v2         with:           username: ${{ secrets.DOCKER_USERNAME }}           password: ${{ secrets.DOCKER_PASSWORD }}        - name: Build the image         run: docker build -t yourusername/myapp:latest .        - name: Push the image         run: docker push yourusername/myapp:latest`

---

## 🔐 Secrets Management

- Store your Docker Hub username and password in **GitHub Secrets** (`Settings > Secrets`).
    
- Never hardcode passwords in workflows.
    

---

## 🧪 Exercise

1. Create a GitHub repo with a Dockerized app.
    
2. Add the workflow above.
    
3. Push to main and watch GitHub Actions build & push your image.
    

---

## ✅ Benefits of Docker CI/CD

|Benefit|Description|
|---|---|
|Automated builds|No manual Docker builds|
|Consistent environments|Same image tested & deployed|
|Easy rollbacks|Tagged images|
|Integrate testing & linting|Build fails if tests fail|
