# **3. Autonomous navigation**
We will studi:
- How to create a map with gmapping
- How to perform localization and path planning

## **3.1. Create a map with gmapping**
This tutorial shows how to create a laser map of the environment with the public simulation of TIAGo using gmapping (http://wiki.ros.org/gmapping). The map is required to use afterwards AMCL based localization to match laser scans with the map to provide reliable estimates of the robot pose in the map.

TIAGo could be launched with its omnidirectional base with the option base_type set to omni_base, by default this parameter is set to pmb2
In the first console launch the following simulation:
```shell
roslaunch tiago_2dnav_gazebo tiago_mapping.launch public_sim:=true base_type:=omni_base
```
In the second console launch the keyboard teleoperation node
```shell
rosrun key_teleop key_teleop.py
```
save the map as follows
```shell
rosservice call /pal_map_manager/save_map "directory: 'test_map1'"
```
The service call will save the map in the following folder:
```shell
/root/.pal/tiago_maps/configurations/
```
Now TIAGo is ready to do autonomous localization and path planning using the map. 

## **3.2. Localization and path planning**
This tutorial shows how to make TIAGo navigate autonomously provided a map build up of laser scans and taking into account the laser and the RGBD camera in order to avoid obstacles.

In the first console launch the simulation of TIAGo with all the required nodes for running the autonomous navigation:
```shell
roslaunch tiago_2dnav_gazebo tiago_navigation.launch public_sim:=true lost:=true base_type:=omni_base map:=$HOME/.pal/tiago_maps/configurations/test_map1
```
Take into account:
- base_type parameter is set by default to pmb2
- the map launched is $(env HOME)/.pal/tiago_maps/configurations/$(arg world) where world is by default small_office

**Localization**

the robot wakes up in a position of the world which does not correspond to the map origin, which is the pose where the localization system assumes the robot is and where particles are spread representing possible poses of the robot.

In order to localize the robot run the following instruction in the second console:
```shell
rosservice call /global_localization "{}"
```
This causes the amcl probabilistic localization system to spread particles all over the map 

Now a good way to help the particle filter to converge to the right pose estimate is to move the robot. You may use the left and right arrows of the key_teleop in order to do so.
```shell
rosrun key_teleop key_teleop.py
```
Now, it is preferable to clear the costmaps as it contains erroneous data due to the bad localization of the robot:
```shell
rosservice call /move_base/clear_costmaps "{}"
```
The costmap now only takes into account obstacles in the static map and the system is ready to perform navigation.

**Autonomous navigation with rviz and TIAGo**

First of all make sure to kill the key_teleop node. Otherwise the robot will not move autonomously as key_teleop takes higher priority.

In order to send the robot to another location of the map the button 2D Nav Goal can be pressed in rviz.
