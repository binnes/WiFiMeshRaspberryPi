*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [**Part 3**](/part3/README.md) - [Resources](/additionalResources/README.md)
***

# Part 3

## Learning Objectives

- Connect Sensors
- Install Node-RED and Node-RED packages
- Create an Internet of Things Starter Application in IBM Cloud
- Send sensor data across the mesh to the Watson IoT Platform

## Prerequisites

This tutorial can be completed using an IBM Cloud Lite account.

- Create an [IBM Cloud account](https://cloud.ibm.com/registration)
- Log into [IBM Cloud](https://cloud.ibm.com/login)

## Connect DHT Temperature Sensor and NeoPixel LED to Raspberry Pi

### Step 1 - Connect the DHT sensor to your Raspberry Pi

Disconnect the Raspberry Pi from the power supply before connecting the DHT sensor.

The DHT sensors have 4 connecting pins.  When looking at the front of the sensor (mesh case) with the pins at the bottom, the connections are (left to right):

- +'ve voltage
- Data
- Not used
- Ground

If you have a DHT mounted on a module then you need to check the pinout, usually indicated on the board, with **+** (to 3V pin), **-** (to G pin) and **out** or **data** :

![DHT module](/images/DHT11-pins.jpg)

### Connect the DHT-11

Connect three female to female jumper wires to the DHT-11 and then follow the [pinout diagram](https://pinout.xyz/#) to connect the jumpers to the Raspberry Pi:

- Ground - Pin 9
- Data - Pin 15
- 3.3v Power - Pin 17

### Step 2 - Connecting the Neopixel to the Raspberry Pi

Now you need to connect the NeoPixel to the Raspberry Pi.  Before you start making any connections please disconnect the device from your power supply so there is no power getting to the device.  You should never make any connection changes when the device is powered on.

Before making the connections we need to identify the 4 connecting pins coming out of the LED.  If you examine the rim of the pixel cover you will see that one side is flattened (this should be the opposite side from the shortest pin) - this pin next to the flattened side is the **Data Out** pin.  We will not be using this pin, as we only have a single pixel.  You can chain pixels together connecting the **Data Out** pin to the **Data In** pin of the next pixel in the chain.

The shortest pin on the Pixel is the **Data In**
The longest pin on the Pixel is **Ground**
The remaining pin is **+'ve voltage**, which should be 5v, but it works with 3.3v that the Raspberry Pi provides.

So, with the shortest pin on the left and the flat side on the right the pinout is (left to right):

- Data In (shortest pin)
- +'ve Voltage
- Gnd (longest pin)
- Data Out (no connection)

You need to connect the Data In, +'ve voltage and ground to the Raspberry Pi as shown in the diagram.  Take care to ensure that the connections are as shown, as connecting the wrong pins can damage the LED:

![ModeMCU LED Wiring](../images/NodeMCU_LED_Wiring.jpg)

### Connect the NeoPixel

Connect three female to female jumper wires to the NeoPixel and then follow the [pinout diagram](https://pinout.xyz/#) to connect the jumpers to :

- Ground - Pin 6
- Data - Pin 12
- 3.3v Power - Pin 1

## Install Node-RED and Node-RED packages

### Install Node.JS v10 on your Raspberry Pi

Follow the [node install instructions](https://github.com/nodesource/distributions)

Note: *The DHT sensor install does not yet work with Node.JS v12*

```bash
sudo bash
curl -sL https://deb.nodesource.com/setup_10.x | bash -
exit
```

Next, install the nodejs package and some prerequisets

```bash
sudo apt-get install -y nodejs
sudo npm -g install node-pre-gyp
sudo npm -g install node-gyp
```

### Install Node-RED and required packages

```bash
sudo npm -g install node-red --unsafe-perms
sudo npm -g install  node-red-contrib-ibm-watson-iot
```

Next install node-red-node-pi-neopixel prerequisites described in [https://flows.nodered.org/node/node-red-node-pi-neopixel](https://flows.nodered.org/node/node-red-node-pi-neopixel)

- Select to continue (y), but don't perform a full install (N) and don't let it configure sound (N)

```bash
curl -sS get.pimoroni.com/unicornhat | bash
```

then

```bash
sudo npm -g install node-red-node-pi-neopixel
```

#### DHT Sensor install

Follow the BMC2835 install instructions at [http://www.airspayce.com/mikem/bcm2835/index.html](http://www.airspayce.com/mikem/bcm2835/index.html)

```bash
wget http://www.airspayce.com/mikem/bcm2835/bcm2835-1.58.tar.gz
tar zxvf bcm2835-1.58.tar.gz
cd bcm2835-1.58
./configure
make
sudo make check
sudo make install
```

Finally install node-red-contrib-dht-sensor

```bash
sudo npm install --unsafe-perm -g node-red-contrib-dht-sensor
```

### Reboot

```bash
sudo reboot -n
```

### Start Node-RED

```bash
node-red
```

Open a browser on your laptop to the IP Address of the Raspberry Pi mesh device. Node-RED is running on Port 1880

```plain
http://192.168.1.35:1880
```

### Test the DHT-11 Temperature Sensor by building this flow

Import this flow : ![Node-RED DHT11 Flow](/images/Node-RED-DHT11-flow.png)

### Test the NeoPixel LED by building this flow

Import this flow : ![Node-RED NeoPixel Flow](/images/Node-RED-NeoPixel-flow.png)

***
*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [**Part 3**](/part3/README.md) - [Resources](/additionalResources/README.md)
