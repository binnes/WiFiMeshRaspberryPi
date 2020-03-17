*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [**Resources**](/additionalResources/README.md)
***
**Resources** - [**Setup**](PREREQUISITES_AND_SETUP.md) - [IoT Starter App](IOT_STARTER_APP.md) - [Headerless Setup](HEADERLESS_SETUP.md) - [Command line access](COMMAND_LINE_ACCESS.md)
***

# Additional Resources - Prerequisites and setup

The [home page](/README.md) set out the prerequisites for this workshop.  This section will add more details about the prerequisites.

## Hardware needed

### Raspberry Pi boards

To complete the workshop you need to have at least 2 Raspberry Pi 3 or 4 Model B(+) boards.  You can extend the mesh and add additional Raspberry Pi boards, so long as they can participate in the WiFi mesh (details below)

For each Raspberry Pi you want to use in the mesh you need:

- Supported Raspberry Pi (see below)
- Micro USB power cable
- Power supply
- microSD card (8MB or larger)
- Optional USB hub for Pi Zero/Pi 1A+, if setting up with keyboard and mouse (It is possible to setup without keyboard, mouse and monitor using [headerless setup](/additionalResources/HEADERLESS_SETUP.md))
- Optional mini-HDMI to HDMI converter for Pi Zero is setting up with Keyboard, mouse and monitor
- Optional USB WiFi dongle for Pi Zero/Pi 1 Model A+ or the Pi 3 or 4 B(+) Gateway/Bridge nodes if you want to do [part 2](/part2/README.md) to add WiFi connectivity instead of Ethernet.

In addition to the Raspberry Pis, you need:

- 2 x Ethernet cables (this assumes Ethernet connectivity is available to your home/office network and your laptop has an Ethernet port or you have the appropriate adapter to add Ethernet connectivity)
- An USB SD reader/writer if your laptop doesn't have this built in

![mesh diagram](/images/PiMesh.png)

The following Raspberry Pi boards have been tested (using [Raspbian Buster Lite](https://www.raspberrypi.org/downloads/raspbian/) dated 26th September 2019) for the following roles in the Mesh network:

- Raspberry Pi 3 Model B : Gateway Node, Bridge Node or Mesh Node (additional USB WiFi dongle needed for WiFi connected Gateway or Bridge Node)
- Raspberry Pi 3 Model B+ : Gateway Node, Bridge Node or Mesh Node (additional USB WiFi dongle needed for WiFi connected Gateway or Bridge Node)
- Raspberry Pi 4 Model B : Gateway Node, Bridge Node or Mesh Node (additional USB WiFi dongle needed for WiFi connected Gateway or Bridge Node)
- Raspberry Pi Zero W : Mesh Node
- Raspberry Pi Zero (With micro USB->USB_A converter cable and USB WiFi dongle): Mesh Node
- Raspberry Pi 1 Model A+ (with USB WiFi Dongle) : Mesh Node

If using a USB WiFi dongle to act as the Mesh interface, then you can verify that the WiFi dongle supports mesh networking.  All Raspberry Pis integrated WiFi supports mesh, but if you are using an additional USB WiFi dongle then connect the WiFi dongle to your pi, log on to the Pi and run command ```iw list```.  You will see all the WiFi interfaces with all the capabilities.  For Mesh network support you are looking for the **Supported interface modes** section to verify **IBSS** mode is supported:

    ```text
    Supported interface modes:
             * IBSS
             * managed
             * AP
             * P2P-client
             * P2P-GO
             * P2P-device
    ```

**Note** : Please make sure your Raspberry Pi boards have sufficient power.  Not all USB ports on laptops can supply sufficient power for a Raspberry Pi running sensors and WiFi.

### Sensors

For [part 3](/part3/README.md) you need to have the following components:

- DHT11 or DHT22 temperature and humidity sensor
- addressable RGB LED (often called a Neopixel)
- 6 x F2F connector cables (often called dupont cable)

When purchasing these you may find the following helpful:

For the addressable RGB LED, they are often sold in packs of 5.  Useful search terms are:

- Neopixel 8mm or 5mm
- ws2812b or sk6812
- chainable or addressable RGB LED

You want the 4-pin version, so it is easy to connect to the Raspberry Pi without needing to solder.  An example of what you might want to buy is [this from Adafruit](https://www.adafruit.com/product/1734)

**Warning** Do not buy a LED if the description includes terms **common anode** or **common cathode** as these are not addressable LEDs and will not work with the instructions provided.  Additional hardware components are needed to drive common anode/cathode RGB LEDs.

The temperature and humidity sensor comes in 2 different versions.  The DHT11 and DHT22.  The DHT22 is more expensive, but is more accurate than the DHT11.  You can choose which ever version you would prefer. An example of what you may want to buy is [either of these from Adafruit](https://learn.adafruit.com/dht)

For the connector cables, you need 6 female-female connectors, though they often come in packs of 20 or 40.  The following terms may help your search:

- F2F or F-F
- dupont cables
- jumper wires

An example of what you may want to buy is [this from Adafruit](https://www.adafruit.com/product/1951)

I've provided links to Adafruit to show examples of what you need to purchase.  You may choose to purchase from the links provided, but there are many outlets across the globe.  Using e-bay, Amazon or a local supplier may provide your best option

## IBM Cloud account

To complete [part 3](/part3/README.md) you need to have an IBM Cloud account.  There is a lite account options available, which doesn't require you to enter a credit card and is totally free to use, but has certain limitations.  If you decide to upgrade and add a credit card to your cloud account then you get additional resources free of charge each month before any charges will apply, but you are responsible for managing your usage to stay within the free usage limits.  

To sign up for the IBM cloud follow [this link](https://cloud.ibm.com/login)

You also need to deploy an Internet of Things starter application.  Follow [these instructions](IOT_STARTER_APP.md) to deploy the application into your account.

***
*Quick links :*
***
**Resources** - [**Setup**](PREREQUISITES_AND_SETUP.md) - [IoT Starter App](IOT_STARTER_APP.md) - [Headerless Setup](HEADERLESS_SETUP.md) - [Command line access](COMMAND_LINE_ACCESS.md)
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [**Resources**](/additionalResources/README.md)
