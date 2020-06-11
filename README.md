sourec: 

    https://www.digitalocean.com/community/tutorials/how-to-migrate-a-docker-compose-workflow-to-kubernetes

    file:///C:/Users/ismail/Downloads/books/2020_microservice/readme/kubernetes-for-full-stack-developers.pdf

To run your services on a distributed platform like Kubernetes, you will need to translate your Docker Compose service definitions to Kubernetes objects. `Kompose` is a conversion tool that helps developers move their Docker Compose workflows to container clusters like Kubernetes.

In this tutorial, you will translate your Node.js application’s Docker Compose services into Kubernetes objects using kompose. You will use the object definitions that kompose provides as a starting point and make adjustments to ensure that your setup will use Secrets, Services, and PersistentVolumeClaims in the way that Kubernetes expects. By the end of the tutorial, you will have a single-instance Node.js application with a MongoDB database running on a Kubernetes cluster.

## step 1 - Install Kompose
    
    https://github.com/kubernetes/kompose/releases

    curl -L https://github.com/kubernetes/kompose/releases/download/v1.21.0/kompose-linux-amd64 -o kompose

Make the binary executable:
    
    $ chmod +x kompose

Move it to your PATH:
    
    $ sudo mv ./kompose /usr/local/bin/kompose

To verify that it has been installed properly, you can do a version check:
$ kompose version

## Step 2 — Cloning and Packaging the Application

To use our application with Kubernetes, we will need to clone the project
code and package the application so that the kubelet service can pull the
image.

Our first step will be to clone the node-mongo-docker-dev repository

This repository includes the code from the setup described in Containerizing a Node.js Application for Development With Docker Compose, which uses a demo Node.js application to demonstrate how to set up a development environment using Docker Compose.
    
    $ git clone https://github.com/do-community/node-mongo-docker-dev.git node_project 

The project directory includes a Dockerfile with instructions for building the application image. Let’s build the image now so that you can push it to your Docker Hub account and use it in your Kubernetes setup.

Using the docker build command, build the image with the -t flag, which allows you to tag it with a memorable name. In this case, tag the image with your Docker Hub username and name it node-kubernetes or a name of your own choosing:

    $ docker build -t your_dockerhub_username/node-kubernetes .

The ` . ` in the command specifies that the build context is the current directory.
It will take a minute or two to build the image. Once it is complete, check your images:

  `$ docker images`

Next, log in to the Docker Hub account you created in the prerequisites:

  `$ docker login -u your_dockerhub_username`

When prompted, enter your Docker Hub account password. Logging in this way will create a `~/.docker/config.json` file in your user’s home directory with your Docker Hub credentials.

Push the application image to Docker Hub with the docker push command. Remember to replace `your_dockerhub_username` with your own Docker Hub username:

  `$ docker push your_dockerhub_username/node-kubernetes`

You now have an application image that you can pull to run your application with Kubernetes. The next step will be to translate your application service definitions to Kubernetes objects.

## Step 3 — Translating Compose Services to Kubernetes Objects with kompose


