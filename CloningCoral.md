
# Creating Image
Following are instructions on how to clone one Google Coral Board onto another, if you would like to clone an image with ROS on it you can skip directly to [Cloning Image](#cloning-image)

After installing certain libraries and creating files on your Coral you might find it helpful to create a duplicate system image either as a backup or to implement on another coral board. Cloning an image will save time especially while compiling a large package like ROS. (Compilation needs to be done overnight whereas Cloning takes about 5 minutes)

In order to clone we will be using the eMMC memory on board the Coral Board directly, for this you will need to do the following

1. Connect the USB OTG cable **and** the USB Micro cable. Do not power the board yet.

2. You can verify if the USB has been attached by typing the command `dmesg | grep ttyUSB`. You should have an output like:
    ```
    [ 6437.706335] usb 2-13.1: cp210x converter now attached to ttyUSB0
    [ 6437.708049] usb 2-13.1: cp210x converter now attached to ttyUSB1
    ```

3. Access the uboot terminal by creating a screen session on your system using:
    ```
    screen /dev/ttyUSB0 115200
    ```
    Note: Since the board is not powered on yet, you should not be able to see anything on the screen

4. Power on the board and immediately press any key into your screen session, this should cancel the autoboot timeout and allow you into the uboot menu

5. To allow the eMMC memory to be read you need to type the command
    ```
    ums 0 mmc 0
    ```

6. If everything has gone correctly, you should be able to see new storage devices with different amounts of storage 

7. Use the command `sudo fdisk -l` on your host system to find where your coral device is stored

8. **Danger Zone** Cloning the disk using the `dd` command. Be very careful when using this command as it can rewrite your data without warning
    ```
    sudo dd if=/dev/path-to-dev-board of=./backup.img bs=4M status=progress
    ```

9. Power off the board by unplugging

# Cloning Image

Note: You may need to have booted into a Mendel Image first before you clone the image given below, otherwise it might not boot up

Download the `.img` file from [here](https://indianinstituteofscience-my.sharepoint.com/:f:/g/personal/namanmenezes_iisc_ac_in/EmnUBM1sd6lDl0HnvgEIp8YBJCmO6WpTRPq7-vN05K40Hg?e=98L3Am) or use the file generated in the first section

1. Connect the USB OTG cable **and** the USB Micro cable. Do not power the board yet. Ensure the board is in boot from eMMC mode (DIP Switches in ON OFF OFF OFF)

2. You can verify if the USB has been attached by typing the command `dmesg | grep ttyUSB`. You should have an output like:
    ```
    [ 6437.706335] usb 2-13.1: cp210x converter now attached to ttyUSB0
    [ 6437.708049] usb 2-13.1: cp210x converter now attached to ttyUSB1
    ```

3. Access the uboot terminal by creating a screen session on your system using:
    ```
    screen /dev/ttyUSB0 115200
    ```
    Note: Since the board is not powered on yet, you should not be able to see anything on the screen

4. Power on the board and immediately press any key into your screen session, this should cancel the autoboot timeout and allow you into the uboot menu

5. To allow the eMMC memory to be read you need to type the command
    ```
    ums 0 mmc 0
    ```

6. If everything has gone correctly, you should be able to see new storage devices with different amounts of storage 

7. Use the command `sudo fdisk -l` on your host system to find where your coral device is stored

8. **Danger Zone** Cloning the disk using the `dd` command. Be very careful when using this command as it can rewrite your data without warning
    ```
    sudo dd if=./backup.img of=/dev/path-to-dev-board bs=4M status=progress 
    ```
