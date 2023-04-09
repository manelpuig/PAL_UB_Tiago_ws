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
Now pressing the up and down arrows will make the robot go forward and backward respectively. Left and right arrows will cause the robot to turn about itself in the corresponding directions.

## **2.2 Moving the base through velocity commands**
This tutorial shows how to move the base by sending velocity commands to the appropriate topic.

The topic where we have to send velocity commands is: /mobile_base_controller/cmd_vel

The message to send is of type geometry_msgs/Twist. A twist is composed of:
```shell
geometry_msgs/Vector3 linear
geometry_msgs/Vector3 angular
```
therefore, it is specified by a linear velocity vector and an angular velocity vector.

In the first console launch for example the following simulation:
```shell
roslaunch tiago_gazebo tiago_gazebo.launch public_sim:=true end_effector:=pal-gripper world:=empty
```
In the second console run the following instruction which will publish a constant linear velocity of 0.5 m/s along the X axis, i.e. the axis of the robot pointing forward:
```shell
rostopic pub /mobile_base_controller/cmd_vel geometry_msgs/Twist -r 3 -- '[0.5,0.0,0.0]' '[0.0, 0.0, 0.0]'
```
Note that the argument -r 3 specifies that the message will be published 3 times per second. This is necessary to maintain a constant speed as the mobile_base_controller only applies a given velocity for less than a second for safety reasons.

Test for exemple the message:
```shell
rostopic pub /mobile_base_controller/cmd_vel geometry_msgs/Twist -r 3 -- '[0.5,0.0,0.0]' '[0.0, 0.0, 0.3]'
```
## **2.3 Joint Trajectory Controller**
This tutorial shows how to use the joint_trajectory_controller to move the arm of TIAGo. The action can be used to give a trajectory to the arm expressed in several waypoints.

There are two mechanisms for sending trajectories to the controller, as written in joint_trajectory_controller (wiki.ros.org/joint_trajectory_controller). By means of the **action interface or the topic interface**. Both use the trajectory_msgs/JointTrajectory message to specify trajectories.

For the TIAGo the following interfaces are available: torso control, head control, arm control. 

In the first console launch for example the following simulation:
```shell
roslaunch tiago_gazebo tiago_gazebo.launch public_sim:=true end_effector:=pal-gripper world:=empty
```
Now we are going to run an example with two waypoints that will bring TIAGO to the following joint space configurations of the arm. First waypoint:
```python
arm_1_joint: 0.2
arm_2_joint: 0.0
arm_3_joint: -1.5
arm_4_joint: 1.94
arm_5_joint: -1.57
arm_6_joint: -0.5
arm_7_joint: 0.0
```
Second waypoint:
```python
arm_1_joint: 2.5
arm_2_joint: 0.2
arm_3_joint: -2.1
arm_4_joint: 1.9
arm_5_joint: 1.0
arm_6_joint: -0.5
arm_7_joint: 0.0
```
The node that will take care to execute the set of waypoints to reach such a kinematic configuration is run_traj_control included in tiago_trajectory_controller package and has to be called as follows
```shell
rosrun tiago_trajectory_controller run_traj_control
```
Note that the key parts of the code are:

- Create an arm controller action client
- Wait for arm controller action server to come up
- Generates a simple trajectory with two waypoints (goal)
- Sends the command to start the given trajectory
- Give time for trajectory execution

Note that:
- we specify the time to reach each waypoint
- for the first waypoint, for each joint in the trajectory, we use a velocity of 1.0 m/s. 
- As we want the robot to move smoothly, we set the velocity to 0.0 m/s just for the last waypoint.
- Joint trajectory messages allow to specify the time at which the trajectory should start executing by means of the header timestamp. In the node will start the given trajectory 1s from "now".

## **2.4 Moving individual joints**
This tutorial shows two different ways to move individual joints in position mode. One is from command line and the other is through a Qt GUI.

In the first console launch for example the following simulation:
```shell
roslaunch tiago_gazebo tiago_gazebo.launch public_sim:=true end_effector:=pal-hey5 world:=simple_office_with_people
```
In the second console run this
```shell
rosrun rqt_joint_trajectory_controller rqt_joint_trajectory_controller
```
We need to select controller_manager and then we will be able to select among the controllers of different groups of joints listed in the dropdown list at the right.

If we select the arm_controller, sliders will appear in order to control each joint of the arm individually.

**Moving joints from command line**

The play_motion package offers a move_joint node that can be used to move individual joints from command line.

For example, if we want to lower the head of TIAGo we may run:
```shell
rosrun play_motion move_joint head_2_joint -0.6 2.0
```
 the first argument is the name of the joint to move; the second is the target position and the third one is the time given to reach this position.

## **2.5 Head control**
This tutorial shows how to use the head action to move the head of TIAGo. The action can be used to make the robot head to look at any point expressed in any frame.

In the first console launch for example the following simulation:
```shell
roslaunch tiago_gazebo tiago_gazebo.launch end_effector:=pal-hey5 public_sim:=true world:=look_to_point
```
To look at speciffic point in the table, a head action client is given in tiago_tutorials/look_to_point/src/look_to_point.cpp.

In order to run the node write the following instruction in the second console:
```shell
rosrun look_to_point look_to_point
```
By clicking on any pixel of the image the robot will move its head in order to look at that point.

## **2.6 Playing pre-defined upper body motions**
The purpose of this tutorials is to show how to run pre-defined upper body motions with TIAGo. This is pretty easy by using the play_motion package, which enables executing simultaneous trajectories in multiple groups of joints.

In the first console launch for example the following simulation:
```shell
roslaunch tiago_gazebo tiago_gazebo.launch public_sim:=true end_effector:=pal-hey5 world:=empty
```
When the simulation is launched, the play_motion action server is automatically launched and the motions defined in tiago_motions_pal-hey5_schunk-ft.yaml have been loaded in the ros parameter server.

In order to see the names of the motions defined run the following instruction in the second console:
```shell
rosparam list | grep  "play_motion/motions" | grep "meta/name" | cut -d '/' -f 4
```
In the case of the wave motion the joints parameter specifies which joints intervene in the motion:
```shell
rosparam get /play_motion/motions/wave/joints
```
The joint trajectory, that composes this motion can be retrieved as follows:
```shell
rosparam get /play_motion/motions/wave/points
```
In order to run a motion the following graphical action client can be used for noetic:
```shell
rosrun actionlib_tools axclient.py /play_motion
```
