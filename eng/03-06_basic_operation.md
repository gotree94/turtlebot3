> **Source**: [https://emanual.robotis.com/docs/en/platform/turtlebot3/basic_operation](https://emanual.robotis.com/docs/en/platform/turtlebot3/basic_operation)

---
# TOC

1. [Humble](#humble)
2. [Jazzy](#jazzy)
3. [Noetic](#noetic)

---
[TOC](#toc)
# Humble

## 3.6 Basic Operation

### 3.6.1 Teleoperation

* The TurtleBot3 can be teleoperated by remote control. Make sure that the necessary ROS packages are supported for your SBC and ROS version.

https://youtu.be/Z4s18hlazb4?si=BG1B2EDN6CCOHq8y

> **WARNING** : Make sure to run [Bringup](https://emanual.robotis.com/docs/en/platform/turtlebot3/bringup/#bringup) on the TurtleBot3 SBC before teleoperation. Additionally, be careful when testing the robot on the table as the robot may drive over the edge.


#### 3.6.1.1 Keyboard

1. Open a terminal on the **Remote PC** .
2. Run the teleoperation node. Replace the `${TB3_MODEL}` with `burger` or `waffle` or `waffle_pi` , if the TURTLEBOT3_MODEL parameter is not predefined.  
**[Remote PC]**
```
$ export TURTLEBOT3_MODEL=${TB3_MODEL}
$ ros2 run turtlebot3_teleop teleop_keyboard
```

4. If the node is successfully launched, the following instructions will appear on the terminal window.  
**[Remote PC]** 
```
Control Your Turtlebot3
Moving around
     w
 a   s   d
     x
w/x : increase/decrease linear velocity (Burger : ~ 0.22, Waffle and Waffle Pi : ~ 0.26)
a/d : increase/decrease angular velocity (Burger : ~ 2.84, Waffle and Waffle Pi : ~ 1.82)
space key, s : force stop
CTRL-C to quit
```

#### 3.6.1.2 RC-100

The settings for the [ROBOTIS RC-100B](https://emanual.robotis.com/docs/en/parts/communication/rc-100/) are included in the OpenCR firmware for TurtleBot3. It can be used with the [BT410](https://emanual.robotis.com/docs/en/parts/communication/bt-410/) Bluetooth module .TurtleBot3 Waffle Pi includes this controller and Bluetooth modules. When using the RC-100B, it is not necessary to execute a specific node because `turtlebot_core` node creates a `/cmd_vel` topic in the firmware directly connected to OpeCR.

![](img/rc100b_with_bt410.png)

1. Connect BT-410 to any of the OpenCR UART ports.
2. Control the TurtleBot3 with RC-100. Up / Down : Increase or decrease linear velocityLeft / Right : Increase or decrease angular velocity


#### 3.6.1.3 PS3 Joystick

1. Connect the PS3 Joystick to theRemote PCvia Bluetooth or a USB cable.
2. Install the `ds4drv` package using pip.  **[Remote PC]** $sudopipinstallds4drv
3. Launch the joystick node.  **[Remote PC]** $sudods4drv$ros2 run joy joy_node
4. Open a new terminal and launch the teleoperation node.  **[Remote PC]** $ros2 run teleop_twist_joy teleop_node


#### 3.6.1.4 XBOX 360 Joystick

1. Connect the XBOX 360 Joystick to the remote PC with the Wireless Adapter or USB cable.
2. Open a terminal on theRemote PC.
3. Launch the joystick node.  **[Remote PC]** $ros2 run joy joy_node
4. Open a new terminal and launch the teleoperation node.  **[Remote PC]** $ros2 run teleop_twist_joy teleop_node


### 3.6.2 Topic Monitor

1. Run rqt from the PC with the command below. If the topic monitor window is not displayed, select the `plugin` -> `Topics` -> `Topic Monitor` .  
**[Remote PC]** 
```
$rqt
```
![](img/rqt_1.png)

2. When the topic monitor is loaded, the topic values are not monitored by default. Click the checkbox next to each topic to monitor the topic.  <br>
![](img/rqt_2.png)
3. To see more detailed topic messages, click the `▶` icon next to the checkbox.  <br>
![](img/rqt_3.png)
- /battery_stateindicates a message relating to the battery condition, such as the current battery voltage and remaining capacity.  <br>
![](img/rqt_4.png)
- /odomindicates a message containing the odometry of the TurtleBot3. This topic has orientation and position encoder data.  <br>
![](img/rqt_5.png)
- /sensor_stateindicates a message containing encoder values, battery and torque status.  <br>
![](img/rqt_6.png)
- /scanindicates a message containing LDS data, such as angle_max and min, and range_max and min.  <br>
![](img/rqt_7.png)


---
[TOC](#toc)
# Jazzy

## 3.6 Basic Operation


### Teleoperation

The TurtleBot3 can be teleoperated by remote control. Make sure that the necessary ROS packages are supported for your SBC and ROS version.

**WARNING** : Make sure to run [Bringup](https://emanual.robotis.com/docs/en/platform/turtlebot3/bringup/#bringup) on the TurtleBot3 SBC before teleoperation. Additionally, be careful when testing the robot on the table as the robot may drive over the edge.


#### Keyboard

1. Open a terminal on the **Remote PC** .
2. Run the teleoperation node. Replace the `${TB3_MODEL}` with `burger` or `waffle` or `waffle_pi` , if the TURTLEBOT3_MODEL parameter is not predefined.  **[Remote PC]** $exportTURTLEBOT3_MODEL=${TB3_MODEL}$ros2 run turtlebot3_teleop teleop_keyboard
3. If the node is successfully launched, the following instructions will appear on the terminal window.  **[Remote PC]** Control Your Turtlebot3
Moving around
     w
 a   s   d
     x
w/x : increase/decrease linear velocity(Burger : ~ 0.22, Waffle and Waffle Pi : ~ 0.26)a/d : increase/decrease angular velocity(Burger : ~ 2.84, Waffle and Waffle Pi : ~ 1.82)space key, s : force stop
CTRL-C to quit


#### RC-100

The settings for the [ROBOTIS RC-100B](https://emanual.robotis.com/docs/en/parts/communication/rc-100/) are included in the OpenCR firmware for TurtleBot3. It can be used with the [BT410](https://emanual.robotis.com/docs/en/parts/communication/bt-410/) Bluetooth module .TurtleBot3 Waffle Pi includes this controller and Bluetooth modules. When using the RC-100B, it is not necessary to execute a specific node because `turtlebot_core` node creates a `/cmd_vel` topic in the firmware directly connected to OpeCR.

![](img/rc100b_with_bt410.png)

1. Connect BT-410 to any of the OpenCR UART ports.
2. Control the TurtleBot3 with RC-100. Up / Down : Increase or decrease linear velocityLeft / Right : Increase or decrease angular velocity


### Topic Monitor

1. Run rqt from the PC with the command below. If the topic monitor window is not displayed, select the `plugin` -> `Topics` -> `Topic Monitor` .  **[Remote PC]** $rqt
2. When the topic monitor is loaded, the topic values are not monitored by default. Click the checkbox next to each topic to monitor the topic.
3. To see more detailed topic messages, click the `▶` icon next to the checkbox.  ![](img/rqt_3.png)

- /battery_stateindicates a message relating to the battery condition, such as the current battery voltage and remaining capacity.
- /odomindicates a message containing the odometry of the TurtleBot3. This topic has orientation and position encoder data.
- /sensor_stateindicates a message containing encoder values, battery and torque status.
- /scanindicates a message containing LDS data, such as angle_max and min, and range_max and min.



---
[TOC](#toc)
# Noetic

## 3.6 Basic Operation


### Teleoperation

**WARNING** : Make sure to [Bringup](https://emanual.robotis.com/docs/en/platform/turtlebot3/bringup/#bringup) the TurtleBot3 SBC before teleoperation, and be careful when testing the robot on a table as the robot may fall off the edge.

The TurtleBot3 can be teleoperated by remote control. Make sure that the necessary ROS packages are available for your SBC and ROS version.


#### Keyboard

1. Launch the `turtlebot3_teleop_key` node from the Remote PC for teleoperation. Replace the `${TB3_MODEL}` parameter with your specific model name ( `burger` or `waffle` or `waffle_pi` ).  **[Remote PC]** $exportTURTLEBOT3_MODEL=${TB3_MODEL}$roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
2. If the node has successfully launched, the following instructions will appear in the terminal window.  **[Remote PC]** Control Your Turtlebot3
Moving around
     w
 a   s   d
     x
w/x : increase/decrease linear velocity
a/d : increase/decrease angular velocity
space key, s : force stop
CTRL-C to quit


#### RC100

Settings for the [ROBOTIS RC-100B](https://emanual.robotis.com/docs/en/parts/communication/rc-100/) controller are included in the OpenCR firmware for TurtleBot3 Burger, Waffle and Waffle Pi. This controller can be used with the [BT410](https://emanual.robotis.com/docs/en/parts/communication/bt-410/) Bluetooth module. The TurtleBot3 Waffle Pi includes the RC-100 controller and Bluetooth modules. When using the RC-100, it is not necessary to execute a specific node because `turtlebot_core` node creates the required `/cmd_vel` .

![](img/rc100b_with_bt410.png)

1. Connect BT-410 to the OpenCR UART1 port (as describedhere).
2. Control the TurtleBot3 with the RC-100. Up / Down : Increase or decrease linear velocityLeft / Right : Increase or decrease angular velocity


#### PS3 Joystick

1. Connect the PS3 Joystick to the remote PC via Bluetooth or a USB cable.
2. Install the required packages for teleoperation using a PS3 joystick.  **[Remote PC]** $sudoaptinstallros-noetic-joy ros-noetic-joystick-drivers ros-noetic-teleop-twist-joy
3. Launch the teleoperation node.  **[Remote PC]** $roslaunch teleop_twist_joy teleop.launch


#### XBOX 360 Joystick

1. Connect the XBOX 360 Joystick to the remote PC with a Wireless Adapter or USB cable.
2. Install the required packages for teleoperation using the XBOX 360 joystick.  **[Remote PC]** $sudoaptinstallxboxdrv ros-noetic-joy ros-noetic-joystick-drivers ros-noetic-teleop-twist-joy
3. Launch the teleoperation packages for the XBOX 360 joystick.  **[Remote PC]** $sudoxboxdrv--silent$roslaunch teleop_twist_joy teleop.launch


#### Wii Remote

1. Connect the Wii remote to the remote PC via Bluetooth.
2. Install the required packages for teleoperation using the Wii remote.  **[Remote PC]** $sudoaptinstallros-noetic-wiimote libbluetooth-dev libcwiid-dev$cd~/catkin_ws/src$git clone https://github.com/ros-drivers/joystick_drivers.git$cd~/catkin_ws&&catkin_make
3. Run the teleoperation packages for the Wii remote.  **[Remote PC]** $rosrun wiimote wiimote_node$rosrun wiimote teleop_wiimote


### Topic Monitor

1. Run rqt from PC with the command below. If the topic monitor window is not displayed, select the `plugin` -> `Topics` -> `Topic Monitor` . $rqt
2. When the topic monitor is opened, the topic values are not monitored by default. Click the checkbox next to each topic to monitor the topic.
3. To see more detailed topic messages, click the `▶` icon next to the checkbox.  ![](img/rqt_3_1.png)

- /battery_stateindicates a message relating to battery condition, such as the current battery voltage and remaining capacity.
- /diagnosticsindicates a message reporting the status of the components connected to the TurtleBot3, such as a MPU9250, DYNAMIXEL-X, HLS-LFCD-LDS, battery or an OpenCR.
- /odomindicates a message containing the odometry of the TurtleBot3. This topic contains orientation and position encoder data.
- /sensor_stateindicates a message containing encoder values, battery and torque.
- /scanindicates a message with LDS data.
