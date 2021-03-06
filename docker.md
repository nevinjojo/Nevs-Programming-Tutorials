# Docker Tutorial for beginnners
## What is docker?
Docker is a tool that allows easy deployment of applications in a sandbox (called containers) to run on the host operating system (Linux). The key benefit of Docker is that it allows users to package an application with all of its dependencies into a standardized unit for software development. Unlike VMs, containers do not have the high overhead and hence enable more efficient usage of the underlying system and resources.

In simple english, Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package.

Still confused? Read [this](https://medium.com/@yannmjl/what-is-docker-in-simple-english-a24e8136b90b).

## Terminology: Docker Containers
Unlike VM (which hosts applications in an isolated virtual hardware using high computational power), containers leverage the low-mechanics of the host OS to host the application for a fraction of the computing power.

Containers offer a logical packaging mechanism in which applications can be abstracted from the environment in which they actually run. This decoupling allows container-based applications to be deployed easily and consistently.

An exited container is not running, but that does not mean it has become an image: it can be restarted and will retain all settings and any filesystem changes.

### ...What?
Still confused? Here's an example.
Suppose you want to test the same instance of your app multiple times. Instead of hosting each operating system per each application, some common resources can be shared, and there is something called “docker engine,” which sits on top of an Operating System as shown below.

![Docker v/s VM!](docker_vs_vm.jpeg)

## Installing Docker
Assuming you have `brew` installed:
`brew cask install docker`

Learn more about what you are installing by installing docker this way:
```
brew install docker
brew install docker-machine
brew services start docker-machine

docker-machine create default # Assuming you already have Virtualbox installed

# Create "docker" group and put me in it
sudo dseditgroup -o create docker
sudo dseditgroup -o edit -a price -t user docker

# Run this to set environment before running docker
eval $(docker-machine env default)
```

* Check if you have the latest version: `docker version`
* Run `docker run hello-world` to verify that Docker is pulling images and running as expected.
### Other Brew Commands
`brew update`            - Fetch latest version of homebrew and formula.
`brew search docker`     - Searches all known Casks for a partial or exact match.
`brew cask info docker`  - Displays information about the given Cask
`brew cleanup`           - Remove any older versions from the cellar.

### Terminology: Docker Image
A docker image is a file, comprised of multiple layers, used to execute code in a Docker container. 

### English please
An image is like a blueprint, a basis for creating — just one, or as many as you like — brand-new containers. On its own, an image is an immutable file, meaning that images do not do anything, and cannot be changed (unless you start a container from an image, perform operations in it, and then save a brand new image based on the latest state of the container).

### Analogy
Think of it this way: an image is like a powered-down computer. Starting up a container is like creating a brand new exact copy of that computer, software and all, only one that is running. The original computer (the image) remains on your desk, still powered down, while the new one (the container) hums away busily with its assigned tasks. Or, in other words, if an image is running, then it is a container.

### Image types
* Base Image - images that have no parent image, usually images with an OS like ubuntu, busybox or debian.
* Child Image - images that build on base images and add additional functionality.
* Official Image - images that are officially maintained and supported by the folks at Docker.These are typically one word long. In the list of images above, the 'python', 'ubuntu', 'busybox' and 'hello-world' images are official images.
* User Image - images created and shared by users. Typically, these are formatted as `user/image-name`.

## Docker Commands
### Develop
* `docker run [image]` - Create a new container from a particular image.
* `docker login --username=yourhubusername --email=youremail@company.com` - Log into the Docker Hub repository.
* `docker pull [image]` - Pull an image from the Docker Hub repository.
* `docker push [username/image]` - Push an image to the Docker Hub repository.

### Run
* `docker start [container]` - Start a particular container.
* `docker stop [container]` - Stop a particular container.
* `docker exec -ti [container] [command]` - Run a shell command inside a particular container.
* `docker run -it [image] [container] [command]` - Create and start a container at the same time, and then run a command inside it.
* `docker run -it image sh` - Access the docker using an interactive tty so that you can run multiple commands on the docker via command line.
* `docker exit` - if you are already inside the tty of the docker, it will exit the docker.
* `docker tag [old-site-name]:latest [new-site-name]:latest` and `docker rmi [old-site-name]` - create copy of an image and remove original image.

### View
* `docker history [image]` - Display the history of a particular image.
* `docker images` or `docker image ls`- Lists all of the images that are currently stored on the system.
* `docker ps` or `docker ps -a` - Lists all of the containers that are currently running.
* `docker version` - Display the version of Docker that is currently installed on the system.
* `docker container ls -a` - Lists all active and inactive containers.

### Clear
* `docker kill [container]` - Kill a particular container.
* `docker rm [container]` - Delete a particular container that is not currently running.
* `docker rm $(docker ps -a -q)` - Delete all containers that are not currently running.
* `docker container rm [containerID1] [containerID2]` - removes one or more containers.
* `docker container prune` - remove all stopped containers (WARNING: Don't do it unless you are sure you don't need it!).
* `docker image prune -a` - removes all images unreferenced by any containers (WARNING: Again, make sure you don't need it!).
* `docker rmi imageID1 imageID2` - removes docker images based on their IDs if they are not being run atm.


Visit the [documentation page](https://docs.docker.com/engine/reference/commandline/docker/) to learn more about docker commands.


## Running Web apps using Docker
If you have an image stored in a docker hub repository, you can pull it using `docker pull usernam/repo`.

To get the feel of how webapp can be run using docker follow commands below:
* `docker run --rm prakhar1989/static-site` - downloads the image from docker hub (`--rm` deletes the container if it already exists).
* `docker run -d -P --name static-site prakhar1989/static-site` - `-d` will detach our terminal, `-P` will publish all exposed ports to random ports and finally `--name` corresponds to a name we want to give
* `docker port static-site` - lists the ports used.
* `docker run -p 8888:80 prakhar1989/static-site` - executes nginx from the container.
* `docker stop static-site` - stops the detached container.


## Terminology: Volumes
Containers are designed to leave nothing behind. But what about having data persist? This is where volumes come in.

When starting up a Docker container you can specify directories as mount points for volumes, which are repositories for shared or persistent data that remain even if a container gets removed. When a container is exited, any volumes it was using persist — so if you start a second container it can use all the data from the previous one.

## Terminology: Registry
The registry is the central distribution point for deploying Docker containers. You can orchestrate distribution directly from your Docker host. The registry works basically like a git repository, allowing you to push and pull container images.

## Build your own Docker Image
Clone the following repo
`git clone https://github.com/prakhar1989/docker-curriculum`

Inside the `flask-app` directory there is a file called `Dockerfile`. What is a Dockerfile?...

### Terminology: Dockerfile
A Dockerfile is a simple text file that contains a list of commands the Docker client calls (on the command line) when assembling an image. This essentially automates the image creation process because these special files are, basically, scripts — a set list of commands/instructions and arguments that automatically perform actions on a chosen base image.

To create the image, add the following to a new file in the directory called Dockerfile:
```
# our base image
FROM python:3-onbuild

# specify the port number the container should expose
EXPOSE 5000

# run the application
CMD ["python", "./app.py"]
```

In the Dockerfile within flask-app you should find the following commands:
* `FROM python:3-onbuild` - specifies the base image.
* `EXPOSE 5000` - specify thr port in which the flask app is running.
* `CMD ["python", "./app.py"]` - command for runnning the python application. 


### Build the image using Dockerfile
Assuming you are still running your webapp using `docker run -p 8888:80 [image name]`
Run the Dockerfile using `docker build -t [docker-hub-username]/catnip .` where `-t` is an optional tag name that will create the image in the same directory where the Dockerfile exist.

Run `docker images` to check if the image had been created.

Run docker run -p 8888:5000 [docker-hub-username]/catnip` to run the image you created. The command we just ran used port 5000 for the server inside the container, and exposed this externally on port 8888.

Head over to `http://192.168.99.100:8888` to view your live app.

If you push your image to a public docker repository, by running `docker push [username]/catnip`, anyone who has docker installed can play with your app by typing just a single command:
`docker run -p 8888:5000 [username]/catnip`

## Docker & AWS: Name a more iconic duo, I'll wait.
AWS provides you with an option of hosting single-container Docker environments. This will allow you to host your application by simply uploading your Dockerrun.aws.json file to the Elastic Beanstalk (EB) application. The Dockerrun.aws.json file will acquire the image from your AWS repository (assuming it is a public repository) and create a container in the EB instance with your application in it.

### How to launch your app using AWS
The rest of the instructions will be based on the assumption that you have an AWS account set up already.

#### Terminoloogy: Beanstalk
AWS Elastic Beanstalk (EB) is a PaaS (Platform as a Service) offered by AWS. As a developer, you just tell EB how to run your app and it takes care of the rest - including scaling, monitoring and even updates. Beanstalk supports running single-container Docker deployments (which is what we'll be using). 

#### Create new application using EB
* Login to your AWS console.
* Click on Elastic Beanstalk. It will be in the compute section on the top left.
* Click on "Create New Application" in the top right
* Give your app a name and provide an (optional) description. The name has to be unique.
* In the New Environment screen, create a new environment and choose the Web Server Environment.
* Fill in the environment information by choosing a domain.
* Under base configuration section, choose Docker from the predefined platform.
* Open the Dockerrun.aws.json file located in the flask-app folder and edit the Name of the image to your image's name: changed `"Name": "prakhar1989/catnip"` to `[your-image-name]`.
* When you are done, click on the radio button for "upload your own" and choose this file.
* Once you complete the setup, it should take a while for the image to be setup into the application.

On the EB application page on AWS, there should be a URL, which should direct you to the application you just deployed using Docker & AWS Elastic Beanstalk. Nice job!






