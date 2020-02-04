*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [**Part 3**](/part3/README.md) - [Resources](/additionalResources/README.md)
***

# Part 3

## Create an IoT app that sends sensor data over the mesh network

In this Part 3 you will build an Internet of Things application that gathers sensor data and uses the mesh network to send the sensor data to the IBM Cloud. An IBM Cloud app receives and processes the sensor data and sends a command back to a Raspberry Pi to set the color of the RGB LED light attached to the Pi.

In this step, you connect the sensors, install Node-RED and some Node-RED packages, create the IoT app in IBM Cloud, and send the sensor data across the mesh to Watson IoT Platform.

## Learning Objectives

* Connect Sensors
* Install Node-RED and Node-RED packages
* Test the Sensors and LED
* Create an Internet of Things Starter Application in IBM Cloud
* Send sensor data across the mesh to the Watson IoT Platform

[This section](SENSORS.md) will install Node-RED on the raspberry Pis, install the drivers and nodes to drive the sensor and LED then connect the sensor and LED to the Raspberry Pi.  Finally Node-RED flows will be written to validate the sensors and LED are working as expected.

[This section](IOT_PLATFORM.md) will send the DHT sensor data to the IoT platform, where a Node-RED application running in the cloud will receive the data and determine the colour the LED should display based on the temperature.  A command is sent via the IoT platform to the Raspberry Pi with the LED connected to change the LED colour.

***
*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [**Part 3**](/part3/README.md) - [Resources](/additionalResources/README.md)

