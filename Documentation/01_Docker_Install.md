# **1. Docker Install**

You have to install:
- Docker
- Visual Studio Code

In Docker, pull the PAL robotics Image:
- In Docker images find: palroboticssl/tiago_tutorials
- Create a container:
```shell
docker run --name PAL_Tiago_mpuig -e DISPLAY=host.docker.internal:0.0 --mount src="C:\Users\puigm\Desktop\ROS_github\myPC_shared",dst=/home/myDocker_shared,type=bind -it palroboticssl/tiago_tutorials:melodic
```
You are ready to use the work space in the container

## **1.1. Use VS Code**
We will use VS Code to sync a copy of your github repository in your local PC:

- Open VS Code and choose "Clone repository"
- type the github link of the desired repository
- select the local destination folder
We will also use VS Code to work and sync the changes we have made in Docker Container:
- Install in your Visual Studio Code the extensions::
    - Docker
    - Dev Containers
    - Git Extension Pack (when you are connected with container)

Execute the container and Connect to it within VS Code:
- from left-side menu choose Docker
- right-click on the running container and select "Attach VS Code"
- select "Open Folder" and choose /


## **1.2. Use customized Dockerfile**

You can add some functionalities to the PAL Image
- git
- nautilus
- gnome terminal
