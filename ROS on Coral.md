# Installation

Connect to the internet using nmtui

If the time shown is incorrect
```
sudo date -s "$(wget --method=HEAD -qSO- --max-redirect=0 google.com 2>&1 | grep Date: | cut -d' ' -f4-10)"
```

If you get the error `The following signatures couldn't be verified because the public key is not available: NO_PUBKEY`

Obtain the keys using
```
sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
If dirmngr is not installed
```
sudo apt-get install dirmngr
```

Obtain the latest keys for Buster
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu buster main" > /etc/apt/sources.list.d/ros-latest.list'
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```
Check for updates
```
sudo apt update
sudo apt upgrade
```

If errors occur in the above step
```
sudo apt --fix-broken install
```

If there is a key error use
```
wget -O- https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo tee /etc/apt/trusted.gpg.d/coral-edgetpu.gpg
```

Install the following tools required for building
```
sudo apt-get install -y python-rosdep python-rosinstall-generator python-wstool python-rosinstall build-essential cmake
sudo apt-get install libboost-all-dev
```

Initialize the dependencies
```
sudo rosdep init
```
Fool rospkg to think you are using Debian 10
```
sudo vim /usr/lib/python2.7/dist-packages/rospkg/os_detect.py
```

For python3.7 systems
```
sudo vim /usr/local/lib/python3.7/dist-packages/rospkg/os_detect.py
```

change lines 543

```
    def __init__(self, os_list=None):
        if os_list is None:
            os_list = OsDetect.default_os_list
        self._os_list = os_list
        self._os_name = "debian"
        self._os_version = "10.0"
        self._os_codename = "buster"
        self._os_detector = None
        self._override = False
```
If you want to build from source you can skip to [Build From Source](#build-from-source)

To install from binaries simply do:
```
sudo apt install ros-noetic-desktop
```

### Build From Source
Install the package list for ROS Noetic

```
sudo pip3 install -U rosdep rosinstall_generator vcstool
rosinstall_generator desktop --rosdistro noetic --deps --tar > noetic-desktop.rosinstall
mkdir ./src
vcs import --input noetic-desktop.rosinstall ./src
rosdep install --from-paths ./src --ignore-packages-from-source --rosdistro noetic -y
```

Allocate some swap space to complete the build
```
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

Start the build (and leave it overnight)

```
./src/catkin/bin/catkin_make_isolated -j2 --install -DCMAKE_BUILD_TYPE=Release -DPYTHON_EXECUTABLE=/usr/bin/python3
```

If you run out of memory try

```
./src/catkin/bin/catkin_make_isolated -j1 --install -DCMAKE_BUILD_TYPE=Release -DPYTHON_EXECUTABLE=/usr/bin/python3
```

Setup ROS to run with 

```
source devel_isolated/setup.bash
```
