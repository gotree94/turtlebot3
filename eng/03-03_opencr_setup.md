# TurtleBot3

> **Source**: [https://emanual.robotis.com/docs/en/platform/turtlebot3/opencr_setup](https://emanual.robotis.com/docs/en/platform/turtlebot3/opencr_setup)

---


## OpenCR Setup

1. Connect the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) to the Rasbperry Pi using a micro USB cable.  ![](img/opencr_setup.png)
2. Install the required packages on the Raspberry Pi in order to upload the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) firmware.  **[TurtleBot3 SBC]** $sudodpkg--add-architecturearmhf$sudoapt-get update$sudoapt-getinstalllibc6:armhf
3. Depending on your specific model, specify either `burger` or `waffle` for the **OPENCR_MODEL** name.  **[TurtleBot3 SBC]** $exportOPENCR_PORT=/dev/ttyACM0$exportOPENCR_MODEL=burger$rm-rf./opencr_update.tar.bz2
4. Download the firmware and required loader, then extract the file to prepare for upload. **[TurtleBot3 SBC]** $wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS2/latest/opencr_update.tar.bz2$tar-xvfopencr_update.tar.bz2
5. Upload firmware to the OpenCR.  **[TurtleBot3 SBC]** $cd./opencr_update$./update.sh$OPENCR_PORT$OPENCR_MODEL.opencr
6. A successful firmware upload for the TurtleBot3 Burger will look like this:
7. If the firmware upload fails, try uploading again through recovery mode according to the following instructions. While in recovery mode, the `STATUS` led of the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) will blink periodically. Hold down thePUSH SW2button.Press theResetbutton.Release theResetbutton.Release thePUSH SW2button.


### OpenCR Test

**NOTE** : If the wheels do not move while performing the OpenCR Test, refer to the” **TurtleBot3 DYNAMIXEL setup instructions** ” to update the DYNAMIXEL’s configuration.

You can use `PUSH SW 1` and `PUSH SW 2` buttons to see whether your robot has been properly assembled. This process tests the left and right DYNAMIXEL configurations and the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) board firmware.

![](img/opencr_models.png)

1. After assembling the TurtleBot3, connect power to the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) and turn on the power switch. The red `Power LED` will be turned on.
2. Place the robot on flat ground in a wide open area. For the test, a safety radius of 1 meter (40 inches) is recommended.
3. Press and hold `PUSH SW 1` for a few seconds to command the robot to move 30 centimeters (about 12 inches) forward.
4. Press and hold `PUSH SW 2` for a few seconds to command the robot to rotate 180 degrees in place.


## OpenCR Setup

1. Connect the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) to the Rasbperry Pi using a micro USB cable.  ![](img/opencr_setup.png)
2. Install the required packages on the Raspberry Pi in order to upload the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) firmware.  **[TurtleBot3 SBC]** $sudodpkg--add-architecturearmhf$sudoapt-get update$sudoapt-getinstalllibc6:armhf
3. Depending on your specific model, specify either `burger` or `waffle` for the **OPENCR_MODEL** name.  **[TurtleBot3 SBC]** $exportOPENCR_PORT=/dev/ttyACM0$exportOPENCR_MODEL=burger$rm-rf./opencr_update.tar.bz2
4. Download the firmware and required loader, then extract the file to prepare for upload. **[TurtleBot3 SBC]** $wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS2/latest/opencr_update.tar.bz2$tar-xvfopencr_update.tar.bz2
5. Upload firmware to the OpenCR.  **[TurtleBot3 SBC]** $cd./opencr_update$./update.sh$OPENCR_PORT$OPENCR_MODEL.opencr
6. A successful firmware upload for the TurtleBot3 Burger will look like this:
7. If the firmware upload fails, try uploading again through recovery mode according to the following instructions. While in recovery mode, the `STATUS` led of the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) will blink periodically. Hold down thePUSH SW2button.Press theResetbutton.Release theResetbutton.Release thePUSH SW2button.


### OpenCR Test

**NOTE** : If the wheels do not move while performing the OpenCR Test, refer to the” **TurtleBot3 DYNAMIXEL setup instructions** ” to update the DYNAMIXEL’s configuration.

You can use `PUSH SW 1` and `PUSH SW 2` buttons to see whether your robot has been properly assembled. This process tests the left and right DYNAMIXEL configurations and the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) board firmware.

![](img/opencr_models.png)

1. After assembling the TurtleBot3, connect power to the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) and turn on the power switch. The red `Power LED` will be turned on.
2. Place the robot on flat ground in a wide open area. For the test, a safety radius of 1 meter (40 inches) is recommended.
3. Press and hold `PUSH SW 1` for a few seconds to command the robot to move 30 centimeters (about 12 inches) forward.
4. Press and hold `PUSH SW 2` for a few seconds to command the robot to rotate 180 degrees in place.


## OpenCR Setup

1. Connect the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) to the Rasbperry Pi using a micro USB cable.  ![](img/opencr_setup.png)
2. Install the required packages on the Raspberry Pi in order to upload the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) firmware.  **[TurtleBot3 SBC]** $sudodpkg--add-architecturearmhf$sudoapt-get update$sudoapt-getinstalllibc6:armhf
3. Depending on your specific model, specify either `burger` or `waffle` for the **OPENCR_MODEL** name.  **[TurtleBot3 SBC]** $exportOPENCR_PORT=/dev/ttyACM0$exportOPENCR_MODEL=burger_noetic$rm-rf./opencr_update.tar.bz2
4. Download the firmware and required loader, then extract the file to prepare for upload. **[TurtleBot3 SBC]** $wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS1/latest/opencr_update.tar.bz2$tar-xvfopencr_update.tar.bz2
5. Upload firmware to the OpenCR.  **[TurtleBot3 SBC]** $cd./opencr_update$./update.sh$OPENCR_PORT$OPENCR_MODEL.opencr
6. A successful firmware upload for the TurtleBot3 Burger will look like this:
7. If the firmware upload fails, try uploading again through recovery mode according to the following instructions. While in recovery mode, the `STATUS` led of the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) will blink periodically. Hold down thePUSH SW2button.Press theResetbutton.Release theResetbutton.Release thePUSH SW2button.


### OpenCR Test

**NOTE** : If the wheels do not move while performing the OpenCR Test, refer to the” **TurtleBot3 DYNAMIXEL setup instructions** ” to update the DYNAMIXEL’s configuration.

You can use `PUSH SW 1` and `PUSH SW 2` buttons to see whether your robot has been properly assembled. This process tests the left and right DYNAMIXEL configurations and the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) board firmware.

![](img/opencr_models.png)

1. After assembling the TurtleBot3, connect power to the [OpenCR](https://emanual.robotis.com/docs/en/parts/controller/opencr10/) and turn on the power switch. The red `Power LED` will be turned on.
2. Place the robot on flat ground in a wide open area. For the test, a safety radius of 1 meter (40 inches) is recommended.
3. Press and hold `PUSH SW 1` for a few seconds to command the robot to move 30 centimeters (about 12 inches) forward.
4. Press and hold `PUSH SW 2` for a few seconds to command the robot to rotate 180 degrees in place.
