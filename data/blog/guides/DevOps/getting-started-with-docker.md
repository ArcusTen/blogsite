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
   - **[Difference b/w Virtualization & Containerization](#virtualization-vs-containerization)**
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
7. **[Basic Commands](#basic-commands)**

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

- ### Docker Daemon (server):
  Runs in the background on our machine, managing the building, running, and stopping of containers.

- ### Docker Client (user interface):
  Allows us to interact with the daemon using commands or a graphical user interface (GUI).

- ### Docker Registries: 
  Online repositories where we can find and share pre-built Docker images (like pre-packed shipping containers). It is a central repository for storing and distributing docker images. A popular registry is `Docker Hub`.

- ### Docker Repository:
  A Docker repository is a collection of Docker images, allowing users to store, distribute, and manage containerized applications and their dependencies efficiently. It serves as a centralized location for sharing and accessing container images.

- ### Dockerfile:
  A text file with instructions for building a Docker image. Think of creating a Docker image like following a recipe for your favorite dish. You gather all the necessary ingredients and steps to make that dish. Similarly, creating a Docker image involves putting together all the components needed for your application to run smoothly. This image can then be used to spawn containers in any setting. Plus, you can stash these images online on platforms like Docker Hub for easy access.Let's dive into how to craft a Dockerfile and use it to build an image.

  #### Understanding the Key Instructions:

  | Instructions      | Explanation |
  | :---------------- | :------: | 
  | FROM    |  This instruction is the foundation. It specifies the base image upon which we will build our customized image Popular choices include Ubuntu, Python, and Node.js images.   | 
  | WORKDIR |  This sets the working directory for our application within the container. It determines where commands are executed by default.   | 
  | RUN     |  This instruction executes commands within the container during the image building process. It's typically used to install dependencies, copy files, or configure your application.   | 
  | CMD     |  This defines the default command that runs when we start a container based on this image. It specifies the action your application should take upon launch.   | 
  | EXPOSE  |  This exposes a specific port on the container, making it accessible from the host machine or other containers. This is crucial for applications that rely on network communication.   | 
  | ENV     |  This sets environment variables within the container. These variables can be used by our application to access configuration details or customize its behavior.   | 
  | COPY    |  Copies local files and directories to the image.   |
  | ADD     |  It is a more feature-rich version of the COPY instruction. It also allows copying from the URL that is the source and tar file auto-extraction into the image. However, usage of COPY command is recommended over ADD. If you want to download remote files, use curl or get using RUN.   |
  | VOLUME  |  It is used to create or mount the volume to the Docker container.   |
  | USER    |  Sets the user name and UID when running the container. You can use this instruction to set a non-root user of the container.   |
  | LABEL   |  It is used to specify metadata information of Docker image.   |
  | ARG     |  Is used to set build-time variables with key and value. the ARG variables will not be available when the container is running. If you wantto persist a variable on a running container, use ENV.   |
  | ENTRYPOINT |  Specifies the commands that will execute when the Docker container starts. If you don't specify any ENTRYPOINT, it defaults to /bin/sh - c. You can also override ENTRYPOINT using the -- entrypoint flag using CLI.   |
  
- ### Docker Images:

  A Docker Image serves as the blueprint for a Docker Container, akin to a snapshot of a Virtual Machine. When a container is transferred between Docker environments with identical operating systems, it operates seamlessly due to the inclusion of all necessary dependencies within the image. Docker Images facilitate the creation of Docker containers and remain immutable once generated, ensuring consistency. They can be stored locally or remotely, similar to repositories on hub.docker.com, akin to github for git.

  Images are constructed in **layers**, each layer representing an unalterable file comprising various files and directories. The final layer allows for data to be written. Layers are identified by unique IDs, calculated through SHA 256 hashing of their contents. Consequently, any alteration in the layer's contents results in a change in its corresponding hash, reflected in the IMAGE ID displayed by Docker commands (`docker images`). These hash values are commonly referred to as "tag" names.

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

## Real World Example:

Let's say I have a terminal-based question/answer game that I built using node and I want to create a dockerfile of it. For your better understanding, this is the [code](https://github.com/ArcusTen/Projects/tree/main/Games/FlashFlow):

```javascript:index.js
#!/usr/bin/env node

import chalk from 'chalk';
import inquirer from 'inquirer';
import gradient from 'gradient-string';
import figlet from 'figlet';
import { createSpinner } from 'nanospinner';
import chalkAnimation from 'chalk-animation';

let playerName;

const sleep = (ms = 2700) => new Promise((r) => setTimeout(r, ms));

async function welcome()
{
    const rainbowTitle = chalkAnimation.rainbow('Who wants to Be the next theFlash2k ?? \n');

    await sleep();
    rainbowTitle.stop();

    console.log(`
    ${chalk.bgCyan('HOW TO PLAY THIS STUPID GAME YOU MAY ASK:')}
    I am a processor on your computer.
    If you get any question wrong I will leave you just like your ${chalk.bgMagentaBright('EX')}
    So be Careful ...
    `)
}

async function askName()
{
    const answers = await inquirer.prompt({
        name: 'player_name',
        type: 'input',
        message: 'Sorry I forgot to ask, what is your name? \n',
        default()
        {
            return 'Player';
        },
    })
    playerName = answers.player_name;
}

async function question_1()
{
    const answers = await inquirer.prompt({
        name: 'question_1',
        type: 'list',
        message: 'What is theFlash2k\'s favourite CTF Domain?? \n',
        choices: [
            'Misc',
            'Pwn',
            'Reversing',
            'Cryptography',
        ]
    })
    return handleAnswer(answers.question_1 == 'Pwn');
}

async function handleAnswer(isCorrect)
{
    const spinner = createSpinner(`Hmmm .... Lemme Check ...`).start();
    await sleep();

    if (isCorrect)
    {
        spinner.success({ text: `Thats Correct ${playerName}. I guess you know something about him 🧐`});
    }
    else
    {
        spinner.error({ text: `💀 Game Over, you lost ${playerName} 💀`});
        process.exit(1);
    }
}

async function question_2()
{
    const answers = await inquirer.prompt({
        name: 'question_2',
        type: 'list',
        message: 'What is theFlash2k\'s favourite Food?? \n',
        choices: [
            'Biryani',
            'Qorma',
            'Fast-Food',
            'Nehari',
        ]
    })
    return handleAnswer(answers.question_2 == 'Fast-Food');
}

function win_screen()
{
    console.clear();
    const msg = `Congratulations  ${playerName}  !!! \n You can become next theFlash2k`;

    figlet(msg, (err, data) => {
        console.log(gradient.pastel.multiline(data));
    });
}

await welcome()
await askName();
await question_1();
await question_2();

await win_screen();
```

This will be the Dockerfile for it:

```dockerfile:Dockerfile
FROM node:21

# Set the working directory inside the container to /usr/src/app
WORKDIR /usr/src/app

# Copying package.json and package-lock.json from the host machine to the working directory in the container
COPY package*.json ./

# Installing dependencies listed in `package.json` using npm
RUN npm install

# Copying all files from the host machine to the working directory in the container
COPY . .

# Define the default command to run when the container starts
CMD [ "node", "./index.js" ]
```

Now run build command to make image from this Dockerfile, I will give this image name "flashflow":

```bash
docker build -t flashflow .
```
![docker-build2-sc](/static/guides/DevOps/getting-started-with-docker/docker-build2.png)

As the Docker image has been created successfully, it's time to spin up a container from it. Remember, it's a terminal-based game, which means we have to interact with it. So, I will create a container of that image with the `-it` flag enabled, specifying interactive mode:

![docker-container-sc](/static/guides/DevOps/getting-started-with-docker/docker-container.png)


Congratulations! You now know how to containerize an app.

After using it, if you don't want to keep this container, you can use the `docker rm` command to remove it. But for that, we need the hash of the container that we want to remove. You can find it using the `docker ps` command (for running containers) or the `docker ps -a` command (for stopped containers):

![docker-rm-sc](/static/guides/DevOps/getting-started-with-docker/docker-rm.png)

## Making a CTF Challenge using Docker:

From here starts the real fun part. Let's say you want to make a CTF challenge based on Linux privilege escalation, but you don't know how to set up a server or don't have enough resources for it. Well, don't worry. Docker has got you covered.

In this part of the Docker guide, we will create a simple privilege escalation challenge based on `CVE-2019-14287`. For more information **[Click Here](https://tryhackme.com/r/room/sudovulnsbypass)**

To make such a container, we need an older version of sudo (a vulnerable version) more explicitly, `sudo-1.8.27` and the Dockerfile will look something like this:

```dockerfile
FROM ubuntu:latest

RUN mkdir /var/run/sshd

# Copying sudo 1.8.27 source code into the container
COPY sudo-1.8.27.tar.gz /tmp/sudo-1.8.27.tar.gz

# Installing necessary packages
RUN apt-get update && \
    apt-get install -y build-essential openssh-server nano

# Installing vulnerable sudo
RUN tar -xzvf /tmp/sudo-1.8.27.tar.gz -C /tmp && \
    cd /tmp/sudo-1.8.27 && \
    ./configure && \
    make && \
    make install && \
    rm -rf /tmp/sudo-1.8.27 /tmp/sudo-1.8.27.tar.gz

# Creating user
RUN useradd -m ctf-player
RUN echo "ctf-player:s3cur3pass4me" | chpasswd
RUN usermod -aG sudo ctf-player

# Setting sudo permissions for `/bin/less`
RUN echo "ctf-player ALL=(ALL,!root) /bin/less" >> /etc/sudoers

# Nulling bash history
RUN ln -sf /dev/null /home/ctf-player/.bash_history

# Creating flag.txt in root's directory
RUN echo "AUCSS{W417_3v3n_5ud0_15nt_53cur3??_7863251}" > /root/flag.txt && \
    chown root:root /root/flag.txt && \
    chmod 400 /root/flag.txt

# Setting up SSH
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
EXPOSE 22

RUN chsh -s /bin/bash ctf-player

CMD ["/usr/sbin/sshd", "-D"]
```

This Dockerfile sets up an Ubuntu container with SSH access and installs a vulnerable version of sudo (1.8.27). It creates a user named "ctf-player" with sudo privileges, sets permissions for sudo access to /bin/less, nulls bash history for the user, and creates a flag file in the root directory. Additionally, it modifies SSH configurations to allow root login and exposes port 22 for SSH connections. Finally, it sets the default shell for the "ctf-player" user to bash and starts the SSH daemon as the container's main process.

## Basic Commands:

Some basic Docker commands to get you started:

**`docker build`:** Build an image from a Dockerfile.

**`docker run`:** Create and start a container based on an image.

**`docker pull`:** Pull an image or a repository from a registry.

**`docker push`:** Push an image or a repository to a registry.

**`docker images`:** List available images.

**`docker ps`:** List running containers.

**`docker exec`:** Run a command in a running container.