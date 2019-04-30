*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [**Part 2**](/part2/README.md) - [Part 3](/part3/README.md) - [Resources](/additionalResources/README.md)
***
**Part 2** - [**WiFi Gateway**](WIFIGW.md) - [WiFi Bridge](WIFIBRDG.md)
***

# Part 2 - WiFi Connected Gateway

This section of the workshop will enable your gateway to connect to your home/office network via WiFi and route mesh traffic over the WiFi connection instead of using an Ethernet connection,  which was setup in part 1.

You need to have a supported USB WiFi dongle to add a second WiFi interface to the Raspberry Pi.  Some USB WiFi dongles need additional drivers to be installed before working with a Raspberry Pi - check the manufacturer website or other community support sites if you need help.

The configuration presented will use wlan0 as the mesh WiFi interface and wlan1 as the home/office WiFi interface.  

When you connect a WiFi dongle it is often assigned wlan0, then the internal WiFi becomes wlan1.  If you need the USB WiFi dongle to be the interface to connect to the home/office network then you should replace wlan0 with wlan 1 in all the previous configuration completed in part 1 and switch wlan0 and wlan1 in the following configuration.

## Setting up the network connection

To connect to a WiFi network the raspberry pi uses a configuration file /etc/wpa_supplicant/wpa_supplicant.conf.  

1. Edit /etc/wpa_supplicant/wpa_supplicant.conf as root user and change the content to be similar to the following, noting the comments below:

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

    - leave the country code as you set with the raspi-config (which should be the country you are located in)
    - set the **network name** to be the name of the network you want to join
    - set the **password** to the network password you want to join

## Updating the mech traffic routing for WiFi

The rules in file start-batman-adv.sh need to be updated to route traiffic to the wlan1 interface rather than the eth0 interface.

1. Edit file ~/start-batman-adv.sh and set the content to:

    ```text
    #!/bin/bash
    # batman-adv interface to use
    sudo batctl if add wlan0
    sudo ifconfig bat0 mtu 1468

    # Tell batman-adv this is an internet gateway
    sudo batctl gw_mode server

    # Enable port forwarding between eth0 and bat0
    sudo sysctl -w net.ipv4.ip_forward=1
    sudo iptables -t nat -A POSTROUTING -o wlan1 -j MASQUERADE
    sudo iptables -A FORWARD -i wlan1 -o bat0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
    sudo iptables -A FORWARD -i bat0 -o wlan1 -j ACCEPT

    # Activates the interfaces for batman-adv
    sudo ifconfig wlan0 up
    sudo ifconfig bat0 up # bat0 is created via the first command
    sudo ifconfig bat0 192.168.199.1/24
    ```

## Verifying the configuration

1. Shutdown the gateway raspberry pi with command ```sudo shutdown -h now```.  Wait for the pi to shutdown then remove the power cable
2. Inset the WiFi USB dongle
3. Reconnect the power to the Raspberry Pi and wait for it to boot.
4. Connect to the pi via ssh
5. Run command ```ifconfig``` and check you can see both wlan0 and wlan1 interfaces.
6. Run command ```iwconfig``` and check you see something similar to:

    ```text
    wlan0     IEEE 802.11  ESSID:"bi-raspi-mesh"  
            Mode:Ad-Hoc  Frequency:2.462 GHz  Cell: B2:7A:83:D4:C2:B9
            Tx-Power=31 dBm
            Retry short limit:7   RTS thr:off   Fragment thr:off
            Power Management:on

    lo        no wireless extensions.

    wlan1     IEEE 802.11  ESSID:"INNES"  
            Mode:Managed  Frequency:2.437 GHz  Access Point: 00:23:6C:BF:51:47
            Bit Rate=39 Mb/s   Tx-Power=31 dBm
            Retry short limit:7   RTS thr:off   Fragment thr:off
            Power Management:on
            Link Quality=44/70  Signal level=-66 dBm  
            Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
            Tx excessive retries:149  Invalid misc:0   Missed beacon:0

    bat0      no wireless extensions.

    eth0      no wireless extensions.
    ```

    noticing:

    - wlan0 is in Ad-Hoc mode and has the mesh network ESSID
    - wlan1 is in Managed mode and is connected to your home/office network ESSID (which you entered in wpa_supplicant)

7. Check the mesh configuration is still OK, as you did in part 1 with commands  ```sudo batctl if``` and ```sudo batctl n```
8. From your gateway raspberry pi, ssh to another node on the mesh (or connect your laptop to the bridged node) and on a commandline on the mesh node enter command ```ping www.ibm.com -c 5``` to verify that routing to the internet and back is working correctly.

You now have your gateway working using a WiFi connection to your home/office network.  Proceed to the [next section](WIFIBRDG.md) if you want to enable an access point to allow devices to connect to the mesh network via WiFi.  If you don't need to WiFi enable a bridge node then you can move to [part 3](/part3/README.md)

***
*Quick links :*
***
**Part 2** - [**WiFi Gateway**](WIFIGW.md) - [WiFi Bridge](WIFIBRDG.md)
***
[Home](/README.md) - [Part 1](/part1/README.md) - [**Part 2**](/part2/README.md) - [Part 3](/part3/README.md) - [Resources**](/additionalResources/README.md)