#**1. Docker Install**
You have to install:
- Docker
- Visual Studio Code

In Docker, pull the PAL robotics Image:
- In Docker images find: palroboticssl/tiago_base_family_tutorials
- Create a container:
```shell
docker run --name PAL_Tiago_mpuig -e DISPLAY=host.docker.internal:0.0 -it palroboticssl/tiago_base_family_tutorials:melodic
```
You are ready to use the work space in the container

##**1.1. Use VS Code**
Use your Visual Studio Code with the extensions:
- Docker
- Dev Containers

Execute the container and Connect to it within VS Code

##**1.2. Use customized Dockerfile**
You can add some functionalities to the PAL Image
- git
- nautilus
- gnome terminal