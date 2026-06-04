# TurtleBot3

> **Source**: [https://emanual.robotis.com/docs/en/platform/turtlebot3/nav_simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/nav_simulation)

---


## Navigation Simulation

Just like with SLAM in the Gazebo simulator, you can select or create various environments and robot models in the virtual Navigation world. However, a complete map has to be prepared before running Navigation. Other than the preparation of a simulation environment instead of bringing up the robot, Navigation Simulation is pretty similar to that of real-world TurtleBot3 [Navigation](https://emanual.robotis.com/docs/en/platform/turtlebot3/navigation/#navigation) .


### Launch Simulation World

Terminate `Ctrl` + `C` all applications that were launched in the previous sections.

In the previous [SLAM](https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/#slam) section, TurtleBot3 World was used to create a map. The same Gazebo environment will be used for Navigation.

Specify your TurtleBot model ( `burger` , `waffle` , `waffle_pi` ) using the `TURTLEBOT3_MODEL` parameter.

```
$ 
export 
TURTLEBOT3_MODEL
=
burger

$ 
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py

```


### Run Navigation Node

Open a new terminal from Remote PC with `Ctrl` + `Alt` + `T` and run the Navigation2 node.

```
$ 
export 
TURTLEBOT3_MODEL
=
burger

$ 
ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:
=
True map:
=
$HOME
/map.yaml

```


### Estimate Initial Pose

**Initial Pose Estimation** must be performed before running Navigation as this process initializes the AMCL parameters that are critical for accurate Navigation. TurtleBot3 has to be correctly located on the map with the LDS sensor data that neatly overlaps the displayed map.

1. Click the `2D Pose Estimate` button in the RViz2 menu.
2. Click on the map where the actual robot is located and drag the large green arrow toward the direction where the robot is facing.
3. Repeat step 1 and 2 until the LDS sensor data is overlaid on the saved map. ![](img/tb3_navigation2_rviz_01.png)
4. Launch keyboard teleoperation node to precisely locate the robot on the map. $ros2 run turtlebot3_teleop teleop_keyboard
5. Move the robot back and forth a bit to collect the surrounding environment information and narrow down the estimated location of the TurtleBot3 on the map which is displayed with tiny green arrows.  ![](img/tb3_amcl_particle_01.png)
6. Terminate the keyboard teleoperation node by entering `Ctrl` + `C` to the teleop node terminal in order to prevent different **cmd_vel** values are published from multiple nodes during Navigation.


### Set Navigation Goal

1. Click the `Navigation2 Goal` button in the RViz2 menu.
2. Click on the map to set the destination of the robot and drag the green arrow toward the direction where the robot will be facing. This green arrow is a marker that can specify the destination of the robot.The root of the arrow isx,ycoordinate of the destination, and the angleθis determined by the orientation of the arrow.As soon as x, y, θ are set, TurtleBot3 will start moving to the destination immediately.


### Launch Simulation World

Terminate `Ctrl` + `C` all applications that were launched in the previous sections.

In the previous [SLAM](https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/#slam) section, TurtleBot3 World was used to create a map. The same Gazebo environment will be used for Navigation.

Specify your TurtleBot model ( `burger` , `waffle` , `waffle_pi` ) using the `TURTLEBOT3_MODEL` parameter.

```
$ 
export 
TURTLEBOT3_MODEL
=
burger

$ 
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py

```


### Run Navigation Node

Open a new terminal from Remote PC with `Ctrl` + `Alt` + `T` and run the Navigation2 node.

```
$ 
export 
TURTLEBOT3_MODEL
=
burger

$ 
ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:
=
True map:
=
$HOME
/map.yaml

```


### Estimate Initial Pose

**Initial Pose Estimation** must be performed before running Navigation as this process initializes the AMCL parameters that are critical for accurate Navigation. TurtleBot3 has to be correctly located on the map with the LDS sensor data that neatly overlaps the displayed map.

1. Click the `2D Pose Estimate` button in the RViz2 menu.
2. Click on the map where the actual robot is located and drag the large green arrow toward the direction where the robot is facing.
3. Repeat step 1 and 2 until the LDS sensor data is overlaid on the saved map. ![](img/tb3_navigation2_rviz_01.png)
4. Launch keyboard teleoperation node to precisely locate the robot on the map. $ros2 run turtlebot3_teleop teleop_keyboard
5. Move the robot back and forth a bit to collect the surrounding environment information and narrow down the estimated location of the TurtleBot3 on the map which is displayed with tiny green arrows.  ![](img/tb3_amcl_particle_01.png)
6. Terminate the keyboard teleoperation node by entering `Ctrl` + `C` to the teleop node terminal in order to prevent different **cmd_vel** values are published from multiple nodes during Navigation.


### Set Navigation Goal

1. Click the `Navigation2 Goal` button in the RViz2 menu.
2. Click on the map to set the destination of the robot and drag the green arrow toward the direction where the robot will be facing. This green arrow is a marker that can specify the destination of the robot.The root of the arrow isx,ycoordinate of the destination, and the angleθis determined by the orientation of the arrow.As soon as x, y, θ are set, TurtleBot3 will start moving to the destination immediately.


### Launch Simulation World

Terminate `Ctrl` + `C` all applications that were launced in the previous sections.

In the previous [SLAM](https://emanual.robotis.com/docs/en/platform/turtlebot3/slam/#slam) section, TurtleBot3 World was used to create a map. The same Gazebo environment will be used for Navigation.

Specify your TurtleBot model ( `burger` , `waffle` , `waffle_pi` ) using the `TURTLEBOT3_MODEL` parameter. **[Remote PC]**

```
$ 
export 
TURTLEBOT3_MODEL
=
burger

$ 
roslaunch turtlebot3_gazebo turtlebot3_world.launch

```


### Run Navigation Node

Open a new terminal on the Remote PC with `Ctrl` + `Alt` + `T` and run the Navigation node.  **[Remote PC]**

```
$ 
export 
TURTLEBOT3_MODEL
=
burger

$ 
roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:
=
$HOME
/map.yaml

```


### Estimate Initial Pose

**Initial Pose Estimation** must be performed before running Navigation as this process initializes the AMCL parameters that are critical for correct Navigation. TurtleBot3 has to be correctly located on the map with LDS sensor data that neatly overlaps the displayed map.

1. Click the `2D Pose Estimate` button in the RViz menu.  ![](img/2d_pose_button.png)
2. Click on the map where the actual robot is located and drag the large green arrow toward the direction where the robot is facing.
3. Repeat step 1 and 2 until the LDS sensor data is overlaid on the saved map.
4. Launch the keyboard teleoperation node to precisely locate the robot on the map.  **[Remote PC]** $roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
5. Move the robot back and forth a bit to collect the surrounding environment information and narrow down the estimated location of the TurtleBot3 on the map which is displayed with tiny green arrows.  ![](img/tb3_amcl_particle_01.png) ![](img/tb3_amcl_particle_02.png)
6. Terminate the keyboard teleoperation node by entering `Ctrl` + `C` to the teleop node terminal in order to prevent different **cmd_vel** values are published from multiple nodes during Navigation.


### Set Navigation Goal

1. Click the `2D Nav Goal` button in the RViz menu.  ![](img/2d_nav_goal_button.png)
2. Click on the map to set the destination of the robot and drag the green arrow toward the direction where the robot will be facing. This green arrow is a marker to specify the destination of the robot.The root of the arrow isx,ycoordinate of the destination, and the angleθis determined by the orientation of the arrow.As soon as x, y, θ are set, TurtleBot3 will start moving to the destination immediately.
