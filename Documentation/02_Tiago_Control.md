# **2. Tiago Control**
We will test the different Tiago control functionalities.

The Tutorial is located in: http://wiki.ros.org/Robots/TIAGo/Tutorials

## **2.1 Teleoperating the mobile base with the keyboard**
How to move the differential drive base of TIAGo using the key_teleop package

This tutorial shows a way to teleoperate the mobile base of TIAGo using the keyboard. This relies on the ROS package key_teleop which is compatible with most of ROS enabled robots.

In the first console launch for example the following simulation
```shell
roslaunch tiago_gazebo tiago_gazebo.launch public_sim:=true robot:=titanium world:=simple_office_with_people
```
In the second console run key_teleop as follows
```shell
rosrun key_teleop key_teleop.py
```
