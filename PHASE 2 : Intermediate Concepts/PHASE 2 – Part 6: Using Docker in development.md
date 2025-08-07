🚀 Using Docker in Development: Live Reload & Volume Mounts
Why it matters
In development, you want to:

See your code changes immediately reflected in the running container

Avoid rebuilding the image on every small change

Easily debug and test locally

Docker’s volume mounts and live reload tools make this workflow seamless.

1. Volume Mounts: Link your code locally to the container
Instead of copying your code into the image (which requires rebuilds), you can mount your project directory into the container.

Example: Node.js app
bash
Copy
Edit
docker run -it -p 3000:3000 -v $(pwd):/app -w /app node:20 bash
-v $(pwd):/app mounts current directory to /app inside container

-w /app sets working directory

You can run npm start inside container and it uses your local files

2. Live Reload with Nodemon (Node.js) or Equivalent
Instead of restarting your container or rebuilding images on every code change, use a file watcher like nodemon:

Install nodemon:

bash
Copy
Edit
npm install --save-dev nodemon
Modify your package.json start script:

json
Copy
Edit
"scripts": {
  "start": "nodemon app.js"
}
Run the container with volume mount:

bash
Copy
Edit
docker run -it -p 3000:3000 -v $(pwd):/app -w /app node:20 npm start
Now, nodemon inside the container watches for file changes and restarts your app automatically.

3. Docker Compose for Development
Simplify this setup in docker-compose.yml:

yaml
Copy
Edit
version: '3.9'
services:
  web:
    image: node:20
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    working_dir: /app
    command: npm start
Run:

bash
Copy
Edit
docker compose up
4. Benefits
No rebuilds for every code change

Consistent environment for all devs

Easy integration with debugging tools and IDEs

Fast iteration and testing