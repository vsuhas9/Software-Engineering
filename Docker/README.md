# Docker for Developers
Contains the resources and notes from various Courses

## Introduction

* its famous for its ability to easily setup the programming environment on local system.


* A Container is where the sepcific resource is located. 
* Image is like a recipe where all the application details are present
* Volume contains the data of these resources
* These three are ideally seperated.

## Installation
* docker.com

## Docker Image and commnads

* Open the code files and add them in the folder
* create two files ".dockerignore" and Dockerfile
* In the dockerignore, add the things that need to be ignored.
* Add the commands and relevant details in docker file.

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
 




