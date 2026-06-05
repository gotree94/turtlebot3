> **출처**: [https://emanual.robotis.com/docs/en/platform/turtlebot3/fakenode_simulation](https://emanual.robotis.com/docs/en/platform/turtlebot3/fakenode_simulation)

---
# TOC

1. [Humble](#humble)
2. [Jazzy](#jazzy)
3. [Noetic](#noetic)

---
# Humble

## 6.4 Fake Node Simulation

https://youtu.be/iHXZSLBJHMg?si=miugXqHhBTSBol_r

> e-Manual의 내용은 사전 공지 없이 변경될 수 있습니다. 동영상 내용은 e-Manual의 내용과 다를 수 있습니다.

가상 TurtleBot3를 사용하려면 간단한 시뮬레이션 노드인 `turtlebot3_fake_node` 패키지의 `turtlebot3_fake_node.launch.py`를 실행하세요.
다음 지침에 따라 Fake Node를 사용하여 TurtleBot3를 가상 세계로 불러옵니다.

1. `turtlebot3_fake_node.launch.py` 파일을 실행합니다.
`TURTLEBOT3_MODEL` 파라미터를 사용하여 TurtleBot3 모델(`burger`, `waffle`, `waffle_pi`)을 지정하세요.
```
$ export TURTLEBOT3_MODEL=burger
$ ros2 launch turtlebot3_fake_node turtlebot3_fake_node.launch.py
```

2. [Teleoperation](https://emanual.robotis.com/docs/en/platform/turtlebot3/basic_operation/#teleoperation)을 사용하여 가상 TurtleBot3를 제어할 수 있습니다.
```
$ export TURTLEBOT3_MODEL=burger
$ ros2 run turtlebot3_teleop teleop_keyboard
```


---
# Jazzy

## 6.4 Fake Node Simulation

> **참고**: 이 기능은 Humble 및 Noetic에서 사용할 수 있습니다.

---
# Noetic

## 6.4 Fake Node Simulation

https://youtu.be/iHXZSLBJHMg?si=miugXqHhBTSBol_r

> e-Manual의 내용은 사전 공지 없이 변경될 수 있습니다. 동영상 내용은 e-Manual의 내용과 다를 수 있습니다.

* `turtlebot3_fake_node`를 사용하려면 `turtlebot3_simulation` 메타패키지가 필요합니다. 다음 명령어와 같이 패키지를 설치하세요.

> **참고**: `turtlebot3_simulation` 메타패키지는 사전 필수 조건으로 `turtlebot3` 메타패키지와 `turtlebot3_msgs` 패키지가 필요합니다. [PC 설정의 ROS 의존 패키지 설치](https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/#install-dependent-ros-1-packages-1) 섹션에서 설치하지 않았다면 지금 설치하세요.

**[Remote PC]** <br>
```
$ cd ~/catkin_ws/src/
$ git clone -b noetic https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
$ cd ~/catkin_ws && catkin_make
```

가상 로봇을 실행하려면 아래와 같이 `turtlebot3_fake` 패키지의 `turtlebot3_fake.launch` 파일을 실행하세요. `turtlebot3_fake`는 실제 로봇 없이도 실행할 수 있는 매우 간단한 시뮬레이션 노드입니다. 원격 제어 노드를 사용하여 RViz에서 가상 TurtleBot3를 제어할 수도 있습니다.

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
