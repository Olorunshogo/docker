# Docker

Docker is an open platform for developing, shipping, and running applications. 
Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. 
By taking advantage of Docker's methodologies for shipping, testing, and deploying code, you can significantly reduce the delay between writing code and running it in production.

## The Docker platform:
Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security let you run many containers simultaneously on a given host. Containers are lightweight and contain everything needed to run the application, so you don't need to rely on what's installed on the host. You can share containers while you work, and be sure that everyone you share with gets the same container that works in the same way.

Docker provides tooling and a platform to manage the lifecycle of your containers:

+ Develop your application and its supporting components using containers.
+ The container becomes the unit for distributing and testing your application.
+ When you're ready, deploy your application into your production environment, as a container or an orchestrated service. This works the same whether your production environment is a local data center, a cloud provider, or a hybrid of the two.


## What is a Docker container?

A container is a sandboxed process running on a host machine that is isolated from all other processes running on that host machine.
That isolation leverages
[kernel namespaces and cgroups](https://medium.com/@saschagrunert/demystifying-containers-part-i-kernel-space-2c53d6979504),
 features that have been in Linux for a long time. Docker makes these capabilities approachable and easy to use.  To summarize, a container:

+ Is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI.
+ Can be run on local machines, virtual machines, or deployed to the cloud.
+ Is portable (and can be run on any OS).
+ Is isolated from other containers and runs its own software, binaries, configurations, etc.

If you're familiar with `chroot`, then think of a container as an extended version of `chroot`. The filesystem comes from the image. However, a container adds additional isolation not available when using `chroot`.

## What is an image?
A running container uses an isolated filesystem. This isolated filesystem is provided by an image, and the image must contain everything needed to run an application - all dependencies, configurations, scripts, binaries, etc. The image also contains other configurations for the container, such as environment variables, a default command to run, and other metadata.

## Build the app&apos;s image
To build the image, you'll need to use a Dockerfile. A Dockerfile is simply a text-based file with no file extension that contains a script of instructions. Docker uses this script to build a container image.

1. In the `getting-started-app directory`, the same location as the `package.json` file, create a file named `Dockerfile` with the following contents:

```dockerfile
# syntax=docker/dockerfile:1

FROM node:24-alpine
WORKDIR /app
COPY . .
RUN pnpm install --omit=dev
CMD ["node", "src/index.js"]
EXPOSE 3000
```

The `docker build` command uses the Dockerfile to build a new image. You might have noticed that Docker downloaded a lot of "layers". This is because you instructed the builder that you wanted to start from the `node:24-alpine` image. But, since you didn't have that on your machine, Docker needed to download the image.

After Docker downloaded the image, the instructions from the Dockerfile copied in your application and used `pnpm` to install your application's dependencies.

Finally, the `-t` flag tags your image. Think of this as a human-readable name for the final image. Since you named the image `getting-started`, you can refer to that image when you run a container.

The `.` at the end of the `docker build` command tells Docker that it should look for the `Dockerfile` in the current directory.


### Start an app container

Now that you have an image, you can run the application in a container using the docker run command.

1. Run your container using the docker run command and specify the name of the image you just created:

```dockerfile
$ docker run -d -p 127.0.0.1:3000:3000 getting-started
```

The `-d` flag (short for `--detach`) runs the container in the background. This means that Docker starts your container and returns you to the terminal prompt. Also, it does not display logs in the terminal.

The `-p` flag (short for `--publish`) creates a port mapping between the host and the container. The `-p` flag takes a string value in the format of `HOST:CONTAINER`, where `HOST` is the address on the host, and `CONTAINER` is the port on the container. The command publishes the container's port 3000 to `127.0.0.1:3000` (`localhost:3000`) on the host. Without the port mapping, you wouldn't be able to access the application from the host.

2. After a few seconds, open your web browser to [http://localhost:3000](http://localhost:3000). You should see your app.


### The container's filesystem
When a container runs, it uses the various layers from an image for its filesystem. Each container also gets its own "scratch space" to **create/update/remove** files. Any changes won't be seen in another container, even if they're using the same image.

#### See this in practice

To see this in action, you're going to start two containers. In one container, you'll create a file. In the other container, you'll check whether that same file exists.

1. Start an Alpine container and create a new file in it.

```bash
$ docker run --rm alpine touch greeting.txt
```

2. Run a new Alpine container and use the `stat` command to check whether the file exists.

```bash
$ docker run --rm alpine stat greeting.txt
```

You should see output similar to the following that indicates the file does not exist in the new container.

```bash
$ stat: can't stat 'greeting.txt': No such file or directory
```

The `greeting.txt` file created by the first container did not exist in the second container. That is because the writeable "top layer" of each container is isolated. Even though both containers shared the same underlying layers that make up the base image, the writable layer is unique to each container.


