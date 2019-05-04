*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [**Part 2**](/part2/README.md) - [Part 3](/part3/README.md) - [Resources](/additionalResources/README.md)
***
**Part 2** - [WiFi Gateway](WIFIGW.md) - [**WiFi Bridge**](WIFIBRDG.md)
***

# Part 2 - WiFi Access Point bridged to mesh

In this part you will convert the bridge node into an access point and enable the WiFi traffic coming over the Access Point to access the mesh network by bridging the Access Point interface into the configured bridge interface br0.

Note the configuration below leaves interface eth0 in the configuration, so you can use both the Ethernet connection and WiFi connection to bridge onto the mesh network.

## Configuring the Access point and bridging to br0 interface

1. Install additional software to run the access point using command ```sudo apt-get install -y hostapd```
2. Edit file **/etc/hostapd/wlan1.conf** as root user and set the content to:, you need to create this file, and set the user and set the content to: as:

    ```text
    interface=wlan1
    bridge=br0
    hw_mode=g
    channel=7
    wmm_enabled=0
    macaddr_acl=0
    auth_algs=1
    ignore_broadcast_ssid=0
    wpa=2
    wpa_key_mgmt=WPA-PSK
    wpa_pairwise=TKIP
    rsn_pairwise=CCMP
    ssid=raspimesh
    wpa_passphrase=passw0rd
    ```

    You can change the SSID and the passphrase to something you'd prefer, as these configure the accesspoint your devices will connect to.

    The above configuration sets up a 2.4GHz network on channel 7, again, you can change all the details to match your WiFi dongle and preferred network.  You can find details of all possible options for the config file [here](https://w1.fi/cgit/hostap/plain/hostapd/hostapd.conf)

3. Edit file **/etc/default/hostapd** as root user and set the content to:, which is used by the startup script for the Access Point service, so it looks like :

    ```text
    # Defaults for hostapd initscript
    #
    # See /usr/share/doc/hostapd/README.Debian for information about alternative
    # methods of managing hostapd.
    #
    # Uncomment and set DAEMON_CONF to the absolute path of a hostapd configuration
    # file and hostapd will be started during system boot. An example configuration
    # file can be found at /usr/share/doc/hostapd/examples/hostapd.conf.gz
    #
    DAEMON_CONF=""

    # Additional daemon options to be appended to hostapd command:-
    #   -d   show more debug messages (-dd for even more)
    #   -K   include key data in debug messages
    #   -t   include timestamps in some debug messages
    #
    # Note that -B (daemon mode) and -P (pidfile) options are automatically
    # configured by the init.d script and must not be added to DAEMON_OPTS.
    #
    DAEMON_OPTS="-B"
    ```

    notice the DAEMON_OPTS option.  This should not need to be set, but I noticed in the latest version of Raspbian lite, the -B options is not set in the startup script, which causes the service to timeout and be restarted, which resets all WiFi connections to the Access Point.
4. Modify the last line of file **/etc/dhcpcd.conf** as root user and add wlan1 to the list of denied interfaces:

    ```text
    denyinterfaces wlan0 eth0 bat0 wlan1
    ```

5. Update the **~/start-batman-adv.sh** file to contain the following:

    ```text
    #!/bin/bash

    # Tell batman-adv which interface to use
    sudo batctl if add wlan0
    sudo ifconfig bat0 mtu 1468

    sudo brctl addbr br0
    sudo brctl addif br0 bat0 eth0

    # Tell batman-adv this is a gateway client
    sudo batctl gw_mode client

    # Activates the interfaces for batman-adv
    sudo ifconfig wlan0 up
    sudo ifconfig bat0 up

    # Restart DHCP now bridge and mesh network are up
    sudo dhclient -r br0
    sudo dhclient br0
    ```

6. Enable the access point service on wlan1 interface using command ```sudo systemctl enable hostapd@wlan1.service```
7. Reboot the pi using command ```sudo reboot -n```

When the Pi reboots you should be able to see a new WiFi network available and devices should be able to join it, using the passphrase you set in the /etc/hostapd/wlan1.conf file.

## Verifying the setup

You can do a number of steps to verify that the configuration has been successfully created and applied.

1. Run command ```ifconfig``` top check all the interfaces are up and running OK.  You should see output similar to :

    ```text
    bat0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1468
            inet6 fe80::4816:e8ff:fe16:a3ba  prefixlen 64  scopeid 0x20<link>
            ether 4a:16:e8:16:a3:ba  txqueuelen 1000  (Ethernet)
            RX packets 1481  bytes 462785 (451.9 KiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 1632  bytes 270351 (264.0 KiB)
            TX errors 0  dropped 3 overruns 0  carrier 0  collisions 0

    br0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1468
            inet 192.168.199.53  netmask 255.255.255.0  broadcast 192.168.199.255
            inet6 fe80::11ac:9f5e:3034:e59d  prefixlen 64  scopeid 0x20<link>
            ether 4a:16:e8:16:a3:ba  txqueuelen 1000  (Ethernet)
            RX packets 360  bytes 42545 (41.5 KiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 281  bytes 51671 (50.4 KiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

    eth0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
            ether b8:27:eb:e8:18:b0  txqueuelen 1000  (Ethernet)
            RX packets 0  bytes 0 (0.0 B)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 0  bytes 0 (0.0 B)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

    lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
            inet 127.0.0.1  netmask 255.0.0.0
            inet6 ::1  prefixlen 128  scopeid 0x10<host>
            loop  txqueuelen 1000  (Local Loopback)
            RX packets 0  bytes 0 (0.0 B)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 0  bytes 0 (0.0 B)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

    wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet6 fe80::ba27:ebff:febd:4de5  prefixlen 64  scopeid 0x20<link>
            ether b8:27:eb:bd:4d:e5  txqueuelen 1000  (Ethernet)
            RX packets 3674  bytes 758584 (740.8 KiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 2652  bytes 500450 (488.7 KiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

    wlan1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet6 fe80::d67b:b0ff:fe0e:800e  prefixlen 64  scopeid 0x20<link>
            ether d4:7b:b0:0e:80:0e  txqueuelen 1000  (Ethernet)
            RX packets 1798  bytes 312112 (304.7 KiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 1728  bytes 506350 (494.4 KiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
            ```

    Notice:
    - you can see both wlan0 and wlan1 and both are running
    - br0 is the only interface with a 192.168.199.x address

2. Command ```sudo brctl show``` show show bat0, wlan1 and eth0 as bridged interfaces to br0:

    ```text
    bridge name bridge id       STP enabled interfaces
    br0     8000.4a16e816a3ba   no      bat0
                                        eth0
                                        wlan1
    ```

3. Command ```iw wlan1 info``` show the details of the access point running on wlan1:

    ```text
    Interface wlan1
        ifindex 5
        wdev 0x100000001
        addr d4:7b:b0:0e:80:0e
        ssid raspimesh
        type AP
        wiphy 1
        channel 7 (2442 MHz), width: 20 MHz, center1: 2442 MHz
        txpower 31.00 dBm
    ```

You can also run the commands ```sudo batctl if``` and ```sudo batctl n``` to verify the mesh configuration is still OK.

***
*Quick links :*
***
**Part 2** - [WiFi Gateway](WIFIGW.md) - [**WiFi Bridge**](WIFIBRDG.md)
***
[Home](/README.md) - [Part 1](/part1/README.md) - [**Part 2**](/part2/README.md) - [Part 3](/part3/README.md) - [Resources**](/additionalResources/README.md)