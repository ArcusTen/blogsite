---
title: Getting Started With Docker 
date: '2024-04-13'
tags: ['devops', 'guides', 'docker', 'containerization']
draft: false
summary: Understanding what is Docker, its significance, workings, how it can benefit you and getting started with docker.
---

## Table of Contents

1. **[What is Docker?](#what-is-docker)**
2. **[Why We Use Docker?](#why-we-use-docker)**
3. **[How Does it Work?](#how-does-it-work)**
   - **[DIfference b/w Virtualization & Containerization](#virtualization-vs-containerization)**
4. **[Docker Architecture](#docker-architecture)**
    - **[Docker Daemon](#docker-daemon-server)**
    - **[Docker Client](#docker-client-user-interface)**
    - **[Docker Registries](#docker-registries)**
    - **[Dockerfile](#dockerfile)**
    - **[Sample Dockerfile](#sample-dockerfile)**
    - **[Docker Images](#docker-images)**
    - **[Docker Container](#docker-container)**
5. **[Recap with Demo](#recap-with-demo)**
6. **[How to use docker to make a CTF challenge](#making-a-ctf-challenge-using-docker)**
7. **[Recap](#recap)**
8. **[Basic Commands](#basic-commands)**

## Introduction

Let's be honest we all have faced the problem when a program that works perfectly on your computer but when you give it to someone else, it doesn't work on theirs? This is where dockers can help. They solve the problem by making sure your program runs the same way on any computer.

## What is Docker?

Imagine a shipping container. It can hold all sorts of things, from furniture to clothes, and transport them safely across vast distances. Similarly, Docker creates standardized "containers" for your applications. These containers include everything an application needs to run: its code, libraries, and settings.

## Why We Use Docker?

- **Consistency:** Run your application exactly the same way on any machine, from your laptop to a massive server farm.

- **Portability:** Move your containers between different environments effortlessly.

- **Isolation:** Applications run in their own space, preventing conflicts with other programs.

- **Efficiency:** Containers share the underlying operating system, making them lightweight and fast.

## How does it work?

Docker works by creating containers, which are lightweight, portable computing environments. These containers run on top of a host operating system and share its kernel. This allows them to be isolated from each other and from the host system, while still being able to communicate and share resources.

- #### Virtualization vs Containerization:

  When you run an application on a virtual machine (VM), it needs its own operating system (OS) to work. This OS is managed by a software called a hypervisor, which creates and handles multiple virtual machines on a single physical machine. Each of these virtual machines has its own OS and operates independently from the host machine's OS. They're like separate compartments with allocated space.

  Now, in the software world, there's a different approach called containerization. Instead of having each application run on its own virtual machine with its OS, containerization bundles the application together with everything it needs to run into a neat package called a container. These containers can be placed on any machine without much fuss, as they don't rely on the specific setup of the host machine. This solves the problem of software dependencies.

  Virtual machines rely on hardware virtualization, meaning they simulate a full computer system, while containerization is more about operating system virtualization. In simple terms, virtualization creates virtual versions of entire systems or resources, while containerization is a lighter method that focuses on bundling applications with their dependencies.

  So, in essence, containerization is a lightweight way to achieve virtualization, making it easier to deploy and manage applications.

    ![Docker](/static/guides/DevOps/getting-started-with-docker/virtualization-vs-containerization.jpg)

    [Image Source](https://www.cloud4y.ru/en/blog/virtualization-vs-containerization/)
  
## Docker Architecture

Docker uses a client-server architecture. We (Docker client) talks to the Docker daemon, which (by the order of us) builds, run, distributes and manages our Docker containers.

- ### Docker daemon (server):
  Runs in the background on our machine, managing the building, running, and stopping of containers.

- ### Docker client (user interface):
  Allows us to interact with the daemon using commands or a graphical user interface (GUI).

- ### Docker registries: 
  Online repositories where we can find and share pre-built Docker images (like pre-packed shipping containers). A popular registry is `Docker Hub`.

- ### Dockerfile:
  A text file with instructions for building a Docker image. Think of creating a Docker image like following a recipe for your favorite dish. You gather all the necessary ingredients and steps to make that dish. Similarly, creating a Docker image involves putting together all the components needed for your application to run smoothly. This image can then be used to spawn containers in any setting. Plus, you can stash these images online on platforms like Docker Hub for easy access.Let's dive into how to craft a Dockerfile and use it to build an image.

  #### Understanding the Key Instructions:

  - **FROM:** This instruction is the foundation. It specifies the base image upon which we will build our customized image. Popular choices include Ubuntu, Python, and Node.js images.

  - **WORKDIR:** This sets the working directory for our application within the container. It determines where commands are executed by default.

  - **RUN:** This instruction executes commands within the container during the image building process. It's typically used to install dependencies, copy files, or configure your application.

  - **CMD:** This defines the default command that runs when we start a container based on this image. It specifies the action your application should take upon launch.

  - **EXPOSE:** This exposes a specific port on the container, making it accessible from the host machine or other containers. This is crucial for applications that rely on network communication.

  - **ENV:** This sets environment variables within the container. These variables can be used by our application to access configuration details or customize its behavior.
  
- ### Docker Images:

  A Docker Image serves as the blueprint for a Docker Container, akin to a snapshot of a Virtual Machine. When a container is transferred between Docker environments with identical operating systems, it operates seamlessly due to the inclusion of all necessary dependencies within the image. Docker Images facilitate the creation of Docker containers and remain immutable once generated, ensuring consistency. They can be stored locally or remotely, similar to repositories on hub.docker.com, akin to github for git.

  Images are constructed in layers, each layer representing an unalterable file comprising various files and directories. The final layer allows for data to be written. Layers are identified by unique IDs, calculated through SHA 256 hashing of their contents. Consequently, any alteration in the layer's contents results in a change in its corresponding hash, reflected in the IMAGE ID displayed by Docker commands (`docker images`). These hash values are commonly referred to as "tag" names.

  To see them run this command:

  ```bash
  docker images --no-trunc
  ```

  ![Docker](/static/guides/DevOps/getting-started-with-docker/docker-tags.png)

  - ### Crafting Your own Docker Image

    Start by crafting a file called `Dockerfile`. It will serve as a blueprint for your custom image.

    ![Docker](/static/guides/DevOps/getting-started-with-docker/dockerfile.png)

      - #### Sample Dockerfile:

      ```Dockerfile
      # Using Python runtime as the base image
      FROM python:3.9-slim

      # Setting up the working directory in the container
      WORKDIR /app

      # Copying the current directory contents into the container at /app
      COPY . /app

      # Installing any needed dependencies specified in requirements.txt
      RUN pip install -r requirements.txt

      # Making port 80 available to the world outside this container
      EXPOSE 80

      # Defining environment variable
      ENV NAME World

      # Run app.py when the container launches
      CMD ["python", "hello.py"]
      ```
      This Dockerfile will create a container based on the Python 3.9 image. It will then install the required libraries from a file called `requirements.txt` and copy your application code into the container. Finally, it will specify the command to run your application, which in this case will be `python hello.py`.
      
      Now, when you're ready to build your image, Docker automatically looks for that `Dockerfile`. You can initiate the build process using the command:

    ```bash
    $ docker build -t myimage:1.0 .
    ```
      ![docker-build-sc](/static/guides/DevOps/getting-started-with-docker/docker-build.png)

      You can check all docker images by running `docker images` command:

      ![docker-image-sc](/static/guides/DevOps/getting-started-with-docker/docker-image.png)

    Congratulations, you have successfully created your first docker image 🥳.

  ## Docker Container
  Container is a running instance of an image (Dockerfile). You can control these containers using Docker through either code or commands. These containers can connect to different networks, have storage attached to them, and even be turned into new versions of your software. But here's the catch: if you delete a container, any data inside it disappears. That's because when a container shuts down and restarts, it starts fresh, like wiping a whiteboard clean. This can be handy during development when you don't need to keep every little change. If you want to keep data safe and consistent, use volumes to store it separately.

## Making a CTF Challenge using Docker:

From here starts the real fun part. 

> *COMING SOON !!*


## Recap:

Let's recap by demonstrating a simple Docker workflow:

**Create a Dockerfile:** Specify your application's dependencies and configuration in a Dockerfile.

**Build a Docker Image:** Use the docker build command to build an image from the Dockerfile.

**Run a Docker Container:** Create and start a container based on the built image using docker run.

**Interact with the Container:** Access and interact with your application running inside the container.

**Cleanup:** Stop and remove the container when finished using docker stop and docker rm commands.
By following these steps, you can experience firsthand the power and versatility of Docker in modern software development and deployment.


## Basic Commands:

Some basic Docker commands to get you started:

**`docker build`:** Build an image from a Dockerfile.

**`docker run`:** Create and start a container based on an image.

**`docker pull`:** Pull an image or a repository from a registry.

**`docker push`:** Push an image or a repository to a registry.

**`docker images`:** List available images.

**`docker ps`:** List running containers.

**`docker exec`:** Run a command in a running container.