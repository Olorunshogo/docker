- Use the following command to stop the Docker Engine service:

```bash
$ sudo systemctl stop docker docker.socket containerd
```

- Depending on your installation, the Docker Engine may be configured to automatically start as a system service when your machine starts. Use the following command to disable the Docker Engine service, and to prevent it from starting automatically:

```bash
$  sudo systemctl disable docker docker.socket containerd
```

#. To build an image from the Dockerfile, run the following command from the `getting-started-app` directory:

```dockerfile
# syntax=docker/dockerfile:1

FROM node:24-alpine
WORKDIR /app
COPY . .
RUN npm install --omit=dev
CMD ["node", "src/index.js"]
EXPOSE 3000

```

+ Docker enable: sudo systemctl enable docker
+ Docker start: sudo systemctl start docker
+ Docker verify: sudo systemctl status docker


+ Docker build: `docker build -t getting-started .`
+ Docker run: `docker run -d -p 3000:3000 getting-started` -d detached mode, -p port mapping
+ Docker stop: `docker stop <container_id>`
+ Docker ps: `docker ps`
+ Docker List all inclusing stopped: `docker ps -a`
+ Docker rm: `docker rm <container_id>`

## Images
docker image

+ docker images
+ docker images ls
+ docker images -a
+ docker rmi IMAGE_ID
+ docker rmi getting-started-image
+ docker rm -f $(docker ps -aq)
+ Force delete: docker rmi -f IMAGE_ID
+ Remove all ununsed images: docker image prune -a


Step 3 (Optional): Cleanup everything else

If you ever want a super-clean slate without touching getting-started, you could do:

```bash
# Remove all stopped containers
docker container prune -f

# Remove all dangling/unreferenced images
docker image prune -a -f

# List all docker contexts
docker context ls

# Check all containers in the default context
docker ps -a --no-trunc

# Rebuilding image (no cache)
docker build --no-cache -t getting-started .
```


## Debugging 

```dockerfile
# See the logs of the container (live error logs)
$ docker run -it getting-started
```


