> **Source**: [https://emanual.robotis.com/docs/en/platform/turtlebot3/docker_container_setup](https://emanual.robotis.com/docs/en/platform/turtlebot3/docker_container_setup)

---


# Docker Container Setup

If you use Docker to set up the TurtleBot3 development environment, you don’t need to set up PC and SBC manually and you can get started quickly and easily.  A Docker container can be used as a replacement for the previous PC and SBC setup.


# 1. Install Docker

- Install Docker by following the official Docker installation guide for Ubuntu: [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- It also possible to install Docker using the apt or snap package manager.


# 2. Set Up TurtleBot3 Docker Container

- The container is created using the same image regardless of whether it is on a PC or an SBC.
- The `start` command creates a container using the image we distributed on Docker Hub.
- In SBC (Raspberry Pi), sudo permission is required to run Docker commands. 
```
$ cd turtlebot3_ws/src/turtlebot3/docker/${ROS_DISTRO}
$ sudo ./container.sh start
```
- Once the container is created, enter the container.
```
$ sudo ./container.sh enter
```


# 3. Set up the bashrc file

- Set up the bashrc file in the docker container.
- Set your `ROS_DOMAIN_ID` , `TURTLEBOT3_MODEL` and `LDS_MODEL` according to your environment.(Uncomment and modify the lines as needed.) 
```
$ nano ~/.bashrc
```
![](img/bashrc_setup.png)

- Save and exit the file with `Ctrl` + `S` , `Ctrl` + `X` . And then, apply the changes to bashrc. 
```
$ source ~/.bashrc
```


# 4. Description

**Using Docker on Remote PC:**

- You can run desired commands directly without building (e.g., nav2, slam, teleop, etc.). 
  * Slam
  ```
  $ ros2 launch turtlebot3_cartographer turtlebot3_cartographer.launch.py
  ```
  
  * Navigation2
  ```
  $ ros2 launch turtlebot3_navigation2 navigation2.launch.py
  ```
  
  * Teleop
  ```
  $ ros2 run turtlebot3_teleop teleop_keyboard.launch.py
  ```

**Using Docker on SBC:**

- You can use bringup directly without building. Bringup$ros2 launch turtlebot3_bringup robot.launch.py
  - Bringup 
  ```
  $ ros2 launch turtlebot3_bringup robot.launch.py
  ```
  
- PI camera is supported. Camera Launch$ros2 launch turtlebot3_bringup camera.launch.py
  - Camera Launch
  ```
  $ ros2 launch turtlebot3_bringup camera.launch.py
  ```
