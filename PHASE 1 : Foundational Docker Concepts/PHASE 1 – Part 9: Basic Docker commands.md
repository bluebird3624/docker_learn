🐳 Phase 1 - Part 8: Basic Docker Commands
1. docker build
Builds a Docker image from a Dockerfile.

bash
Copy
Edit
docker build -t image_name:tag .
-t tags the image (name:tag).

. is the build context (current directory).

2. docker run
Runs a container from an image.

bash
Copy
Edit
docker run [options] image_name:tag
Common options:

-d : Run in background (detached)

-it : Interactive terminal (attach stdin/stdout)

--name : Assign a container name

-p host_port:container_port : Port mapping

-v host_dir:container_dir : Volume mount

Example:

bash
Copy
Edit
docker run -it --name mycontainer -p 8080:80 nginx
3. docker exec
Run a command inside a running container.

bash
Copy
Edit
docker exec -it container_name bash
-it interactive terminal

Use sh if bash isn’t installed.

4. docker ps
List running containers.

bash
Copy
Edit
docker ps
Add -a to show all containers (running + stopped):

bash
Copy
Edit
docker ps -a
5. docker logs
View logs (stdout/stderr) of a container.

bash
Copy
Edit
docker logs container_name
Follow logs live:

bash
Copy
Edit
docker logs -f container_name
6. docker stop
Stop a running container gracefully.

bash
Copy
Edit
docker stop container_name
7. docker rm
Remove a stopped container.

bash
Copy
Edit
docker rm container_name
Add -f to force remove a running container (kills it):

bash
Copy
Edit
docker rm -f container_name
8. docker rmi
Remove an image.

bash
Copy
Edit
docker rmi image_name:tag
Add -f to force remove an image if it’s being used:

bash
Copy
Edit
docker rmi -f image_name:tag
🔥 Quick Reference Table
Command	What it does
docker build	Build image from Dockerfile
docker run	Run container from image
docker exec	Run command inside running container
docker ps	List running containers
docker ps -a	List all containers
docker logs	View container logs
docker stop	Stop a running container
docker rm	Remove a container
docker rmi	Remove an image