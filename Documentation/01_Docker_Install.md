# **1. Docker Install**

You have to install:
- Docker
- Visual Studio Code

In Docker, pull the PAL robotics Image:
- In Docker images find: palroboticssl/tiago_tutorials
- Create a container:
```shell
docker run --name PAL_Tiago_mpuig -e DISPLAY=host.docker.internal:0.0 -it palroboticssl/tiago_tutorials:melodic
```
You are ready to use the work space in the container

## **1.1. Use VS Code**

Use your Visual Studio Code with the extensions:
- Docker
- Dev Containers

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
- Open Xlaunch and select terminal:0
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
