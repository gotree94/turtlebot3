# Simulation

> **Source**: [https://emanual.robotis.com/docs/en/platform/turtlebot3/simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/simulation)

---


# Simulation

**NOTE**

- The Simulation should be run from the **Remote PC** .
- Launching the Simulation for the first time on the Remote PC may take some time to setup the environment.


## Gazebo Simulation

The content in the e-Manual may be updated without prior notice and video content may be outdated.

This Gazebo Simulation uses the **ROS Gazebo package** , Gazebo version ROS 2 Humble has to be installed before running these instructions.


### Install Simulation Package

The **TurtleBot3 Simulation Package** requires `turtlebot3` and `turtlebot3_msgs` packages. Without these prerequisite packages, the Simulation cannot be launched.  Please follow the [PC Setup](https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/) instructions if you did not install required packages and dependent packages.

```
$ 
cd
 ~/turtlebot3_ws/src/

$ 
git clone 
-b
 humble https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git

$ 
cd
 ~/turtlebot3_ws 
&&
 colcon build 
--symlink-install


```


### Launch Simulation World

Three simulation environments are prepared for TurtleBot3. Please select one of these environments to launch Gazebo.

**Please make sure to completely terminate any other Simulation world before launching a new world.**

1. Empty World  ![](img/turtlebot3_empty_world.png)
2. TurtleBot3 World  ![](img/gazebo_world.png)
3. TurtleBot3 House  ![](img/turtlebot3_house.png)

**NOTE** : If TurtleBot3 House is launched for the first time, downloading the map may take more than a few minutes depending on network status.


### Operate TurtleBot3

In order to teleoperate the TurtleBot3 with a keyboard, launch the teleoperation node with the command below in a new terminal window.

```
$ 
ros2 run turtlebot3_teleop teleop_keyboard

```

**NOTE**

- The Simulation should be run from the **Remote PC** .
- Launching the Simulation for the first time on the Remote PC may take some time to setup the environment.


## Gazebo Simulation

This Gazebo Simulation uses the **ros-gz package** , Gazebo version ROS 2 Humble has to be installed before running these instructions.


### Install Simulation Package

The **TurtleBot3 Simulation Package** requires `turtlebot3` and `turtlebot3_msgs` packages. Without these prerequisite packages, the Simulation cannot be launched.  Please follow the [PC Setup](https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/) instructions if you did not install required packages and dependent packages.

```
$ 
cd
 ~/turtlebot3_ws/src/

$ 
git clone 
-b
 jazzy https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git

$ 
cd
 ~/turtlebot3_ws 
&&
 colcon build 
--symlink-install


```


### Launch Simulation World

Three simulation environments are prepared for TurtleBot3. Please select one of these environments to launch Gazebo.

**Please make sure to completely terminate any other Simulation world before launching a new world.**

1. Empty World $exportTURTLEBOT3_MODEL=burger$ros2 launch turtlebot3_gazebo empty_world.launch.py
2. TurtleBot3 World $exportTURTLEBOT3_MODEL=waffle$ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
3. TurtleBot3 House $exportTURTLEBOT3_MODEL=waffle_pi$ros2 launch turtlebot3_gazebo turtlebot3_house.launch.py

**NOTE** : If TurtleBot3 House is launched for the first time, downloading the map may take more than a few minutes depending on network status.


### Operate TurtleBot3

In order to teleoperate the TurtleBot3 with a keyboard, launch the teleoperation node with the command below in a new terminal window.

```
$ 
ros2 run turtlebot3_teleop teleop_keyboard

```

**NOTE**

- Simulation should be run on the **Remote PC** .
- Launching the Simulation for the first time on the Remote PC may take additional time to setup the environment.


## Gazebo Simulation

The contents of the e-Manual can be updated without prior notice and video content may be outdated.


### Install Simulation Package

The **TurtleBot3 Simulation Package** requires `turtlebot3` and `turtlebot3_msgs` packages. Without these prerequisite packages, the Simulation cannot be launched.  Please follow the [PC Setup](https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/) instructions if you did not install all required packages and dependent packages.  **[Remote PC]**

```
$ 
cd
 ~/catkin_ws/src/

$ 
git clone 
-b
 noetic https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git

$ 
cd
 ~/catkin_ws 
&&
 catkin_make

```


### Launch Simulation World

Three simulation environments are prepared for TurtleBot3. Please select one of these environments to launch Gazebo.

**Please make sure to completely terminate any other Simulation world before launching a new world.**

1. Empty World  ![](img/turtlebot3_empty_world.png)  **[Remote PC]** $exportTURTLEBOT3_MODEL=burger$roslaunch turtlebot3_gazebo turtlebot3_empty_world.launch
2. TurtleBot3 World  ![](img/turtlebot3_world_bugger.png)  **[Remote PC]** $exportTURTLEBOT3_MODEL=waffle$roslaunch turtlebot3_gazebo turtlebot3_world.launch
3. TurtleBot3 House  ![](img/turtlebot3_house.png)  **[Remote PC]** $exportTURTLEBOT3_MODEL=waffle_pi$roslaunch turtlebot3_gazebo turtlebot3_house.launch

**NOTE** : When TurtleBot3 House is launched for the first time, downloading the map may take more than a few minutes depending on network status.


### Operate TurtleBot3

In order to teleoperate the TurtleBot3 with the keyboard, launch the teleoperation node in a new terminal window.  **[Remote PC]**

```
$ 
roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch

```
