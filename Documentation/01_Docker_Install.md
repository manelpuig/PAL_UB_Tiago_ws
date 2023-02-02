# **1. Docker Install**
Docker Desktop is a one-click-install application for your Mac, Linux, or Windows environment that enables you to build and share containerized applications and microservices. (https://docs.docker.com/desktop/)

You have to install:
- Docker for windows: an easy to install application that enables you to manage your containers (https://docs.docker.com/desktop/install/windows-install/)
- Xming X server for Windows, an X11 display server that will allow you to visualize, in
particular the RViZ robot visualization and tools and the Gazebo simulation of the
robot. (https://sourceforge.net/projects/xming/)
- Visual Studio Code

In Docker, pull the PAL robotics Image:
- In Docker images find: palroboticssl/tiago_tutorials
```shell
docker pull palroboticssl/tiago_tutorials:melodic
```
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
    - Office Viewer extension

Execute the container and Connect to it within VS Code:
- from left-side menu choose Docker
- right-click on the running container and select "Attach VS Code"
- select "Open Folder" and choose /Tiago_Public_ws
- Open file (file>open file...): /root/.bashrc and verify to source our repository (last line).
```shell
source /tiago_public_ws/devel/setup.bash
```

## **1.2. Test the simulation**

To verify the repository is working:
- Open Xlaunch:
    - First choose “Multiple windows” and set to 0 the “Display number”
    - Then, set “Start no client” in the second screen
    - In the third screen, click “Clipboard” and Primary Selection and “Disable access control”, while leaving “Native opengl” unclicked
    - And just click “Finalize” in the last screen
- launch in a terminal:
```shell
cd /tiago_public_ws/
source ./devel/setup.bash
roslaunch tiago_gazebo tiago_gazebo.launch public_sim:=true end_effector:=pal-gripper
```
## **1.3. Repository syncronisation**

When working on VS Code, you need to:
- Select "source control" from left side menu
- select changes to sync
- Add a commit
- Push
