# Docker

## Introduction to Docker Containers
- Docker containers allow us to package up our application and system images into one package for easy deployment and distribution.
- Docker consists of two main pieces - Docker images and Docker containers.
- You build an image first, then run it inside of a container.
- Let's take a look at what [the documentation](https://docs.docker.com/introduction/understanding-docker/) says about how this process works.

##### Sample Docker File:

```
FROM ubuntu:14.04

RUN apt-get update

RUN apt-get install -y nodejs npm git git-core

# Bundle app source
COPY . /src

# Install app dependencies
RUN cd /src; npm install

# Run app on specified port
EXPOSE 3000

CMD ["nodejs", "/src/app.js"]
```

## Build Docker Image
- When your app is ready to be packaged and you have created the Dockerfile, you can build the image like so:

```
docker build -t arsood/docker_hello_world .
```

## Run Docker Container
- After the image is built, you can run your image inside of a container.
- The `-p` flag tells the app to map the container port 3000 over to port 80 of the host.
- The `-d` flag runs the container in detached mode, stopping the process from accessing STDOUT, STDIN, and STDERR in the console.

```
docker run -p 80:3000 -d arsood/docker_hello_world
```

## Remove Docker Container
- [Documentation here](https://docs.docker.com/reference/commandline/rm/)

```
docker rm $(docker ps -a -q)
```

## Remove Docker Image
- [Documentation here](https://docs.docker.com/reference/commandline/rmi/)

```
docker rmi arsood/docker_hello_world
```

## Sharing Docker Images
- When your images are ready to be shared, they can be uploaded (pushed) to [Docker Hub](https://hub.docker.com/).
- You can create private or public Docker images that can be shared similar to code on GitHub.
- First step is to login with Docker Hub:

```
docker login --username=jsmith --password=myPassword --email=john@smith.com
```

- If there is a tag associated with your image it can be pushed like so:

```
docker push arsood/docker_hello_world
```

- If you want to run an image from another user simply use the `docker pull <image name>` command.

## Docker Lab
- In this lab we will be building images in pairs and learning how to pull them and run them.
- In partners, each of you should create a simple Node application using Express that prints out "Hello <partner's name>!" on the root route.
- Each of you will then push your images to Docker Hub.
- You will then pull each other's image and run it on your virtual machine.

## Putting It Together Lab
- For this lab we are going to put all of the concepts we learned together by building an application with the full MEAN stack.
- We will be using the HTML already created for you [here](chirp_html/).
- Your goal is to create an API with Node that will render JSON from a Mongo database.
- The application will render that JSON into the template through Angular.
- We will work on this application in a series of steps:
	- Step 1: Connect the views to the application as EJS files.
	- Step 2: Create the API in Node that performs CRUD operations on a Chirp.
	- Step 3: Activate the functionality via the $http module in Angular.
	- **Bonus:** When the edit button is clicked for a chirp, show the same bird on the edit page.

##### Starter code: To render JSON from Node

```javascript
res.json(userData);
```