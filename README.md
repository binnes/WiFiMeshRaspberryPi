*Quick links :*
[**Home**](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [Resources](/additionalResources/README.md)
***

# Create a Mesh Network over WiFi using Raspberry Pi

Welcome to this workshop, where you will create a mesh network over WiFI using Raspberry Pis.  After creating the mesh you will use it to extend an existing network, then use the mesh to connect sensors to the Internet and allow other devices (not participating in the mesh) to connect to the Internet via the mesh network.

## Expected outcomes

After completing this workshop you should be able to :

- understand how mesh networks work at a high level
- create a mesh network using Raspberry Pis
- connect the mesh to an existing network, understanding the difference between routing and bridging network traffic
- enable non-mesh devices to use the mesh to connect to the Internet
- create a sensor application on a mesh node which securely communicates with a cloud application

## Prerequisites

To complete this workshop you need some hardware and some prerequisite knowledge.  Mode details about the prerequisites is available in the [Additional Resources section](/additionalResources/README.md) :

- A minimum of 2 raspberry Pis (3 Model B(+)) additional Pis can be added to extend the mesh, which can also include Pi Zero W.  Power supply for Raspberry Pis and optionally keyboard, mouse and monitor if not doing headerless setup (details in [Additional Resources](/additionalResources/README.md))
- A laptop or desktop computer with a modern OS (Linux, MacOS or Windows - windows users will need additional software to communicate with Raspberry Pis if using [headerless setup](/additionalResources/HEADERLESS_SETUP.md))
- Ability to flash the SD card for the raspberry Pi (SD card slot in laptop or USB adapter and appropriate software)
- Ethernet network, cables and connection to connect Laptop to Ethernet (if this isn't available then WiFi is also possible)
- Internet connectivity is required
- Optional WiFi USB dongle(s) if want to enable WiFi rather than Ethernet connectivity
- DHT11 or DHT22 temperature and humidity sensor and an addressable RGB LED.  Additional details about of these components are available in the [Additional Resources section](/additionalResources/README.md)
- You should be able to setup a Raspberry Pi and get access to a command line on the Pi (help available in the [Additional Resouces section](/additionalResources/README.md))
- An IBM public Cloud account (the free, lite account is OK, if you have resources avaialble in the account to deploy an application)

## Outline

***
*Quick links :*
[**Home**](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [Resources](/additionalResources/README.md)