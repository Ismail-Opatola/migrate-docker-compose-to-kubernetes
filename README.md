sourec: 
  https://www.digitalocean.com/community/tutorials/how-to-migrate-a-docker-compose-workflow-to-kubernetes

  file:///C:/Users/ismail/Downloads/books/2020_microservice/readme/kubernetes-for-full-stack-developers.pdf

To run your services on a distributed platform like Kubernetes, you will need to translate your Docker Compose service definitions to Kubernetes objects. `Kompose` is a conversion tool that helps developers move their Docker Compose workflows to container clusters like Kubernetes.

In this tutorial, you will translate your Node.js application’s Docker Compose services into Kubernetes objects using kompose. You will use the object definitions that kompose provides as a starting point and make adjustments to ensure that your setup will use Secrets, Services, and PersistentVolumeClaims in the way that Kubernetes expects. By the end of the tutorial, you will have a single-instance Node.js application with a MongoDB database running on a Kubernetes cluster.

step 1 - Install Kompose
https://github.com/kubernetes/kompose/releases

curl -L https://github.com/kubernetes/kompose/releases/download/v1.21.0/kompose-linux-amd64 -o kompose

Make the binary executable:
$ chmod +x kompose

Move it to your PATH:
$ sudo mv ./kompose /usr/local/bin/kompose

To verify that it has been installed properly, you can do a version check:
$ kompose version

Step 2 — Cloning and Packaging the Application

To use our application with Kubernetes, we will need to clone the project
code and package the application so that the kubelet service can pull the
image.

Our first step will be to clone the node-mongo-docker-dev repository

This repository includes the code from the setup described in Containerizing a Node.js Application for Development With Docker Compose, which uses a demo Node.js application to demonstrate how to set up a development environment using Docker Compose.

$ git clone https://github.com/do-community/nodemongo-docker-dev.git node_project

