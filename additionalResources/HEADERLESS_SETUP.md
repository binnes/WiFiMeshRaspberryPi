*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [**Resources**](/additionalResources/README.md)
***
**Resources** - [Setup](PREREQUISITES_AND_SETUP.md) - [**Headerless Setup**](HEADERLESS_SETUP.md) - [Command line access](COMMAND_LINE_ACCESS.md)
***

# Additional Resources - Headerless setup

When you setup a Raspberry Pi, most documentation requires you to have a USB Keyboard, mouse and an HDMI monitor.  You then have access to the Raspberry Pi, using it like a normal computer.  However, this is not always convenient and it is possible to setup a Raspberry Pi just using your laptop.

# Updating the SD Card image

Once you have flashed the Raspberry Pi image to a micro-SD card you need to reinsert it to your laptop.  You will find a partition on the SD card called boot, which you need to add files to or modify existing files to enable a headerless setup.

Note: *if you are prompted that a partition is not readable on your laptop - do **not** initialise the partition as this is the main Linux filesystem that your Raspberry Pi uses.  Windows and MacOS systems cannot read this filesystem without getting additional software installed*

Open a command line on your laptop and complete the following:

- Create an empty file called ssh in the boot partition of the SD card:
  - *Linux* : ```touch ssh```
  - *MacOS* : ```touch ssh```
  - *Windows command prompt* :  ```type NUL >> ssh```
  - *Windows PowerShell* : ```echo $null >> ssh```

If your laptop has Ethernet capability and you are working with a Raspberry Pi 3 or 4 Model B(+) you don't need to do anything else to the SD card, you can eject it from your laptop and insert it into the Raspberry Pi.  Connect a standard Ethernet cable between your laptop and the Raspberry Pi, then power on the Raspberry Pi.

If you don't have Ethernet connectivity options then you need to enable WiFi on the Raspberry Pi.  To do this you need to add another file to the boot partition of the SD card called **wpa_supplicant.conf** and it should have the following content:

    ```text
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1
    country=GB
    network={
        ssid="network name"
        scan_ssid=1
        psk="password"
    }
    ```

    You should replace **network name** with the WiFi network name you want the Pi to connect to on boot and **password** with the WiFi network network password.

Eject the SD card from your laptop and insert it into the Raspberry Pi.  If you are using a USB WiFi dongle on a Pi Zero or Pi 1 Model A+ then insert the dongle and power on the Raspberry Pi

You can now access your Raspberry Pi [using the command line](COMMAND_LINE_ACCESS.md)

For completeness, there is one further option for Pi Zero boards, which can help when WiFi setup doesn't work.  It is possible to setup a network connection over the USB port, which can also power the board for the setup process (the USB port is the port nearest the centre of the board).

To enable network over USB you need to modify 2 files in the boot partition of the SD card:

- Add a new line to the bottom of **config.txt** containing ```dtoverlay=dwc2```
- Edit **cmdline.txt** and remove the resize filesystem option and replace it with ```modules-load=dwc2,g_ether```.  Ensure that the cmdline.txt file only has a single line and the content is similar to :

    ```plain
    dwc_otg.lpm_enable=0 console=serial0,115200 console=tty1 root=PARTUUID=c1dc39e5-02 rootfstype=ext4 elevator=deadline fsck.repair=yes rootwait modules-load=dwc2,g_ether
    ```

You can now eject the SD card from your laptop, insert it into the Raspberry Pi Zero(W) and then connect the USB cable to the USB port and your laptop to power the Raspberry Pi from your laptop. After a short delay you should be able to access your Raspberry Pi using the [command line](COMMAND_LINE_ACCESS.md)

One further action needs to be taken during the initial setup.  When you run the **raspi-config** command, you need to go into the Advanced options and select to expand the filesystem.  This normally happens automatically on first boot, but we removed that option to get the network over USB functionality working.

***
*Quick links :*
***
**Resources** - [Setup](PREREQUISITES_AND_SETUP.md) - [**Headerless Setup**](HEADERLESS_SETUP.md) - [Command line access](COMMAND_LINE_ACCESS.md)
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [**Resources**](/additionalResources/README.md)
