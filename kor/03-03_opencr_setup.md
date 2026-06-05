# TurtleBot3

> **출처**: [https://emanual.robotis.com/docs/en/platform/turtlebot3/opencr_setup](https://emanual.robotis.com/docs/en/platform/turtlebot3/opencr_setup)

---

## OpenCR 설정

1. 마이크로 USB 케이블을 사용하여 [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/)을 Raspberry Pi에 연결하세요.  ![](img/opencr_setup.png)
2. [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) 펌웨어를 업로드하기 위해 Raspberry Pi에 필요한 패키지를 설치하세요.  **[TurtleBot3 SBC]** $sudodpkg--add-architecturearmhf$sudoapt-get update$sudoapt-getinstalllibc6:armhf
3. 모델에 따라 **OPENCR_MODEL** 이름을 `burger` 또는 `waffle`로 지정하세요.  **[TurtleBot3 SBC]** $exportOPENCR_PORT=/dev/ttyACM0$exportOPENCR_MODEL=burger$rm-rf./opencr_update.tar.bz2
4. 펌웨어와 필요한 로더를 다운로드한 후 파일을 추출하여 업로드 준비를 합니다. **[TurtleBot3 SBC]** $wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS2/latest/opencr_update.tar.bz2$tar-xvfopencr_update.tar.bz2
5. OpenCR에 펌웨어를 업로드합니다.  **[TurtleBot3 SBC]** $cd./opencr_update$./update.sh$OPENCR_PORT$OPENCR_MODEL.opencr
6. TurtleBot3 Burger의 성공적인 펌웨어 업로드 화면은 다음과 같습니다.
7. 펌웨어 업로드가 실패하면 다음 지침에 따라 복구 모드를 통해 다시 업로드하세요. 복구 모드에서는 [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/)의 `STATUS` LED가 주기적으로 깜빡입니다. PUSH SW2 버튼을 누른 상태에서 Reset 버튼을 누르고, Reset 버튼을 뗀 후 PUSH SW2 버튼을 놓으세요.


### OpenCR 테스트

> **참고**: OpenCR 테스트 중 바퀴가 움직이지 않으면 "**TurtleBot3 DYNAMIXEL 설정 방법**"을 참조하여 DYNAMIXEL 구성을 업데이트하세요.

`PUSH SW 1` 및 `PUSH SW 2` 버튼을 사용하여 로봇이 올바르게 조립되었는지 확인할 수 있습니다. 이 과정은 좌우 DYNAMIXEL 구성과 [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) 보드 펌웨어를 테스트합니다.

![](img/opencr_models.png)

1. TurtleBot3 조립 후 [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/)에 전원을 연결하고 전원 스위치를 켭니다. 빨간색 `Power LED`가 켜집니다.
2. 로봇을 넓고 평평한 바닥에 놓습니다. 테스트 시 안전 반경 1미터(40인치)를 권장합니다.
3. `PUSH SW 1`을 몇 초간 눌러 로봇이 약 30센티미터(약 12인치) 전진하도록 명령합니다.
4. `PUSH SW 2`를 몇 초간 눌러 로봇이 제자리에서 180도 회전하도록 명령합니다.


## OpenCR 설정

1. 마이크로 USB 케이블을 사용하여 [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/)을 Raspberry Pi에 연결하세요.  ![](img/opencr_setup.png)
2. [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) 펌웨어를 업로드하기 위해 Raspberry Pi에 필요한 패키지를 설치하세요.  **[TurtleBot3 SBC]** $sudodpkg--add-architecturearmhf$sudoapt-get update$sudoapt-getinstalllibc6:armhf
3. 모델에 따라 **OPENCR_MODEL** 이름을 `burger` 또는 `waffle`로 지정하세요.  **[TurtleBot3 SBC]** $exportOPENCR_PORT=/dev/ttyACM0$exportOPENCR_MODEL=burger$rm-rf./opencr_update.tar.bz2
4. 펌웨어와 필요한 로더를 다운로드한 후 파일을 추출하여 업로드 준비를 합니다. **[TurtleBot3 SBC]** $wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS2/latest/opencr_update.tar.bz2$tar-xvfopencr_update.tar.bz2
5. OpenCR에 펌웨어를 업로드합니다.  **[TurtleBot3 SBC]** $cd./opencr_update$./update.sh$OPENCR_PORT$OPENCR_MODEL.opencr
6. TurtleBot3 Burger의 성공적인 펌웨어 업로드 화면은 다음과 같습니다.
7. 펌웨어 업로드가 실패하면 다음 지침에 따라 복구 모드를 통해 다시 업로드하세요. 복구 모드에서는 [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/)의 `STATUS` LED가 주기적으로 깜빡입니다. PUSH SW2 버튼을 누른 상태에서 Reset 버튼을 누르고, Reset 버튼을 뗀 후 PUSH SW2 버튼을 놓으세요.


### OpenCR 테스트

> **참고**: OpenCR 테스트 중 바퀴가 움직이지 않으면 "**TurtleBot3 DYNAMIXEL 설정 방법**"을 참조하여 DYNAMIXEL 구성을 업데이트하세요.

`PUSH SW 1` 및 `PUSH SW 2` 버튼을 사용하여 로봇이 올바르게 조립되었는지 확인할 수 있습니다. 이 과정은 좌우 DYNAMIXEL 구성과 [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) 보드 펌웨어를 테스트합니다.

![](img/opencr_models.png)

1. TurtleBot3 조립 후 [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/)에 전원을 연결하고 전원 스위치를 켭니다. 빨간색 `Power LED`가 켜집니다.
2. 로봇을 넓고 평평한 바닥에 놓습니다. 테스트 시 안전 반경 1미터(40인치)를 권장합니다.
3. `PUSH SW 1`을 몇 초간 눌러 로봇이 약 30센티미터(약 12인치) 전진하도록 명령합니다.
4. `PUSH SW 2`를 몇 초간 눌러 로봇이 제자리에서 180도 회전하도록 명령합니다.


## OpenCR 설정

1. 마이크로 USB 케이블을 사용하여 [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/)을 Raspberry Pi에 연결하세요.  ![](img/opencr_setup.png)
2. [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) 펌웨어를 업로드하기 위해 Raspberry Pi에 필요한 패키지를 설치하세요.  **[TurtleBot3 SBC]** $sudodpkg--add-architecturearmhf$sudoapt-get update$sudoapt-getinstalllibc6:armhf
3. 모델에 따라 **OPENCR_MODEL** 이름을 `burger` 또는 `waffle`로 지정하세요.  **[TurtleBot3 SBC]** $exportOPENCR_PORT=/dev/ttyACM0$exportOPENCR_MODEL=burger_noetic$rm-rf./opencr_update.tar.bz2
4. 펌웨어와 필요한 로더를 다운로드한 후 파일을 추출하여 업로드 준비를 합니다. **[TurtleBot3 SBC]** $wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS1/latest/opencr_update.tar.bz2$tar-xvfopencr_update.tar.bz2
5. OpenCR에 펌웨어를 업로드합니다.  **[TurtleBot3 SBC]** $cd./opencr_update$./update.sh$OPENCR_PORT$OPENCR_MODEL.opencr
6. TurtleBot3 Burger의 성공적인 펌웨어 업로드 화면은 다음과 같습니다.
7. 펌웨어 업로드가 실패하면 다음 지침에 따라 복구 모드를 통해 다시 업로드하세요. 복구 모드에서는 [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/)의 `STATUS` LED가 주기적으로 깜빡입니다. PUSH SW2 버튼을 누른 상태에서 Reset 버튼을 누르고, Reset 버튼을 뗀 후 PUSH SW2 버튼을 놓으세요.


### OpenCR 테스트

> **참고**: OpenCR 테스트 중 바퀴가 움직이지 않으면 "**TurtleBot3 DYNAMIXEL 설정 방법**"을 참조하여 DYNAMIXEL 구성을 업데이트하세요.

`PUSH SW 1` 및 `PUSH SW 2` 버튼을 사용하여 로봇이 올바르게 조립되었는지 확인할 수 있습니다. 이 과정은 좌우 DYNAMIXEL 구성과 [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) 보드 펌웨어를 테스트합니다.

![](img/opencr_models.png)

1. TurtleBot3 조립 후 [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/)에 전원을 연결하고 전원 스위치를 켭니다. 빨간색 `Power LED`가 켜집니다.
2. 로봇을 넓고 평평한 바닥에 놓습니다. 테스트 시 안전 반경 1미터(40인치)를 권장합니다.
3. `PUSH SW 1`을 몇 초간 눌러 로봇이 약 30센티미터(약 12인치) 전진하도록 명령합니다.
4. `PUSH SW 2`를 몇 초간 눌러 로봇이 제자리에서 180도 회전하도록 명령합니다.
