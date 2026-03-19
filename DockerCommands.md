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

```bash

# Docker enable: 
$ sudo systemctl enable docker

# Docker start: 
$ sudo systemctl start docker

# Docker verify: 
$ sudo systemctl status docker


# Docker build: 
$ docker build -t getting-started .`

# Docker run: (-d detached mode, -p port mapping)
$ docker run -d -p 3000:3000 getting-started 

# Docker stop: 
$ docker stop <container_id>
$ docker rm <the-container-id>
$ docker rm -f <the-container-id>

# Docker ps:
$ docker ps

# Docker List all inclusing stopped:
$ docker ps -a

# Docker rm: 
$ docker rm <container_id>


## Images
Docker images:

$ docker images
$ docker images ls
$ docker images -a
$ docker rmi IMAGE_ID
$ docker rmi getting-started-image
$ docker rm -f $(docker ps -aq)

# Force delete: 
$ docker rmi -f IMAGE_ID

# Remove all ununsed images: 
$ docker image prune -a


# Live/Run
$ docker build -t getting-started .
$ docker run -dp 127.0.0.1:3000:3000 getting-started
$ docker run -dp 127.0.0.1:3001:3000 getting-started



# Cleanup

## If you ever want a super-clean slate without touching getting-started, you could do:

# Remove all stopped containers
$ docker container prune -f

# Remove all dangling/unreferenced images
$ docker image prune -a -f

# List all docker contexts
$ docker context ls

# Check all containers in the default context
$ docker ps -a --no-trunc

# Rebuilding image (no cache)
$ docker build --no-cache -t getting-started .


## Debugging 

# See the logs of the container (live error logs)

$ docker run -it getting-started
$ docker volume inspect todo-db
$ docker logs <container-id>
$ docker image history getting-started

## Sharing

$ docker login showgo001
$ docker tag getting-started showgo001/getting-started
$ docker push showgo001/getting-started



$ docker run -it --mount type=bind,src=.,target=/src ubuntu bash

# Multi container apps
$ docker run -dp 127.0.0.1:3000:3000 \ -w /app --mount type=bind,src=.,target=/app \ node:24-alpine \ sh -c "npm install && npm run dev"
docker run -dp 127.0.0.1:3000:3000 \
  -w /app -v ".:/app" \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node:24-alpine \
  sh -c "npm install && npm run dev"




```
