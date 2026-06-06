> **Source**: [https://emanual.robotis.com/docs/en/platform/turtlebot3/autonomous_driving](https://emanual.robotis.com/docs/en/platform/turtlebot3/autonomous_driving)

---

# TOC

1. [Humble](#humble)
2. [Noetic](#Noetic)

---

[TOC](#toc)
# Humble


![](img/autorace_rbiz_challenge_2017_robots_1.png)


# 8. Autonomous Driving


## 8.1 Getting Started

> **NOTE**
> - The Autorace package was developed for `Ubuntu 22.04` running `ROS 2 Humble Hawksbill` .
> - The Autorace package has only been comprehensively tested for operation in the Gazebo simulator.
> - Instructions for correct simulation setup are available in the [Simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/simulation/) section of the manual.

> For ROS2 Humble, our Autonomous Driving package has only been tested in simulation.

### 8.1.1 Prerequisites

`Remote PC`

- ROS 2 Humble installed on a Laptop or desktop PC.


### 8.1.2 Install Autorace Packages

1. Install the AutoRace meta package on the `Remote PC` . 
```
$ cd ~/turtlebot3_ws/src/
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3_autorace.git
$ cd ~/turtlebot3_ws && colcon build --symlink-install
```

2. Install additional dependent packages on the `Remote PC` . 
```
$ sudo apt install ros-humble-image-transport ros-humble-cv-bridge ros-humble-vision-opencv python3-opencv libopencv-dev ros-humble-image-pipeline
```

### 8.1.3 Setting World Plugin

Add an export line to your ~/.bashrc, put your workspace name in {your_ws}. This plugin allows you to animate dynamic environments in your world.

```
$ echo 'export GAZEBO_PLUGIN_PATH=$HOME/{your_ws}/build/turtlebot3_gazebo:$GAZEBO_PLUGIN_PATH' >> ~/.bashrc
```

### 8.1.4 Setting TurtleBot3 Model

Add an export line to your ~/.bashrc. Autorace only supports the burger_cam model.

```
$ echo 'export TURTLEBOT3_MODEL=burger_cam' >> ~/.bashrc
```


## 8.2 Camera Calibration

Camera calibration is crucial for autonomous driving as it ensures the camera provides accurate data about the robot’s environment. Although the Gazebo simulation simplifies some calibration steps, understanding the calibration process is important for transitioning to a real-world robot. 
Camera calibration typically consists of two steps: **intrinsic calibration** , which deals with the internal camera properties, and **extrinsic calibration** , which aligns the camera’s view with the robot’s coordinate system. In Gazebo, these steps are not required because the simulation uses predefined camera parameters, but these instructions will help you understand the overall process for real hardware deployment.


### 8.2.1 Camera Imaging Calibration

In the Gazebo simulation, camera imaging calibration is unnecessary because the simulated camera does not have lens distortion.

To begin, launch the Gazebo simulation on the Remote PC by running the following command:

```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```


### 8.2.2 Intrinsic Camera Calibration

Intrinsic calibration focuses on correcting lens distortion and determining the camera’s internal properties, such as focal length and optical center.
In real robots, this process is essential, but in a Gazebo simulation, intrinsic calibration is not required because the simulated camera is already distortion-free and provides an ideal image. However, this step is included to help users understand the process for real hardware deployment.

To execute the intrinsic calibration process as it would run on real hardware, launch:

```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

This step will not modify the image output but ensures that the correct topics ( `/camera/image_rect` or `/camera/image_rect_color/compressed` ) are available for subsequent processing.


### 8.2.3 Extrinsic Camera Calibration

Extrinsic calibration aligns the camera’s perspective with the robot’s coordinate system, ensuring that objects detected in the camera’s view correspond to their actual positions in the robot’s environment. In real robots, this process is crucial, but in a Gazebo simulation, the calibration is performed for consistency and to familiarize users with the real-world workflow.

Once the simulation is running, launch the extrinsic calibration process:

```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py calibration_mode:=True
```

This will activate the nodes responsible for camera-to-ground projection and compensation.

**Visualization and Parameter Adjustment**

1. Execute rqt on `Remote PC` . 
```
$ rqt
```
2. Navigate toPlugins>Visualization>Image view. Create two image view windows.

3. Select the `/camera/image_extrinsic_calib` topic in one window and `/camera/image_projected` in the other. The first topic shows an image with a red trapezoidal shape and the latter shows the ground projected view (Bird’s eye view)./camera/image_extrinsic_calib(Left) and/camera/image_projected(Right)

![](img/humble_extrinsic_calibration.png)


4. Navigate toPlugins>Configuration>Dynamic Reconfigure.

5. Adjust the parameters in `/camera/image_projection` and `/camera/image_compensation` to tune the camera’s calibration. 
  * Change the/camera/image_projectionvalue to adjust the/camera/image_extrinsic_calibtopic.
  * Intrinsic camera calibration modifies the perspective of the image in the red trapezoid.
  * Adjust/camera/image_compensationto fine-tune the/camera/image_projectedbird’s-eye view.rqt_reconfigure

![](img/humble_extrinsic_calibration_reconfigure.png)
> rqt_reconfigure

**Saving Calibration Data**

Once the best projection settings are found, the calibration data must be saved to ensure that the parameters persist across sessions. One way to save the extrinsic calibration data is by manually editing the YAML configuration files.

1. Navigate to the directory where the calibration files are stored: 
```
$ cd ~/turtlebot3_ws/src/turtlebot3_autorace/turtlebot3_autorace_camera/calibration/extrinsic_calibration/
```

2. Open the relevant YAML file (e.g., `projection.yaml` ) in a text editor: 
```
$ gedit projection.yaml
```

3. Modify the projection parameters to match the values obtained from dynamic reconfiguration.

This method ensures that the extrinsic calibration parameters are correctly saved for future runs.

![](img/humble_projection_yaml.png)

![](img/humble_compensation_yaml.png)

turtlebot3_autorace_camera/calibration/extrinsic_calibration/projection.yaml(Left)   |   
  turtlebot3_autorace_camera/calibration/extrinsic_calibration/compensation.yaml(Right)


### 8.2.4 Check Calibration Result

After completing the calibration process, follow the instructions below on the `Remote PC` to verify the calibration results.

1. Stop the current extrinsic calibration process.If the extrinsic calibration was launched incalibration_mode:=True, stop the process by closing the terminal or pressingCtrl + C.

2. **Launch the extrinsic calibration node without calibration mode.**  This ensures that the system applies the saved calibration parameters for verification. 
```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

3. Execute rqt and navigate **Plugins** > **Visualization** > **Image view** .
```
$ rqt
```

4. With successful calibration settings, the bird-eye view image should appear like below when the `/camera/image_projected` topic is selected. 

![](img/humble_camera_calibration_rqt_image_view.png)

## 8.3 Lane Detection

Lane detection allows the TurtleBot3 to recognize lane markings and follow them autonomously. The system processes camera images from either a real TurtleBot3 or Gazebo simulation, applies color filtering, and identifies lane boundaries.

https://youtu.be/IqV4huXGBEk?si=Lq4xcuTwPKblm6iD

This section explains how to launch the lane detection system, visualize the detected lane markings, and calibrate the parameters to ensure accurate tracking.

**Launching Lane Detection in Simulation**

To begin, start the Gazebo simulation with a pre-defined lane-tracking course:

```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

Next, run the camera calibration processes, which ensure that the detected lanes are accurately mapped to the robot’s perspective:

```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

These steps activate intrinsic and extrinsic calibration to correct any distortions in the camera feed.

Finally, launch the lane detection node in calibration mode to begin detecting lanes:

```
$ ros2 launch turtlebot3_autorace_detect detect_lane.launch.py calibration_mode:=True
```

**Visualizing Lane Detection Output**

To inspect the detected lanes, open rqt on `Remote PC` :

```
$ rqt
```

Then navigate to **Plugins** > **Visualization** > **Image View** and open three image viewers to display different lane detection results:

- `/detect/image_lane/compressed`  
![](img/noetic_detect_image_lane.png)

- `/detect/image_yellow_lane_marker/compressed` : a yellow range color filtered image.  
![](img/noetic_detect_yellow_lane.png)

- `/detect/image_white_lane_marker/compressed` : a white range color filtered image.  
![](img/noetic_detect_white_lane.png)


These visualizations help confirm that the lane detection algorithm is correctly identifying lane boundaries.

**Calibrating Lane Detection Parameters**

For optimal accuracy, tuning detection parameters is necessary. Adjusting these parameters ensures the robot properly identifies lanes under different lighting and environmental conditions.

1. Open the **lane.yaml** file located in **turtlebot3_autorace_detect/param/lane/** and write your modified values to this file. This will ensure the camera uses the modified parameters for future launches. ```
```
 $ cd ~/turtlebot3_ws/src/turtlebot3_autorace/turtlebot3_autorace_detect/param/lane
 $ gedit lane.yaml
```

![](img/humble_lane_yaml.png)
> Modified lane.yaml file

**Running Lane Tracking**

Once calibration is complete, restart the lane detection node without the calibration option:

```
$ ros2 launch turtlebot3_autorace_detect detect_lane.launch.py
```

Then, launch the lane following control node, which enables TurtleBot3 to automatically follow the detected lanes:

```
$ ros2 launch turtlebot3_autorace_mission control_lane.launch.py
```

## 8.4 Traffic Sign Detection

Traffic sign detection allows the TurtleBot3 to recognize and respond to traffic signs while driving autonomously.

https://youtu.be/DhSZo3dGW6A?si=zIEix-k61qo8slP-

This feature uses the `SIFT` (Scale-Invariant Feature Transform) algorithm, which detects key feature points in an image and compares them to a stored reference image for recognition. Signs with more distinct edges tend to yield better recognition results.

This section explains how to capture and store traffic sign images, configure detection parameters, and run the detection process in the Gazebo simulation.

> **NOTE** : More and better defined edges in the traffic sign increase recognition results from the SIFT algorithm.  Please refer to [this SIFT documentation](https://docs.opencv.org/master/da/df5/tutorial_py_sift_intro.html) for additional information.

**Launching Traffic Sign Detection in Simulation**

Start the Autorace Gazebo simulation to set up the environment:

```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch
```

Then, control the TurtleBot3 manually using the keyboard to navigate the vehicle toward traffic signs:

```
$ ros2 run turtlebot3_teleop teleop_keyboard
```

Position the robot so that traffic signs are clearly visible in the camera feed.

**Capturing and Storing Traffic Sign Images**

To ensure accurate recognition, the system requires pre-captured traffic sign images as reference data. While the repository provides default images, recognition accuracy may vary depending on conditions. **If the SIFT algorithm does not perform well with the provided images, capturing and using your own traffic sign images can improve recognition results** .

1. Open `rqt` , then navigate to Plugins > Visualization > Image View.
2. Create a new image view window and select the topic: `/camera/image_compensated` to display the camera feed.
3. Position the TurtleBot3 so that traffic signs are clearly visible in the camera view.
4. Capture images of each traffic sign and crop any unnecessary background, focusing only on the sign itself.
5. For the best performance, use the original traffic signs from the track whenever possible.

Save the images in the turtlebot3_autorace_detect package **/turtlebot3_autorace/turtlebot3_autorace_detect/image/** .

Ensure that the file names match those used in the source code, as the system references these names:

- The `construction.png` , `intersection.png` , `left.png` , `right.png` , `parking.png` , `stop.png` and `tunnel.png` file names are used by default.

If recognition performance is inconsistent with the default images, manually captured traffic sign images may enhance accuracy and improve overall detection reliability.

**Running Traffic Sign Detection**

Before launching the detection node, run the camera calibration processes to ensure the camera feed is properly aligned:

```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

Then, launch the traffic sign detection node, specifying the mission type:  A specific mission for the **mission** argument must be selected from the following options:

- `intersection` , `construction` , `parking` , `level_crossing` , `tunnel`

```
$ ros2 launch turtlebot3_autorace_detect detect_sign.launch.py mission:=SELECT_MISSION
```

This command starts the detection process and allows TurtleBot3 to recognize and respond to the selected traffic sign. 
> NOTE: Replace theSELECT_MISSIONkeyword with one of the available options above.

**Visualizing Detection Results**

To check the detected traffic signs, open `rqt` , then navigate to: **Plugins > Visualization > Image View**

Create a new image view window and select the topic: `/detect/image_traffic_sign/compressed`

This will display the result of traffic sign detection in real-time. The detected traffic sign will be overlaid on the screen based on the mission assigned.

Below are examples of successfully detected traffic signs for different missions:

noetic_detect_intersection.png

<p align="center">
  <img src="img/noetic_detect_intersection.png" width="45%">
  <img src="img/noetic_detect_left.png" width="45%">
</p>

<p align="center">
    <img src="img/noetic_detect_right.png" width="50%">
</p>

<p align="center">
    <b>Detecting Intersection, Left, and Right signs (mission:=intersection)</b>
</p>

<p align="center">
  <img src="img/noetic_detect_construction.png" width="45%">
  <img src="img/noetic_detect_parking.png" width="45%">
</p>

<p align="center">
    <b>Detecting Construction, and Parking signs (mission:=construction,mission:=parking)</b>
</p>

<p align="center">
  <img src="img/noetic_detect_level_crossing.png" width="45%">
  <img src="img/noetic_detect_tunnel.png" width="45%">
</p>

<p align="center">
    <b>Detecting the Tunnel, and Level Crossing signs (mission:=level_crossing,mission:=tunnel)</b>
</p>



## 8.5 Missions

> AutoRace is a competition for autonomous driving robot platforms designed to provide varied test conditions for autonomous robotics development. The provided open source libraries are based on ROS and are intended to be used as a base for further competitor development. Join Autorace and show off your development skill! **WARNING** : Be sure to read [Autonomous Driving](https://emanual.robotis.com/docs/en/platform/turtlebot3/autonomous_driving#autonomous-driving) in order to start missions.


### 8.5.1 Traffic Lights

This section describes how to complete the traffic light mission by having TurtleBot3 recognize the traffic lights and complete the course.

https://youtu.be/SaU8PjXT5lA?si=jmpXFMQJWpsNsnxW

##### Traffic Lights detection process

1. Filter the image to extract the red, yellow, green color mask images.
2. Locate the circle in the region of interest(RoI) for each masked image.
3. Find the red, yellow, and green traffic lights in that order.


##### Traffic Lights Detection

1. Open a new terminal and launch Autorace Gazebo simulation. 
```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

2. Open a new terminal and launch the intrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

3. Open a new terminal and launch the extrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

4. Open a new terminal and launch the traffic light detection node with a calibration option. 
```
$ ros2 launch turtlebot3_autorace_detect detect_traffic_light.launch.py calibration_mode:=True
```

5. Execute rqt on `Remote PC`.
```
$ rqt
```

6. Navigate toPlugins>Visualization>Image view. Create two image view windows.
7. In one window, select the `/detect/image_traffic_light/compressed` topic. In another window, select one of the four topics to view the masked images:`/detect/image_red_light`,`/detect/image_yellow_light`,`/detect/image_green_light`,`/detect/image_traffic_light`. <br>
![](img/noetic_detect_traffic_light_green.png) <br>
> Detecting the Yellow light. The image on the right displays/detect/image_yellow_lighttopic. 

![](img/noetic_detect_traffic_light_yellow.png) <br>
> Detecting the Yellow light. The image on the right displays/detect/image_yellow_lighttopic. 

![](img/noetic_detect_traffic_light_red.png) <br>
> Detecting the Red light. The image on the right displays/detect/image_red_lighttopic.

8. Navigate to **Plugins** > **Configuration** > **Dynamic Reconfigure** .
9. Adjust the parameters in `/detect/traffic_light` to adjust the configuration of each masked image topic. Traffic light reconfigure

![](img/noetic_traffic_light_reconfigure.png)

##### Saving Calibration Data

1. Open the `traffic_light.yaml` file located at **turtlebot3_autorace_detect/param/traffic_light/** . 
```
$ gedit ~/turtlebot3_ws/src/turtlebot3_autorace_2020/turtlebot3_autorace_detect/param/traffic_light/traffic_light.yaml
```

![](img/humble_traffic_light_yaml.png)
> turtlebot3_autorace_detect/param/traffic_light/’traffic_light.yaml’


2. Write the modified values and save the file to keep your changes.


##### Testing Traffic Light Detection

1. Close all terminals or terminate them with `Ctrl + C`
2. Open a new terminal and launch Autorace Gazebo simulation. 
```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.p
```

3. Open a new terminal and launch the intrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

4. Open a new terminal and launch the extrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

5. Open a new terminal and launch the traffic light detection node. 
```
$ ros2 launch turtlebot3_autorace_detect detect_traffic_light.launch.py
```

6. Open a new terminal and execute the rqt_image_view.
```
$ rqt
```

7. Check each topics: `/detect/image_red_light` , `/detect/image_yellow_light` , `/detect/image_green_light` .


### 8.5.2 Intersection

> This mission does not have any associated example code.


### 8.5.3 Construction

This section describes how to complete the construction mission. If the TurtleBot encounters an object while following a lane, it will swerve into the opposite lane to avoid the object before returning to its original lane.

https://youtu.be/eMk39rIGiPc?si=3QS5lWOStUy4JbRQ

##### Construction avoidance process

1. The TurtleBot is following a lane and it determines that there may be an obstacle in its path.
2. If an obstacle is detected within the danger zone, Turtlebot swerves to the opposite lane to avoid the obstacle.
3. The TurtleBot returns to it’s original lane again and continues following it.


##### How to Run Construction Mission

1. Close all terminals or terminate them with `Ctrl + C`
2. Open a new terminal and launch the Autorace Gazebo simulation. 
```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

3. Open a new terminal and launch the intrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

4. Open a new terminal and launch the extrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

5. Open a new terminal and launch the construction mission node. 
```
$ ros2 launch turtlebot3_autorace_mission mission_construction.launch.py
```

6. On the image window, you can watch the LiDAR visualization. The detected lidar points, and danger zone are displayed.

![](img/humble_construction_image_window.png)


### 8.5.4 Parking

This mission does not have any associated example code.


### 8.5.5 Level Crossing

This section describes how you can detect a traffic bar. TurtleBot should detect the stop sign and wait for the crossing gate to open.

https://youtu.be/sy8LAX8KEY0?si=MzTXCEHDUQqu_8DE

##### Level Crossing detection process

1. Filter the image to extract the red color mask image.
2. Find the rectangle in the masked image.
3. Connect the three squares to make a straight line.
4. Determine whether the bar is open or closed by measuring the slope of the line.


##### Level Crossing Detection

1. Close all terminals or terminate them with `Ctrl + C` 
2. Open a new terminal and launch Autorace Gazebo simulation. 
```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

3. Open a new terminal and launch the intrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

4. Open a new terminal and launch the extrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

5. Open a new terminal and launch the level crossing detection node with a calibration option. 
```
$ ros2 launch turtlebot3_autorace_detect detect_level_crossing.launch.py calibration_mode:=True
```

6. Open a new terminal and execute rqt.
```
$ rqt
```

7. Select two topics on Image View Plugin:/detect/image_level_color_filtered/compressed,/detect/image_level/compressed.

![](img/noetic_detect_level.png)

8. Adjust parameters in thedetect_level_crossingon Dynamic Reconfigure Plugin

![](img/noetic_level_reconfigure.png)

9. Open `level.yaml` file located at **turtlebot3_autorace_detect/param/level/** . 
```
$ gedit ~/turtlebot3_ws/src/turtlebot3_autorace/turtlebot3_autorace_detect/param/level/level.yaml
```

![](img/humble_level_yaml.png)

10. Write modified values to the file and save.


##### Testing Level Crossing Detection

1. Close all terminals or terminate them with `Ctrl + C`
2. Open a new terminal and launch Autorace Gazebo simulation. 
```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

3. Open a new terminal and launch the intrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

4. Open a new terminal and launch the extrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

5. Open a new terminal and launch the level crossing detection node. 
```
$ ros2 launch turtlebot3_autorace_detect detect_level_crossing.launch.py
```

6. Open a new terminal and execute the rqt_image_view.
```
$ rqt
```

7. Check the image topic: `/detect/image_level/compressed` on Image View Plugin.


### 8.5.6 Tunnel

This section describes how to complete the tunnel mission. The TurtleBot must use maps and navigation to proceed through obstacle areas with no lanes.

https://youtu.be/pPS3tM90gAc?si=SSkRh1aCL2pdvHYn

##### How to Run Tunnel Mission

> **NOTE** : Change the navigation parameters in the **turtlebot3/turtlebot3_navigation2/param/buger_cam** file. If you slam and make a new map, Place the new map in the turtlebot3_autorace package at **/turtlebot3_autorace/turtlebot3_autorace_tunnel/map/** .

1. Close all terminals or terminate them with `Ctrl + C`

2. Open a new terminal and launch the Autorace Gazebo simulation. 
```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

3. Open a new terminal and launch the tunnel mission node. This node runs the navigation and specifies the initial and target locations. 
```
$ ros2 launch turtlebot3_autorace_mission mission_tunnel.launch.py
```

4. On the Rviz2 screen, you can watch the TurtleBot generate and follow a path in real-time.

![](img/humble_tunnel_rviz.png)


##### Set Initial Position and Goal Position

You can modify the initial position and goal position to fit your plan.

1. Open the `navigation.yaml` file located at **turtlebot3_autorace_mission/param/** . 
```
$ gedit ~/turtlebot3_ws/src/turtlebot3_autorace/turtlebot3_autorace_mission/param/navigation.yaml
```

![](img/humble_tunnel_yaml.png)

2. Write modified values and save the file.

---

[TOC](#toc)
# Noetic


# 8. Autonomous Driving


## 8.1 Getting Started

> **NOTE**
> - The Autorace package was developed for `Ubuntu 22.04` running `ROS 2 Humble Hawksbill` .
> - The Autorace package has only been comprehensively tested for operation in the Gazebo simulator.
> - Instructions for correct simulation setup are available in the [Simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/simulation/) section of the manual.

> For ROS2 Humble, our Autonomous Driving package has only been tested in simulation.

### 8.1.1 Prerequisites

`Remote PC`

- ROS 2 Humble installed on a Laptop or desktop PC.


### 8.1.2 Install Autorace Packages

1. Install the AutoRace meta package on the `Remote PC` . 
```
$ cd ~/turtlebot3_ws/src/
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3_autorace.git
$ cd ~/turtlebot3_ws && colcon build --symlink-install
```

2. Install additional dependent packages on the `Remote PC` . 
```
$ sudo apt install ros-humble-image-transport ros-humble-cv-bridge ros-humble-vision-opencv python3-opencv libopencv-dev ros-humble-image-pipeline
```

### 8.1.3 Setting World Plugin

Add an export line to your ~/.bashrc, put your workspace name in {your_ws}. This plugin allows you to animate dynamic environments in your world.

```
$ echo 'export GAZEBO_PLUGIN_PATH=$HOME/{your_ws}/build/turtlebot3_gazebo:$GAZEBO_PLUGIN_PATH' >> ~/.bashrc
```

### 8.1.4 Setting TurtleBot3 Model

Add an export line to your ~/.bashrc. Autorace only supports the burger_cam model.

```
$ echo 'export TURTLEBOT3_MODEL=burger_cam' >> ~/.bashrc
```


## 8.2 Camera Calibration

Camera calibration is crucial for autonomous driving as it ensures the camera provides accurate data about the robot’s environment. Although the Gazebo simulation simplifies some calibration steps, understanding the calibration process is important for transitioning to a real-world robot. 
Camera calibration typically consists of two steps: **intrinsic calibration** , which deals with the internal camera properties, and **extrinsic calibration** , which aligns the camera’s view with the robot’s coordinate system. In Gazebo, these steps are not required because the simulation uses predefined camera parameters, but these instructions will help you understand the overall process for real hardware deployment.


### 8.2.1 Camera Imaging Calibration

In the Gazebo simulation, camera imaging calibration is unnecessary because the simulated camera does not have lens distortion.

To begin, launch the Gazebo simulation on the Remote PC by running the following command:

```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```


### 8.2.2 Intrinsic Camera Calibration

Intrinsic calibration focuses on correcting lens distortion and determining the camera’s internal properties, such as focal length and optical center.
In real robots, this process is essential, but in a Gazebo simulation, intrinsic calibration is not required because the simulated camera is already distortion-free and provides an ideal image. However, this step is included to help users understand the process for real hardware deployment.

To execute the intrinsic calibration process as it would run on real hardware, launch:

```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

This step will not modify the image output but ensures that the correct topics ( `/camera/image_rect` or `/camera/image_rect_color/compressed` ) are available for subsequent processing.


### 8.2.3 Extrinsic Camera Calibration

Extrinsic calibration aligns the camera’s perspective with the robot’s coordinate system, ensuring that objects detected in the camera’s view correspond to their actual positions in the robot’s environment. In real robots, this process is crucial, but in a Gazebo simulation, the calibration is performed for consistency and to familiarize users with the real-world workflow.

Once the simulation is running, launch the extrinsic calibration process:

```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py calibration_mode:=True
```

This will activate the nodes responsible for camera-to-ground projection and compensation.

**Visualization and Parameter Adjustment**

1. Execute rqt on `Remote PC` . 
```
$ rqt
```
2. Navigate toPlugins>Visualization>Image view. Create two image view windows.

3. Select the `/camera/image_extrinsic_calib` topic in one window and `/camera/image_projected` in the other. The first topic shows an image with a red trapezoidal shape and the latter shows the ground projected view (Bird’s eye view)./camera/image_extrinsic_calib(Left) and/camera/image_projected(Right)

![](img/humble_extrinsic_calibration.png)


4. Navigate toPlugins>Configuration>Dynamic Reconfigure.

5. Adjust the parameters in `/camera/image_projection` and `/camera/image_compensation` to tune the camera’s calibration. 
  * Change the/camera/image_projectionvalue to adjust the/camera/image_extrinsic_calibtopic.
  * Intrinsic camera calibration modifies the perspective of the image in the red trapezoid.
  * Adjust/camera/image_compensationto fine-tune the/camera/image_projectedbird’s-eye view.rqt_reconfigure

![](img/humble_extrinsic_calibration_reconfigure.png)
> rqt_reconfigure

**Saving Calibration Data**

Once the best projection settings are found, the calibration data must be saved to ensure that the parameters persist across sessions. One way to save the extrinsic calibration data is by manually editing the YAML configuration files.

1. Navigate to the directory where the calibration files are stored: 
```
$ cd ~/turtlebot3_ws/src/turtlebot3_autorace/turtlebot3_autorace_camera/calibration/extrinsic_calibration/
```

2. Open the relevant YAML file (e.g., `projection.yaml` ) in a text editor: 
```
$ gedit projection.yaml
```

3. Modify the projection parameters to match the values obtained from dynamic reconfiguration.

This method ensures that the extrinsic calibration parameters are correctly saved for future runs.

![](img/humble_projection_yaml.png)

![](img/humble_compensation_yaml.png)

turtlebot3_autorace_camera/calibration/extrinsic_calibration/projection.yaml(Left)   |   
  turtlebot3_autorace_camera/calibration/extrinsic_calibration/compensation.yaml(Right)


### 8.2.4 Check Calibration Result

After completing the calibration process, follow the instructions below on the `Remote PC` to verify the calibration results.

1. Stop the current extrinsic calibration process.If the extrinsic calibration was launched incalibration_mode:=True, stop the process by closing the terminal or pressingCtrl + C.

2. **Launch the extrinsic calibration node without calibration mode.**  This ensures that the system applies the saved calibration parameters for verification. 
```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

3. Execute rqt and navigate **Plugins** > **Visualization** > **Image view** .
```
$ rqt
```

4. With successful calibration settings, the bird-eye view image should appear like below when the `/camera/image_projected` topic is selected. 

![](img/humble_camera_calibration_rqt_image_view.png)

## 8.3 Lane Detection

Lane detection allows the TurtleBot3 to recognize lane markings and follow them autonomously. The system processes camera images from either a real TurtleBot3 or Gazebo simulation, applies color filtering, and identifies lane boundaries.

https://youtu.be/IqV4huXGBEk?si=Lq4xcuTwPKblm6iD

This section explains how to launch the lane detection system, visualize the detected lane markings, and calibrate the parameters to ensure accurate tracking.

**Launching Lane Detection in Simulation**

To begin, start the Gazebo simulation with a pre-defined lane-tracking course:

```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

Next, run the camera calibration processes, which ensure that the detected lanes are accurately mapped to the robot’s perspective:

```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

These steps activate intrinsic and extrinsic calibration to correct any distortions in the camera feed.

Finally, launch the lane detection node in calibration mode to begin detecting lanes:

```
$ ros2 launch turtlebot3_autorace_detect detect_lane.launch.py calibration_mode:=True
```

**Visualizing Lane Detection Output**

To inspect the detected lanes, open rqt on `Remote PC` :

```
$ rqt
```

Then navigate to **Plugins** > **Visualization** > **Image View** and open three image viewers to display different lane detection results:

- `/detect/image_lane/compressed`  
![](img/noetic_detect_image_lane.png)

- `/detect/image_yellow_lane_marker/compressed` : a yellow range color filtered image.  
![](img/noetic_detect_yellow_lane.png)

- `/detect/image_white_lane_marker/compressed` : a white range color filtered image.  
![](img/noetic_detect_white_lane.png)


These visualizations help confirm that the lane detection algorithm is correctly identifying lane boundaries.

**Calibrating Lane Detection Parameters**

For optimal accuracy, tuning detection parameters is necessary. Adjusting these parameters ensures the robot properly identifies lanes under different lighting and environmental conditions.

1. Open the **lane.yaml** file located in **turtlebot3_autorace_detect/param/lane/** and write your modified values to this file. This will ensure the camera uses the modified parameters for future launches. ```
```
 $ cd ~/turtlebot3_ws/src/turtlebot3_autorace/turtlebot3_autorace_detect/param/lane
 $ gedit lane.yaml
```

![](img/humble_lane_yaml.png)
> Modified lane.yaml file

**Running Lane Tracking**

Once calibration is complete, restart the lane detection node without the calibration option:

```
$ ros2 launch turtlebot3_autorace_detect detect_lane.launch.py
```

Then, launch the lane following control node, which enables TurtleBot3 to automatically follow the detected lanes:

```
$ ros2 launch turtlebot3_autorace_mission control_lane.launch.py
```

## 8.4 Traffic Sign Detection

Traffic sign detection allows the TurtleBot3 to recognize and respond to traffic signs while driving autonomously.

https://youtu.be/DhSZo3dGW6A?si=zIEix-k61qo8slP-

This feature uses the `SIFT` (Scale-Invariant Feature Transform) algorithm, which detects key feature points in an image and compares them to a stored reference image for recognition. Signs with more distinct edges tend to yield better recognition results.

This section explains how to capture and store traffic sign images, configure detection parameters, and run the detection process in the Gazebo simulation.

> **NOTE** : More and better defined edges in the traffic sign increase recognition results from the SIFT algorithm.  Please refer to [this SIFT documentation](https://docs.opencv.org/master/da/df5/tutorial_py_sift_intro.html) for additional information.

**Launching Traffic Sign Detection in Simulation**

Start the Autorace Gazebo simulation to set up the environment:

```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch
```

Then, control the TurtleBot3 manually using the keyboard to navigate the vehicle toward traffic signs:

```
$ ros2 run turtlebot3_teleop teleop_keyboard
```

Position the robot so that traffic signs are clearly visible in the camera feed.

**Capturing and Storing Traffic Sign Images**

To ensure accurate recognition, the system requires pre-captured traffic sign images as reference data. While the repository provides default images, recognition accuracy may vary depending on conditions. **If the SIFT algorithm does not perform well with the provided images, capturing and using your own traffic sign images can improve recognition results** .

1. Open `rqt` , then navigate to Plugins > Visualization > Image View.
2. Create a new image view window and select the topic: `/camera/image_compensated` to display the camera feed.
3. Position the TurtleBot3 so that traffic signs are clearly visible in the camera view.
4. Capture images of each traffic sign and crop any unnecessary background, focusing only on the sign itself.
5. For the best performance, use the original traffic signs from the track whenever possible.

Save the images in the turtlebot3_autorace_detect package **/turtlebot3_autorace/turtlebot3_autorace_detect/image/** .

Ensure that the file names match those used in the source code, as the system references these names:

- The `construction.png` , `intersection.png` , `left.png` , `right.png` , `parking.png` , `stop.png` and `tunnel.png` file names are used by default.

If recognition performance is inconsistent with the default images, manually captured traffic sign images may enhance accuracy and improve overall detection reliability.

**Running Traffic Sign Detection**

Before launching the detection node, run the camera calibration processes to ensure the camera feed is properly aligned:

```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

Then, launch the traffic sign detection node, specifying the mission type:  A specific mission for the **mission** argument must be selected from the following options:

- `intersection` , `construction` , `parking` , `level_crossing` , `tunnel`

```
$ ros2 launch turtlebot3_autorace_detect detect_sign.launch.py mission:=SELECT_MISSION
```

This command starts the detection process and allows TurtleBot3 to recognize and respond to the selected traffic sign. 
> NOTE: Replace theSELECT_MISSIONkeyword with one of the available options above.

**Visualizing Detection Results**

To check the detected traffic signs, open `rqt` , then navigate to: **Plugins > Visualization > Image View**

Create a new image view window and select the topic: `/detect/image_traffic_sign/compressed`

This will display the result of traffic sign detection in real-time. The detected traffic sign will be overlaid on the screen based on the mission assigned.

Below are examples of successfully detected traffic signs for different missions:

noetic_detect_intersection.png

<p align="center">
  <img src="img/noetic_detect_intersection.png" width="45%">
  <img src="img/noetic_detect_left.png" width="45%">
</p>

<p align="center">
    <img src="img/noetic_detect_right.png" width="50%">
</p>

<p align="center">
    <b>Detecting Intersection, Left, and Right signs (mission:=intersection)</b>
</p>

<p align="center">
  <img src="img/noetic_detect_construction.png" width="45%">
  <img src="img/noetic_detect_parking.png" width="45%">
</p>

<p align="center">
    <b>Detecting Construction, and Parking signs (mission:=construction,mission:=parking)</b>
</p>

<p align="center">
  <img src="img/noetic_detect_level_crossing.png" width="45%">
  <img src="img/noetic_detect_tunnel.png" width="45%">
</p>

<p align="center">
    <b>Detecting the Tunnel, and Level Crossing signs (mission:=level_crossing,mission:=tunnel)</b>
</p>



## 8.5 Missions

> AutoRace is a competition for autonomous driving robot platforms designed to provide varied test conditions for autonomous robotics development. The provided open source libraries are based on ROS and are intended to be used as a base for further competitor development. Join Autorace and show off your development skill! **WARNING** : Be sure to read [Autonomous Driving](https://emanual.robotis.com/docs/en/platform/turtlebot3/autonomous_driving#autonomous-driving) in order to start missions.


### 8.5.1 Traffic Lights

This section describes how to complete the traffic light mission by having TurtleBot3 recognize the traffic lights and complete the course.

https://youtu.be/SaU8PjXT5lA?si=jmpXFMQJWpsNsnxW

##### Traffic Lights detection process

1. Filter the image to extract the red, yellow, green color mask images.
2. Locate the circle in the region of interest(RoI) for each masked image.
3. Find the red, yellow, and green traffic lights in that order.


##### Traffic Lights Detection

1. Open a new terminal and launch Autorace Gazebo simulation. 
```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

2. Open a new terminal and launch the intrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

3. Open a new terminal and launch the extrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

4. Open a new terminal and launch the traffic light detection node with a calibration option. 
```
$ ros2 launch turtlebot3_autorace_detect detect_traffic_light.launch.py calibration_mode:=True
```

5. Execute rqt on `Remote PC`.
```
$ rqt
```

6. Navigate toPlugins>Visualization>Image view. Create two image view windows.
7. In one window, select the `/detect/image_traffic_light/compressed` topic. In another window, select one of the four topics to view the masked images:`/detect/image_red_light`,`/detect/image_yellow_light`,`/detect/image_green_light`,`/detect/image_traffic_light`. <br>
![](img/noetic_detect_traffic_light_green.png) <br>
> Detecting the Yellow light. The image on the right displays/detect/image_yellow_lighttopic. 

![](img/noetic_detect_traffic_light_yellow.png) <br>
> Detecting the Yellow light. The image on the right displays/detect/image_yellow_lighttopic. 

![](img/noetic_detect_traffic_light_red.png) <br>
> Detecting the Red light. The image on the right displays/detect/image_red_lighttopic.

8. Navigate to **Plugins** > **Configuration** > **Dynamic Reconfigure** .
9. Adjust the parameters in `/detect/traffic_light` to adjust the configuration of each masked image topic. Traffic light reconfigure

![](img/noetic_traffic_light_reconfigure.png)

##### Saving Calibration Data

1. Open the `traffic_light.yaml` file located at **turtlebot3_autorace_detect/param/traffic_light/** . 
```
$ gedit ~/turtlebot3_ws/src/turtlebot3_autorace_2020/turtlebot3_autorace_detect/param/traffic_light/traffic_light.yaml
```

![](img/humble_traffic_light_yaml.png)
> turtlebot3_autorace_detect/param/traffic_light/’traffic_light.yaml’


2. Write the modified values and save the file to keep your changes.


##### Testing Traffic Light Detection

1. Close all terminals or terminate them with `Ctrl + C`
2. Open a new terminal and launch Autorace Gazebo simulation. 
```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.p
```

3. Open a new terminal and launch the intrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

4. Open a new terminal and launch the extrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

5. Open a new terminal and launch the traffic light detection node. 
```
$ ros2 launch turtlebot3_autorace_detect detect_traffic_light.launch.py
```

6. Open a new terminal and execute the rqt_image_view.
```
$ rqt
```

7. Check each topics: `/detect/image_red_light` , `/detect/image_yellow_light` , `/detect/image_green_light` .


### 8.5.2 Intersection

> This mission does not have any associated example code.


### 8.5.3 Construction

This section describes how to complete the construction mission. If the TurtleBot encounters an object while following a lane, it will swerve into the opposite lane to avoid the object before returning to its original lane.

https://youtu.be/eMk39rIGiPc?si=3QS5lWOStUy4JbRQ

##### Construction avoidance process

1. The TurtleBot is following a lane and it determines that there may be an obstacle in its path.
2. If an obstacle is detected within the danger zone, Turtlebot swerves to the opposite lane to avoid the obstacle.
3. The TurtleBot returns to it’s original lane again and continues following it.


##### How to Run Construction Mission

1. Close all terminals or terminate them with `Ctrl + C`
2. Open a new terminal and launch the Autorace Gazebo simulation. 
```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

3. Open a new terminal and launch the intrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

4. Open a new terminal and launch the extrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

5. Open a new terminal and launch the construction mission node. 
```
$ ros2 launch turtlebot3_autorace_mission mission_construction.launch.py
```

6. On the image window, you can watch the LiDAR visualization. The detected lidar points, and danger zone are displayed.

![](img/humble_construction_image_window.png)


### 8.5.4 Parking

This mission does not have any associated example code.


### 8.5.5 Level Crossing

This section describes how you can detect a traffic bar. TurtleBot should detect the stop sign and wait for the crossing gate to open.

https://youtu.be/sy8LAX8KEY0?si=MzTXCEHDUQqu_8DE

##### Level Crossing detection process

1. Filter the image to extract the red color mask image.
2. Find the rectangle in the masked image.
3. Connect the three squares to make a straight line.
4. Determine whether the bar is open or closed by measuring the slope of the line.


##### Level Crossing Detection

1. Close all terminals or terminate them with `Ctrl + C` 
2. Open a new terminal and launch Autorace Gazebo simulation. 
```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

3. Open a new terminal and launch the intrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

4. Open a new terminal and launch the extrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

5. Open a new terminal and launch the level crossing detection node with a calibration option. 
```
$ ros2 launch turtlebot3_autorace_detect detect_level_crossing.launch.py calibration_mode:=True
```

6. Open a new terminal and execute rqt.
```
$ rqt
```

7. Select two topics on Image View Plugin:/detect/image_level_color_filtered/compressed,/detect/image_level/compressed.

![](img/noetic_detect_level.png)

8. Adjust parameters in thedetect_level_crossingon Dynamic Reconfigure Plugin

![](img/noetic_level_reconfigure.png)

9. Open `level.yaml` file located at **turtlebot3_autorace_detect/param/level/** . 
```
$ gedit ~/turtlebot3_ws/src/turtlebot3_autorace/turtlebot3_autorace_detect/param/level/level.yaml
```

![](img/humble_level_yaml.png)

10. Write modified values to the file and save.


##### Testing Level Crossing Detection

1. Close all terminals or terminate them with `Ctrl + C`
2. Open a new terminal and launch Autorace Gazebo simulation. 
```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

3. Open a new terminal and launch the intrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera intrinsic_camera_calibration.launch.py
```

4. Open a new terminal and launch the extrinsic calibration node. 
```
$ ros2 launch turtlebot3_autorace_camera extrinsic_camera_calibration.launch.py
```

5. Open a new terminal and launch the level crossing detection node. 
```
$ ros2 launch turtlebot3_autorace_detect detect_level_crossing.launch.py
```

6. Open a new terminal and execute the rqt_image_view.
```
$ rqt
```

7. Check the image topic: `/detect/image_level/compressed` on Image View Plugin.


### 8.5.6 Tunnel

This section describes how to complete the tunnel mission. The TurtleBot must use maps and navigation to proceed through obstacle areas with no lanes.

https://youtu.be/pPS3tM90gAc?si=SSkRh1aCL2pdvHYn

##### How to Run Tunnel Mission

> **NOTE** : Change the navigation parameters in the **turtlebot3/turtlebot3_navigation2/param/buger_cam** file. If you slam and make a new map, Place the new map in the turtlebot3_autorace package at **/turtlebot3_autorace/turtlebot3_autorace_tunnel/map/** .

1. Close all terminals or terminate them with `Ctrl + C`

2. Open a new terminal and launch the Autorace Gazebo simulation. 
```
$ ros2 launch turtlebot3_gazebo turtlebot3_autorace_2020.launch.py
```

3. Open a new terminal and launch the tunnel mission node. This node runs the navigation and specifies the initial and target locations. 
```
$ ros2 launch turtlebot3_autorace_mission mission_tunnel.launch.py
```

4. On the Rviz2 screen, you can watch the TurtleBot generate and follow a path in real-time.

![](img/humble_tunnel_rviz.png)


##### Set Initial Position and Goal Position

You can modify the initial position and goal position to fit your plan.

1. Open the `navigation.yaml` file located at **turtlebot3_autorace_mission/param/** . 
```
$ gedit ~/turtlebot3_ws/src/turtlebot3_autorace/turtlebot3_autorace_mission/param/navigation.yaml
```

![](img/humble_tunnel_yaml.png)

2. Write modified values and save the file.
