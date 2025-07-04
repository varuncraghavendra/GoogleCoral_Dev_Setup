# Setting up Coral for Turtlebot3

1. Follow the instructions present [https://emanual.robotis.com/docs/en/platform/turtlebot3/opencr_setup/#opencr-setup](here) from step 3 onwards using your host system
    This is to setup the opencr board. This setup will not run on the Coral by itself because of the lack of support for the aarch64 ARM architecture
    Choose the appropriate port and burger_noetic model

2. Test the OpenCR board by pressing the pushbuttons, if there is no response then you will have to go through the instructions to setup the arduino ide on the host    computer [https://emanual.robotis.com/docs/en/software/arduino_ide/](here)
    1. Setup the dynamixel motors with the instructions that are given [https://emanual.robotis.com/docs/en/platform/turtlebot3/faq/#setup-dynamixels-for-turtlebot3](here)
    2. Do not forget to flash the turtlebot core program back
    3. You might have to run step 1 to reinstall the noetic package (however the motor configuration will be retained)

3. Install the required libraries for turtlebot3 
    ```
    sudo apt install ros-noetic-rosserial-python ros-noetic-tf
    sudo apt install ros-noetic-hls-lfcd-lds-driver
    sudo apt install ros-noetic-turtlebot3-msgs
    sudo apt install ros-noetic-dynamixel-sdk
    cd ~/catkin_ws/src
    git clone -b noetic-devel https://github.com/ROBOTIS-GIT/turtlebot3.git
    cd ~/catkin_ws/src/turtlebot3
    rm -r turtlebot3_description/ turtlebot3_teleop/ turtlebot3_navigation/ turtlebot3_slam/ turtlebot3_example/
    cd ~/catkin_ws/
    catkin_make -j1
    ```
    Edit your bashrc
    ```
    nano ~/.bashrc
    ```

    Add the following lines
    ```
    source /opt/ros/noetic/setup.bash
    export ROS_MASTER_URI=http://localhost:11311
    export ROS_HOSTNAME=localhost
    ```
    Then
    ```
    source ~/.bashrc
    ```
4. Setup the udev rules

    ```
    rosrun turtlebot3_bringup create_udev_rules
    ```

    If this fails with the error

    ```
    sudo: unable to resolve host hostname: Name or service not known
    ```


    Then run
    ```
    echo $(hostname -I | cut -d\  -f1) $(hostname) | sudo tee -a /etc/hosts
    ```