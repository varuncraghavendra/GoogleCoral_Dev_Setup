# Setup google coral for the first time: 

https://coral.ai/docs/dev-board/get-started/

On a Windows PC, install Balena Etcher : https://www.balena.io/etcher/
Connect a 32GB/64GB/128GB SD card with a card reader. Format it and flash the following image on it : 
https://dl.google.com/coral/mendel/enterprise/enterprise-eagle-flashcard-20211117215217.zip

Mount the flashed SD card on the Coral Dev board and set the DIP switch to the following configuration

Make sure the SoM is on the Google Coral Development Board
Power it up using a 5V 3A / 5V 4A Type C wall socket adapter [Note, do not power it from Laptop USB, it will cause damage to your laptop]. 
Connect it to the PWR port on the Coral Dev Board. 
At this point you can see the fan spinning and the red light turned on, it will take around 20 to 25 minutes to copy the image onto the eMMC memory.
After that , you should observe the red light off. Disconnect the power and reset the DIP switches to the following configuration. 

# First Time Bootup

1. Turn on google coral by powering it with a 5v 3A adaptor(USB C type cable). Connect the cable to the PWR port. 
   Wait for approx 30 seconds for the coral to boot up.
2. For the first time WIFI setup, connect the coral dev board to a HDMI monitor with a keyboard and mouse, enter the terminal and type "nmtui".
   Activate a WIFI network of your choice.
3. On the terminal, type "sudo ifconfig" and note down the IP address of WLAN0.
4. Set permissions to access the system via SSH. Follow these instructions.
   
   On the terminal, type "sudo nano /etc/ssh/sshd_config" and reset the following permissions.
   
   ChallengeResponseAuthentication yes
   PasswordAuthentication yes
   
   Ctrl+O to save the changes and Ctrl+X to come out of the nano shell.
   
5. Reboot the system by running "sudo reboot now" and for 20 seconds for it to boot up.
6. At this point you do not need a keyboard, mouse and monitor.
7. Select your host computer and connect to the same network as of coral.
   NOTE: Default Password is "mendel" for both windows and LINUX
8. If your host computer is windows, you can get root access to coral using PuTTY. 
   Install PuTTY from this link
   https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
   Open PuTTY once it's installed and select SSH, enter the IP address and open the shell.
9. If your host computer is LINUX, type the following in the terminal " ssh mendel@IP address".


# Running Camera Examples:
1. Connect a camera to the USB 3.0 port.

Supported cameras:
Logitech C270 (Install driver seperately- instructions available in Onboard AI folder in this repo)
Logitech C930e 

2. Check if the camera is connected and is being detected by running the following: 

  v4l2-ctl --list-devices

You should see the name of the device under /video1.
