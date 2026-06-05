# TurtleBot3

> **Source**: [https://emanual.robotis.com/docs/en/platform/turtlebot3/fakenode_simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/fakenode_simulation)

---
# TOC

1. [Humble)(#humble)
2. [Jazzy)(#jazzy)
3. [Noetic)(#noetic)

---
[TOC)(#toc)
# Humble

## 6.4 Fake Node Simulation

https://youtu.be/iHXZSLBJHMg?si=miugXqHhBTSBol_r

> The contents in e-Manual are subject to change without prior notice. Video content may differ from the content in the e-Manual.

To use a virtual TurtleBot3, execute `turtlebot3_fake_node.launch.py` in a `turtlebot3_fake_node` package that is simple simulation node.
Follow the instructions to bring TurtleBot3 into the virtual world using Fake Node.

1. Execute `turtlebot3_fake_node.launch.py` file.  
Specify your TurtleBot3 model ( `burger` , `waffle` , `waffle_pi` ) using the `TURTLEBOT3_MODEL` parameter. 
```
$ export TURTLEBOT3_MODEL=burger
$ ros2 launch turtlebot3_fake_node turtlebot3_fake_node.launch.py
```

2. You can control the virtual TurtleBot3 by using [Teleoperation](https://emanual.robotis.com/docs/en/platform/turtlebot3/basic_operation/#teleoperation) 
```
$ export TURTLEBOT3_MODEL=burger
$ ros2 run turtlebot3_teleop teleop_keyboard
```


---
[TOC)(#toc)
# Jazzy

> **NOTE** : This feature is available for Humble and Noetic.


---
[TOC)(#toc)
# Noetic

## 6.4 Fake Node Simulation

https://youtu.be/iHXZSLBJHMg?si=miugXqHhBTSBol_r

> The contents of the e-Manual are subject to change without prior notice. Video content may differ from the content in the e-Manual.

* To use `turtlebot3_fake_node` , you need the `turtlebot3_simulation` metapackage. Install the package as shown in the following command.

> **NOTE** : The `turtlebot3_simulation` metapackage requires `turtlebot3` metapackage and `turtlebot3_msgs` package as a prerequisite. If you didn’t install it in the [Install Dependent ROS Packages of PC Setup](https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/#install-dependent-ros-1-packages-1) section, install it now.

**[Remote PC]** <br>
```
$ cd ~/catkin_ws/src/
$ git clone -b noetic https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
$ cd ~/catkin_ws && catkin_make
```

To launch the virtual robot, execute the `turtlebot3_fake.launch` file in the `turtlebot3_fake` package as shown below. The `turtlebot3_fake` is a very simple simulation node that can be run without having an actual robot. You can even control the virtual TurtleBot3 in RViz with a teleoperation node.

**[Remote PC]** <br>
```
$ export TURTLEBOT3_MODEL=burger
$ roslaunch turtlebot3_fake turtlebot3_fake.launch
```

**[Remote PC]** <br>
```
$ export TURTLEBOT3_MODEL=burger
$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```
