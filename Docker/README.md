# Docker for Developers
Contains the resources and notes from various Courses

## 1. Introduction

* its famous for its ability to easily setup the programming environment on local system.


* A Container is where the sepcific resource is located. 
* Image is like a recipe where all the application details are present
* Volume contains the data of these resources
* These three are ideally seperated.

## 2. Installation
* Docker Application can be found at docker.com
* Mongo DB community edition can be found at https://www.mongodb.com/try/download/community 
* Also better to have an account at docker hub

## 3. Docker Image and commnads

* Open the code files and add them in the folder
* create two files ".dockerignore" and Dockerfile
* In the dockerignore, add the things that need to be ignored.
* Add the commands and relevant details in docker file.

* cd to simple-backend folder and run the commands in the terminal

```
docker build -t vsuhas9/simple-backend .

docker images (to list the image details and IDs)

docker rmi Image ID (removes the image)

docker run -p 4000:4000 vsuhas9/simple-backend (maps the docker application 4000 to system port 4000)

docker ps (lists the containers)

docker stop vsuhas9/simple-backend
docker stop dfee

docker kill Image ID (Force kill)

docker push vsuhas9/simple-backend

docker pull vsuhas9/simple-backend
```

## 4. Docker Approach for a Full Stack Application


### A. Backend 

* All the frontend/backend assets would be on a container
* The dynamic part which is data would be Volume (data of the database)

* Compose is a file for handling docker steps for the entire setup

* The below code shows a simple docker-compose file for the backend server.

* For this to run correctly, docker desktop and mongodb community must be running and change the terminal path to backend folder

```
version: "1" (version is for docker engine's version )
services: (different componenets of the system)
  app:
    container_name: app (name of this app)
    restart: always (to ensure it restarts in case it fails)
    build: .
    ports:
      - "4000:4000" (maps docker port to system port)
    links:
      - mongo (this container is linked with another container called mongo)
  mongo:
    container_name: mongo
    image: mongo (this will download from docker hub)
    volumes:
      - ./data:/data/db (to store the data)
    ports:
       - "27017:27017" (default ports)
```

* Now the commands to  be used are as follows

```
docker-compose build (builds all the files and downloads the necessary things)

docker compose up (will run everythig  but might cause errors)

docker compose up -d mongo (This runs first)

docker compose up -d app (runs next)

docker ps (reveals the running instances)

docker logs container-id-number (shows the logs of the this application)

docker-compose stop

```

### B. FrontEnd

* Simillarly for frontend, we can use the below commands after cd to fronend folder

```
docker build -t vsuhas9/frontend .

docker run -p 3000:3000 vsuhas9/frontend

```

### C. FullStack

* For Fullstack file, the backend goes to api, frontend goes to client.

* The below is how the docker compose file looks like for api and client together


```
version: "3.9"
services:
  client:
    container_name: client
    restart: always
    build: ./client
    ports: 
      - "3000:3000"
    links:
      - app
  app:
    container_name: app
    restart: always
    build: ./api
    ports:
      - "4000:4000"
    depends_on:
      - mongo
  mongo:
    container_name: mongo
    image: mongo
    restart: always
    expose:
      - "27017"
    volumes:
      - ./data:/data/db
    ports:
       - "27017:27017"
```

* We start running the system from mongo first then to api(app) and finally to client.

* cd to frontend folder and follow the commands

```
docker-compose build 

docker-compose up -d mongo

docker-compose up -d app

docker-compose up -d client

docker-compose stop (stops everything if needed)
```

* There could be error which says, the mongo or app are already running. These can be terminated first and later started as well.


## 5. For Different Languages

* DockerFile formats/imports can be found docker hub.


## 6. Swarm

* Swarm allows you cluster and manage many containers together.

### Creating a Swarm and managing them

* First we need to create anode which becomes swarm manager

* We ssh to the target machine (with docker installed) and run the docker swarm command

```
docker swarm init (initialises the docker swarm manager)

Executing this will give the below command as the output which is ran on the target servers.

docker swarm join --token SwarmToken ipaddres:port (This results in a singel node swarm)

docker info

docker node ls (gives the swarm nodes)

docker service create --replica 1 --name nodeserver2 node ping docker.com (adds a service from docker hub)

docker service ls
```

## 7. Kubernetes

* Here the size can be very large than a swarm

* This is more customizable than swarms

* Download can be found at kubernates.io (https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)

* Add the kubectl in the path variable

```
kubectl version --client (THis can be used to check the version of the installation)
```

* minikube is used to handle the kubernetes(https://minikube.sigs.k8s.io/docs/start/).

* In the kubernetes folder open powershell (ps) and type

```
New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
```

### Setting up the kubernetes cluster

* run the below command
```
minikube start
```

* if this throws an error saying default docker context is not found then try the below and try the minikube start command again

```
docker context use default
```

### Deploying the app to Kubernetes Cluster

```
kubectl create deployment nodeapplication2 --image=node

This deploys node image to docker hub instead the urtl to application can also be given.

kubectl get deployments
(This gives the list of current deployments)
```

## 8. CI and deployment Use Case

* Typically CI functions as a cycle, the changes to new functions are commited to repo, then CI provider tests the builds and deploys the code.

* Travis CI, can be used as an open source alternative.

### Create Travis Yaml file
* Create a new folder called node













