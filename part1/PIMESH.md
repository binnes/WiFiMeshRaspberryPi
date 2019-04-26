*Quick links :*
***
[Home](/README.md) - [**Part 1**](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [Resources](/additionalResources/README.md)
***
**Part 1** - [Mesh Networks](MESH.md) - [**Build a Mesh network**](PIMESH.md) - [Network access](ROUTE.md)
***

# Part 1 - Mesh Networks

In this part of the workshop you will build your own mesh network over WiFi using Raspberry Pi computers.  You need a minimum of 2 Raspberry Pi computers, but can optionally add as many Pis as you want to make a larger mesh network.

## batman-adv

To create the mesh network on the Raspberry Pi we are using [batman-adv](https://www.open-mesh.org/projects/open-mesh/wiki), which is part of the standard linux kernel.  We are going to configure the batman-adv kernel module to take control of the WiFi interface **wlan0** and create a mesh network over WiFi.  Batman-adv will then create a new interface **bat0** to allow the Pi to send network traffic over the mesh network.  This will be explored more in the section on [network access](ROUTE.md)

You need to complete the following steps on all the Raspberry Pis that you want to be part of the mesh network.  You can choose to use [headerless setup](/additionalResources/HEADERLESS_SETUP.md) or a keyboard, mouse and monitor to access the Pi command line.

## Create the SD card and perform initial setup

1. Download the latest Raspbian image from [https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/).  Choose the Raspbian Stretch Lite version.
2. Flash the image to an SD card suitable for your Raspberry Pi.  Instructions are available [here](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) if needed.
3. If you are doing a [headerless setup](/additionalResources/HEADERLESS_SETUP.md) insert the SD card back into your computer.  Open a command line or terminal on your computer and change to the directory where the SD card boot partition is mounted and create an empty file in the boot partition called ssh:
    - *Linux* : ```touch ssh```
    - *MacOS* : ```touch ssh```
    - *Windows* :  ```type NUL >> ssh```

    *optionally, create the wpa_supplicant.conf file or modify the cmdline.txt and config.txt files if you need OTG mode or WiFi connectivity*

    Eject the SD card from your operating system and remove the card from your computer.
4. Insert the SD card into the Raspberry Pi and then power on the Raspberry Pi.
5. Login to the pi with user pi and password raspberry.  If using headerless setup then connect via ssh.  The hostname on first boot is raspberrypi.local.  
7. On the Raspberry Pi command line issue the command ```sudo raspi-config``` and then go through and change the following settings:
    - Change the user password (don't forget it, as you will need it everytime you remotely connect to the Pi)
    - Network Options - Hostname
    - Network Option - WiFi.  If your pi is not connected to the internet already, use this option to setup WiFi connectivity to ensure your Pi has access to the internet.
    - Boot Options - Desktop/CLI - choose Console Autologin
    - Localisation Options - set Locale, Timezone and WiFi country to match your location

    Exit raspi-config, don't reboot yet.
8. Issue command ```sudo apt-get update && sudo apt-get upgrade -y```
9. Reboot the Raspberry Pi with command ```sudo reboot -n```

## Setup batman-adv

Once the Pi has rebooted, get to the command line (remember your pi now has a new hostname and the pi user has a new password that you set in the previous section).  Perform the following on the pi command line:

1. To manage the mesh network, a utility called **batctl** needs to be installed.  This can be done using command ```sudo apt-get install -y batctl```
2. Using your preferred editor create a file ~/start-batman-adv.sh

    e.g.
    - ```vi ~/start-batman-adv.sh```
    - ```nano ~/start-batman-adv.sh```

    the file should contain the following:

    ```bash
    #!/bin/bash
    # Tell batman-adv which interface to use
    sudo batctl if add wlan0

    # Activates the interfaces for batman-adv
    sudo ifconfig wlan0 up
    sudo ifconfig bat0 up
    ```

3. Make the start-batman-adv.sh file executable with command ```chmod +x ~/start-batman-adv.sh```
***
*Quick links :*
***
**Part 1** - [Mesh Networks](MESH.md) - [**Build a Mesh network**](PIMESH.md) - [Network access](ROUTE.md)
***
[Home](/README.md) - [**Part 1**](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [Resources**](/additionalResources/README.md)