> **Source**: [https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation](https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation)

---
# TOC

1. [Humble](#humble)
2. [Jazzy](#jazzy)
3. [Noetic](#noetic)

---
[TOC](#toc)
# Humble


# Navigation

**WARNING** : While following these instructions, your TurtleBot3 may move and rotate unexpectedly. Please place the robot in safe location on the ground.

**NOTE**

- Navigation should be run on the Remote PC.
- Make sure to launch [Bringup](https://emanual.robotis.com/docs/en/platform/turtlebot3/bringup/) on your TurtleBot3 before executing the following operations.
- Navigation uses maps created by [SLAM](https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/) . Please prepare a map before running Navigation.

**Navigation** is used move the robot from one location to a specified destination in a given environment. For this purpose, a map that contains geometry information describing furniture, objects, and walls of the given environment is required. As described in the previous SLAM section, a map was created with the distance information obtained by the sensor and the pose information of the robot itself.


## Run Navigation Nodes

1. If `Bringup` is not running on the TurtleBot3 SBC, launch Bringup. Open a new terminal from Remote PC withCtrl+Alt+Tand connect to Raspberry Pi with its IP address.
The default password isubuntu.[Remote PC]$ssh ubuntu@{IP_ADDRESS_OF_RASPBERRY_PI}$exportTURTLEBOT3_MODEL=${TB3_MODEL}$ros2 launch turtlebot3_bringup robot.launch.py
2. Open a new terminal from Remote PC with `Ctrl` + `Alt` + `T` and launch the Navigation node. ROS 2 uses [Navigation2](https://navigation.ros.org/) .
  Specify your TurtleBot3 model ( `burger` , `waffle` , `waffle_pi` ) using the `TURTLEBOT3_MODEL` parameter.  **[Remote PC]** $exportTURTLEBOT3_MODEL=burger$ros2 launch turtlebot3_navigation2 navigation2.launch.py map:=$HOME/map.yaml

From the Jazzy version, the `cmd_vel` topic uses the `TwistStamped` type.  If you want to use the Twist type, set the enable_stamped_cmd_vel parameter to false in both the turtlebot3_bringup and turtlebot3_navigation2 packages.

1. If `Bringup` is not running on the TurtleBot3 SBC, launch Bringup. Open a new terminal from Remote PC withCtrl+Alt+Tand connect to Raspberry Pi with its IP address.
The default password isubuntu.[Remote PC]$ssh ubuntu@{IP_ADDRESS_OF_RASPBERRY_PI}$exportTURTLEBOT3_MODEL=${TB3_MODEL}$ros2 launch turtlebot3_bringup robot.launch.py
2. Open a new terminal from Remote PC with `Ctrl` + `Alt` + `T` and launch the Navigation node. ROS 2 uses [Navigation2](https://navigation.ros.org/) .
  Specify your TurtleBot3 model ( `burger` , `waffle` , `waffle_pi` ) using the `TURTLEBOT3_MODEL` parameter.  **[Remote PC]** $exportTURTLEBOT3_MODEL=burger$ros2 launch turtlebot3_navigation2 navigation2.launch.py map:=$HOME/map.yaml

1. Run `roscore` on the remote PC. **Skip this step if roscore is already running** .  **[Remote PC]** $roscore
2. Run `Bringup` on the TurtleBot3 SBC. **Skip this step if bringup is already running** . Open a new terminal from Remote PC withCtrl+Alt+Tand connect to the Raspberry Pi with its IP address.
The default password isturtlebot, and you will be required to specify your TurtleBot3 model (burger,waffle,waffle_pi) using theTURTLEBOT3_MODELparameter.[Remote PC]$ssh ubuntu@{IP_ADDRESS_OF_RASPBERRY_PI}$exportTURTLEBOT3_MODEL=${TB3_MODEL}$roslaunch turtlebot3_bringup turtlebot3_robot.launch
3. Launch Navigation.  **[Remote PC]** $roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:=$HOME/map.yaml


## Estimate Initial Pose

**Initial Pose Estimation** must be performed before running Navigation as this process initializes the AMCL parameters that are critical for Navigation. The TurtleBot3 has to be correctly located on the map with LDS sensor data that overlaps the displayed map.

1. Click the `2D Pose Estimate` button in the RViz2 menu.
2. Click on the map where the actual robot is located and drag the large green arrow toward the direction where the robot is facing.
3. Repeat step 1 and 2 until the LDS sensor data is overlayed on the saved map. ![](img/tb3_navigation2_rviz_01.png)
4. Launch the keyboard teleoperation node to precisely locate the robot on the map.  **[Remote PC]** $ros2 run turtlebot3_teleop teleop_keyboard
5. Move the robot back and forth a bit to collect the surrounding environment information and narrow down the estimated location of the TurtleBot3 on the map (displayed with tiny green arrows).  ![](img/tb3_amcl_particle_01.png)
6. Terminate the keyboard teleoperation node with `Ctrl` + `C` to prevent different **cmd_vel** values from being published from multiple nodes during Navigation.

**Initial Pose Estimation** must be performed before running Navigation as this process initializes the AMCL parameters that are critical for Navigation. The TurtleBot3 has to be correctly located on the map with LDS sensor data that overlaps the displayed map.

1. Click the `2D Pose Estimate` button in the RViz2 menu.
2. Click on the map where the actual robot is located and drag the large green arrow toward the direction where the robot is facing.
3. Repeat step 1 and 2 until the LDS sensor data is overlayed on the saved map. ![](img/tb3_navigation2_rviz_01.png)
4. Launch the keyboard teleoperation node to precisely locate the robot on the map.  **[Remote PC]** $ros2 run turtlebot3_teleop teleop_keyboard
5. Move the robot back and forth a bit to collect the surrounding environment information and narrow down the estimated location of the TurtleBot3 on the map (displayed with tiny green arrows).  ![](img/tb3_amcl_particle_01.png)
6. Terminate the keyboard teleoperation node with `Ctrl` + `C` to prevent different **cmd_vel** values from being published from multiple nodes during Navigation.

**Initial Pose Estimation** must be performed before running Navigation as this process initializes the AMCL parameters that are critical for successful Navigation. The TurtleBot3 has to be correctly located on the map with the LDS sensor data overlapping the displayed map.

1. Click the `2D Pose Estimate` button in the RViz menu.  ![](img/2d_pose_button.png)
2. Click on the map where the actual robot is located and drag the large green arrow toward the direction where the robot is facing.
3. Repeat step 1 and 2 until the LDS sensor data is overlayed on the saved map.
4. Launch keyboard teleoperation node to precisely locate the robot on the map.  **[Remote PC]** $roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
5. Move the robot back and forth a bit to collect the surrounding environment information and narrow down the estimated location of the TurtleBot3 on the map (displayed with tiny green arrows).  ![](img/tb3_amcl_particle_01.png)
6. Terminate the keyboard teleoperation node with `Ctrl` + `C` in order to prevent different **cmd_vel** values being published from multiple nodes during Navigation.


## Set Navigation Goal

1. Click the `Navigation2 Goal` button in the RViz2 menu.
2. Click on the map to set the destination of the robot and drag the green arrow toward the direction where the robot will be facing. This green arrow is a marker to can specify the destination of the robot.The root of the arrow is thex,ycoordinate of the destination, and the angleθis determined by the orientation of the arrow.As soon as x, y, θ are set, the TurtleBot3 will start moving to the destination immediately.

1. Click the `Navigation2 Goal` button in the RViz2 menu.
2. Click on the map to set the destination of the robot and drag the green arrow toward the direction where the robot will be facing. This green arrow is a marker to can specify the destination of the robot.The root of the arrow is thex,ycoordinate of the destination, and the angleθis determined by the orientation of the arrow.As soon as x, y, θ are set, the TurtleBot3 will start moving to the destination immediately.

1. Click the `2D Nav Goal` button in the RViz menu.  ![](img/2d_nav_goal_button.png)
2. Click on the map to set the destination of the robot and drag the green arrow toward the direction where the robot will be facing. This green arrow is a marker to specify the destination of the robot.The root of the arrow is thex,ycoordinate of the destination, and the angleθis determined by the orientation of the arrow.As soon as x, y, θ are set, the TurtleBot3 will start moving to the destination immediately.


## Tuning Guide

The Navigation2 stack has many parameters to change performances for different robots. Although it’s similar to ROS1 Navigation, please refer to the [Configuration Guide of Navigation2](https://navigation.ros.org/configuration/index.html) or [ROS Navigation Tuning Guide by Kaiyu Zheng](http://kaiyuzheng.me/documents/navguide.pdf) for more details.


### Costmap Parameters


#### inflation_layer.inflation_radius

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- This parameter defines an inflation area of inaccesability around detected obstacles. Generated paths will be planned not to cross this area. It is safe to set this value to be slightly bigger than robot radius. For more information, please refer to [costmap_2d wiki](http://wiki.ros.org/costmap_2d#Inflation) .

![](img/tuning_inflation_radius.png)


#### inflation_layer.cost_scaling_factor

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- This is an inverse proportional factor that is multiplied by the value of the costmap. If this parameter is increased, the value of the costmap is decreased.

![](img/tuning_cost_scaling_factor.png)

The optimal path for the robot to pass through obstacles is to take a median path between them. Setting a smaller value for this parameter will create a farther path from the obstacles.


### dwb_controller


#### max_vel_x

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- This factor sets the maximum value for translational velocity.


#### min_vel_x

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- This factor sets the minimum value for translational velocity. If set this negative, the robot can move backwards.


#### max_vel_y

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The maximum y velocity for the robot in m/s.


#### min_vel_y

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The minimum y velocity for the robot in m/s.


#### max_vel_theta

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- Value setting the maximum rotational velocity. The robot can not spin faster than this.


#### min_speed_theta

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- Value setting the minimum rotational velocity. The robot can not spin slower than this.


#### max_speed_xy

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The absolute value of the maximum translational velocity for the robot in m/s.


#### min_speed_xy

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The absolute value of the minimum translational velocity for the robot in m/s.


#### acc_lim_x

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The x acceleration limit of the robot in meters/sec^2.


#### acc_lim_y

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The y acceleration limit of the robot in meters/sec^2.


#### acc_lim_theta

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The rotational acceleration limit of the robot in radians/sec^2.


#### decel_lim_x

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The deceleration limit of the robot in the x direction in m/s^2.


#### decel_lim_y

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The deceleration limit of the robot in the y direction in m/s^2.


#### decel_lim_theta

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The deceleration limit of the robot in the theta direction in rad/s^2.


#### xy_goal_tolerance

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The x,y distance allowed when the robot reaches its goal pose.


#### yaw_goal_tolerance

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The yaw angle allowed when the robot reaches its goal pose.


#### transform_tolerance

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The allowance for latency for tf messages.


#### sim_time

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- This factor sets forward simulation time in seconds. Setting this too small makes it difficult for the robot to pass through narrow spaces while large values limit dynamic turns. You can observe the differences in length of the yellow line in the below image that representing the simulation path.

![](img/tuning_sim_time.png)

The Navigation2 stack has many parameters to change performances for different robots. Although it’s similar to ROS1 Navigation, please refer to the [Configuration Guide of Navigation2](https://navigation.ros.org/configuration/index.html) or [ROS Navigation Tuning Guide by Kaiyu Zheng](http://kaiyuzheng.me/documents/navguide.pdf) for more details.


### Costmap Parameters


#### inflation_layer.inflation_radius

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- This parameter defines an inflation area of inaccesability around detected obstacles. Generated paths will be planned not to cross this area. It is safe to set this value to be slightly bigger than robot radius. For more information, please refer to [costmap_2d wiki](http://wiki.ros.org/costmap_2d#Inflation) .

![](img/tuning_inflation_radius.png)


#### inflation_layer.cost_scaling_factor

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- This is an inverse proportional factor that is multiplied by the value of the costmap. If this parameter is increased, the value of the costmap is decreased.

![](img/tuning_cost_scaling_factor.png)

The optimal path for the robot to pass through obstacles is to take a median path between them. Setting a smaller value for this parameter will create a farther path from the obstacles.


### dwb_controller


#### max_vel_x

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- This factor sets the maximum value for translational velocity.


#### min_vel_x

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- This factor sets the minimum value for translational velocity. If set this negative, the robot can move backwards.


#### max_vel_y

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The maximum y velocity for the robot in m/s.


#### min_vel_y

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The minimum y velocity for the robot in m/s.


#### max_vel_theta

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- Value setting the maximum rotational velocity. The robot can not spin faster than this.


#### min_speed_theta

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- Value setting the minimum rotational velocity. The robot can not spin slower than this.


#### max_speed_xy

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The absolute value of the maximum translational velocity for the robot in m/s.


#### min_speed_xy

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The absolute value of the minimum translational velocity for the robot in m/s.


#### acc_lim_x

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The x acceleration limit of the robot in meters/sec^2.


#### acc_lim_y

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The y acceleration limit of the robot in meters/sec^2.


#### acc_lim_theta

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The rotational acceleration limit of the robot in radians/sec^2.


#### decel_lim_x

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The deceleration limit of the robot in the x direction in m/s^2.


#### decel_lim_y

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The deceleration limit of the robot in the y direction in m/s^2.


#### decel_lim_theta

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The deceleration limit of the robot in the theta direction in rad/s^2.


#### xy_goal_tolerance

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The x,y distance allowed when the robot reaches its goal pose.


#### yaw_goal_tolerance

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The yaw angle allowed when the robot reaches its goal pose.


#### transform_tolerance

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- The allowance for latency for tf messages.


#### sim_time

- Defined in `turtlebot3_navigation2/param/${TB3_MODEL}.yaml`
- This factor sets forward simulation time in seconds. Setting this too small makes it difficult for the robot to pass through narrow spaces while large values limit dynamic turns. You can observe the differences in length of the yellow line in the below image that representing the simulation path.

![](img/tuning_sim_time.png)

Navigation stack has many parameters to change performance for different robots.

You can get more information about Navigation tuning from [Basic Navigation Tuning Guide](http://wiki.ros.org/navigation/Tutorials/Navigation%20Tuning%20Guide) , [ROS Navigation Tuning Guide by Kaiyu Zheng](http://kaiyuzheng.me/documents/navguide.pdf) , and chapter 11 of [ROS Robot Programming](https://community.robotsource.org/t/download-the-ros-robot-programming-book-for-free/51) book.


### inflation_radius

- Defined in `turtlebot3_navigation/param/costmap_common_param_${TB3_MODEL}.yaml`
- This parameter makes inflation area from the obstacle. Path would be planned in order that it don’t across this area. It is safe that to set this to be bigger than robot radius. For more information, please refer to the [costmap_2d wiki](http://wiki.ros.org/costmap_2d#Inflation) .

![](img/tuning_inflation_radius.png)


### cost_scaling_factor

- Defined in `turtlebot3_navigation/param/costmap_common_param_${TB3_MODEL}.yaml`
- This factor is a reciprical proportional parameter used to multiply the cost values. When this parameter is increased, the cost generated decreases.

![](img/tuning_cost_scaling_factor.png)

The best path is for the robot to pass through a central point between obstacles. Set this factor to be smaller in order to pass farther from obstacles.


### max_vel_x

- Defined in `turtlebot3_navigation/param/dwa_local_planner_params_${TB3_MODEL}.yaml`
- This factor sets the maximum value for translational velocity.


### min_vel_x

- Defined in `turtlebot3_navigation/param/dwa_local_planner_params_${TB3_MODEL}.yaml`
- This factor sets the minimum value for translational velocity. If set to a negative value, the robot can move backwards.


### max_trans_vel

- Defined in `turtlebot3_navigation/param/dwa_local_planner_params_${TB3_MODEL}.yaml`
- Value of the maximum allowable translational velocity. The robot can not move faster than this.


### min_trans_vel

- Defined in `turtlebot3_navigation/param/dwa_local_planner_params_${TB3_MODEL}.yaml`
- Value of the minimum allowable translational velocity. The robot can not move slower than this.


### max_rot_vel

- Defined in `turtlebot3_navigation/param/dwa_local_planner_params_${TB3_MODEL}.yaml`
- Value of the maximum allowable rotational velocity. The robot can not rotate faster than this.


### min_rot_vel

- Defined in `turtlebot3_navigation/param/dwa_local_planner_params_${TB3_MODEL}.yaml`
- Value of the minimum allowable rotational velocity. The robot can not rotate slower than this.


### acc_lim_x

- Defined in `turtlebot3_navigation/param/dwa_local_planner_params_${TB3_MODEL}.yaml`
- Value of the translational acceleration limit.


### acc_lim_theta

- Defined in `turtlebot3_navigation/param/dwa_local_planner_params_${TB3_MODEL}.yaml`
- Value of the rotational acceleration limit.


### xy_goal_tolerance

- Defined in `turtlebot3_navigation/param/dwa_local_planner_params_${TB3_MODEL}.yaml`
- The x,y tolerance allowed when the robot reaches its goal pose.


### yaw_goal_tolerance

- Defined in `turtlebot3_navigation/param/dwa_local_planner_params_${TB3_MODEL}.yaml`
- The yaw angle allowed when the robot reaches its goal pose.


### sim_time

- Defined in `turtlebot3_navigation/param/dwa_local_planner_params_${TB3_MODEL}.yaml`
- This factor sets forward simulation time in seconds. Setting this too small makes it difficult for the robot to pass through narrow spaces while large values limit dynamic turns. You can observe the differences in length of the yellow line in the below image that representing the simulation path.

![](img/tuning_sim_time.png)

---
[TOC](#toc)
# Jazzy



---
[TOC](#toc)
# Noetic




