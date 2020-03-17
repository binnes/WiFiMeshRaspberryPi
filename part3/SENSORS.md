*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [**Part 3**](/part3/README.md) - [Resources](/additionalResources/README.md)
***
**Part 3** - [**Connect Sensors**](SENSORS.md) - [IoT Platform](IOT_PLATFORM.md)
***

# Part 3 - Connect and test the sensors

## Section 1 - Connect DHT Temperature Sensor and NeoPixel LED to Raspberry Pi

You can attach the DHT temperature and humidity sensor to any Pi that is part of the mesh network or the bridge Pi. Similarly, you can attach the RGB LED to any pi that is part of the mesh network or the bridge Pi. You can attach the DHT sensor and RGB LED to the same Pi or use different Pis.

### Step 1 - Connect the DHT sensor to your Raspberry Pi

Disconnect the Raspberry Pi from the power supply before connecting the DHT sensor. Depending on the type of DHT sensor that you ordered, there are several ways to connect them.

If your DHT sensors have 4 connecting pins.follow these wiring steps. When looking at the front of the sensor (mesh case) with the pins at the bottom, the connections are (left to right):

* +'ve voltage
* Data
* Not used
* Ground

If you have a DHT mounted on a module then you need to check the pinout, usually indicated on the board, with **+** (to 3V pin), **-** (to G pin) and **out** or **data** :

![DHT module](/images/DHT11-pins.jpg)

### Connect the DHT-11

Connect three female to female jumper wires to the DHT-11 and then follow the Raspberry Pi [pinout diagram](https://pinout.xyz/#) to connect the jumpers to the Raspberry Pi:

| DHT | Raspberry Pi |
| --- | --- |
| Ground | Pin 9 |
| Data | Pin 15 |
| 3.3v Power | Pin 17 |

### Step 2 - Connecting the Neopixel to the Raspberry Pi

Now you need to connect the NeoPixel to the Raspberry Pi. Before you start making any connections please disconnect the device from your power supply so there is no power getting to the device. You should never make any connection changes when the device is powered on.

Before making the connections we need to identify the 4 connecting pins coming out of the LED. If you examine the rim of the pixel cover you will see that one side is flattened (this should be the opposite side from the shortest pin) - this pin next to the flattened side is the **Data Out** pin. We will not be using this pin, as we only have a single pixel. You can chain pixels together connecting the **Data Out** pin to the **Data In** pin of the next pixel in the chain.

The shortest pin on the Pixel is the **Data In**
The longest pin on the Pixel is **Ground**
The remaining pin is **+'ve voltage**, which should be 5v, but it works with 3.3v that the Raspberry Pi provides.

So, with the shortest pin on the left and the flat side on the right the pinout is (left to right):

* Data In (shortest pin)
* +'ve Voltage
* Gnd (longest pin)
* Data Out (no connection)

You need to connect the Data In, +'ve voltage and ground to the Raspberry Pi as shown in the diagram. Take care to ensure that the connections are as shown, as connecting the wrong pins can damage the LED:

![RaspPi LED Wiring](../images/LED-pins.png)

### Step 2 - Connect the NeoPixel

Connect three female to female jumper wires to the NeoPixel and then follow the Raspberry Pi [pinout diagram](https://pinout.xyz/#) to connect the jumpers to :

| NeoPixel | Raspberry Pi |
| --- | --- |
| Ground | Pin 6 |
| Data | Pin 12 |
| 3.3v Power | Pin 1 |

## Section 2 - Install Node-RED and Node-RED packages

Our next step will be to install packages on the Raspberry Pi(s) with the DHT sensor and RGB LED. The easiest way to connect from your laptop to each of the nodes is to complete the [Bridge Access Point steps](../part2/WIFIBRDG.md) from Part 2.

Determine the IP addresses of each of the Raspberry Pi nodes in your mesh network and use **ssh** to connect to each of them. Install the following packages on each of the Raspberry Pi nodes that you have connected the DHT sensor and RGB LED.

### Install Node.js on your Raspberry Pi

Note: *The DHT sensor install does not yet work with Node.js v12*

These steps detail how to install the Node-RED and node.js packages and some Node-RED prerequisites. The nodered package has a dependency on the nodejs package. The first command will also install the latest nodejs LTS package.

``` plain
sudo apt-get install -y nodered
sudo npm -g install npm
sudo npm -g install node-pre-gyp
sudo npm -g install node-gyp
```

#### node-red-node-pi-neopixel Install instructions

On the Raspberry Pi with the RGB LED attached, install node-red-node-pi-neopixel prerequisites
described in [https://flows.nodered.org/node/node-red-node-pi-neopixel](https://flows.nodered.org/node/node-red-node-pi-neopixel)

* Select to continue (y), but don't perform a full install (N) and don't let it configure sound (N)

``` bash
curl -sS get.pimoroni.com/unicornhat | bash
```

don't reboot the pi just yet, so select N then install the Node-RED node for the neopixel:

``` bash
sudo npm -g install node-red-node-pi-neopixel
```

#### node-red-contrib-dht-sensor DHT Sensor install instructions

On the Raspberry Pi with the DHT sensor attached, follow the BMC2835 install instructions at [http://www.airspayce.com/mikem/bcm2835/index.html](http://www.airspayce.com/mikem/bcm2835/index.html)

``` bash
wget http://www.airspayce.com/mikem/bcm2835/bcm2835-1.62.tar.gz
tar zxvf bcm2835-1.62.tar.gz
cd bcm2835-1.62
./configure
make
sudo make check
sudo make install
```

Finally install node-red-contrib-dht-sensor

``` bash
sudo npm -g install --unsafe-perm node-red-contrib-dht-sensor
```

### Reboot your Raspberry Pi mesh node

Reboot all the Raspberry Pis that have a DHT sensor or RGB LED sensor attached:

``` bash
sudo reboot -n
```

## Section 3 - Test the Sensors and LED

In this section, you will test the sensor readings by creating some simple Node-RED flows. Try to create the flows by following the steps. You can also import the solution flows from github.

### Start Node-RED

Reconnect to each Raspberry Pi with a sensor or LED attached via **ssh**, login and run the following command:

``` bash
node-red
```

Open a browser on your laptop to the IP Address or hostname.local of the desired Raspberry Pi. Node-RED is running on Port 1880 - [http://pi_address:1880](http://pi_address:1880) or [http://hostname.local:1880](http://hostname.local:1880)

### Test the DHT Temperature Sensor by building this flow

Connect your browser to Node-RED running on the raspberry pi with the DHT sensor connected.

![Node-RED DHT11 Test Flow](/images/Node-RED-DHT11-flow.png)

#### Flow Instructions

* Select an **Inject** node from the left palette and drag it to your flow canvas.
* Select a **rpi-dht22** node from the palette and drag it to your flow. Double-click on the **rpi-dht22** node. In the Edit dialog, select type of DHT sensor (DHT11 / DHT22) and the Pin number (15) that you have connected the female jumper wire to the Raspberry Pi (Pin 15 suggested above). Press the **Done** button to close the Edit dialog.
  ![DHT config](/images/DHTconfig.png)
* Select a **Debug** node from the palette and drag it to your flow.
* Wire the three nodes as suggested in the screen shot.
* Press the red **Deploy** button.
* To test your flow, click on the tab leading into the **Inject** node.
* Turn to the Node-RED **Debug** right pane by clicking on the little beetle bug icon.
* To test your flow, click on the tab leading into the **Inject** node.

If you are stuck, import the solution:

* From the Node-RED menu (☰), select Import then copy and paste the solution flow below into the Import nodes dialog

  ```JSON
  [{"id":"2c2e21c6.64fe66","type":"rpi-dht22","z":"b701c9b1.f426a8","name":"DHT Sensor","topic":"rpi-dht22","dht":"11","pintype":"2","pin":"15","x":350,"y":180,"wires":[["78a9a66d.0663c8"]]},{"id":"6723ad1.e604c54","type":"inject","z":"b701c9b1.f426a8","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":180,"y":180,"wires":[["2c2e21c6.64fe66"]]},{"id":"78a9a66d.0663c8","type":"debug","z":"b701c9b1.f426a8","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":530,"y":180,"wires":[]}]
  ```

* Press the Import button to complete the import, then click on the editor where you want to place the imported nodes

### Test the NeoPixel LED by building this flow

Connect your browser to Node-RED running on the raspberry pi with the RGB LED connected.

![Node-RED NeoPixel Test Flow](/images/Node-RED-NeoPixel-flow.png)

The NeoPixel Test flow uses **Inject** nodes and **Change** nodes to send RBG color values to the NeoPixel LED.

* From the Node-RED menu (☰), select Import then copy and paste the solution flow below into the Import nodes dialog

  ```JSON
  [{"id":"f0a2eeff.563c28","type":"inject","z":"7e5ededd.5355d8","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":140,"y":200,"wires":[["68f71092.7ed0e8"]]},{"id":"aa512d8e.317718","type":"change","z":"7e5ededd.5355d8","name":"Off - Other","rules":[{"t":"set","p":"payload","pt":"msg","to":"0,0,0","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":430,"y":320,"wires":[["17118e26.f25012"]]},{"id":"39a015f6.c6e582","type":"change","z":"7e5ededd.5355d8","name":"Pink Color","rules":[{"t":"set","p":"payload","pt":"msg","to":"#FF1493","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":430,"y":280,"wires":[["17118e26.f25012"]]},{"id":"1c861bf3.f4acf4","type":"change","z":"7e5ededd.5355d8","name":"Purple Color","rules":[{"t":"set","p":"payload","pt":"msg","to":"238,130,238","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":430,"y":240,"wires":[["17118e26.f25012"]]},{"id":"68f71092.7ed0e8","type":"change","z":"7e5ededd.5355d8","name":"Green Color","rules":[{"t":"set","p":"payload","pt":"msg","to":"#00FF00","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":430,"y":200,"wires":[["17118e26.f25012"]]},{"id":"479915c8.8880dc","type":"change","z":"7e5ededd.5355d8","name":"Blue Color","rules":[{"t":"set","p":"payload","pt":"msg","to":"blue","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":430,"y":160,"wires":[["17118e26.f25012"]]},{"id":"1624ce32.be371a","type":"change","z":"7e5ededd.5355d8","name":"Red Color","rules":[{"t":"set","p":"payload","pt":"msg","to":"red","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":420,"y":120,"wires":[["17118e26.f25012"]]},{"id":"f334b3ad.16fe7","type":"change","z":"7e5ededd.5355d8","name":"Yellow Color","rules":[{"t":"set","p":"payload","pt":"msg","to":"255,255,0","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":430,"y":80,"wires":[["17118e26.f25012"]]},{"id":"2f103b9a.c56d24","type":"inject","z":"7e5ededd.5355d8","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":140,"y":160,"wires":[["479915c8.8880dc"]]},{"id":"5abefa34.531984","type":"inject","z":"7e5ededd.5355d8","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":140,"y":120,"wires":[["1624ce32.be371a"]]},{"id":"75977be3.1cb8dc","type":"inject","z":"7e5ededd.5355d8","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":140,"y":80,"wires":[["f334b3ad.16fe7"]]},{"id":"b3a5588a.75cd3","type":"inject","z":"7e5ededd.5355d8","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":140,"y":240,"wires":[["1c861bf3.f4acf4"]]},{"id":"348d5d9c.35d402","type":"inject","z":"7e5ededd.5355d8","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":140,"y":280,"wires":[["39a015f6.c6e582"]]},{"id":"c52803ea.cef008","type":"inject","z":"7e5ededd.5355d8","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":140,"y":320,"wires":[["aa512d8e.317718"]]},{"id":"17118e26.f25012","type":"rpi-neopixels","z":"7e5ededd.5355d8","name":"Neopixel","pixels":"1","bgnd":"","fgnd":"","wipe":"40","mode":"pcent","rgb":"grb","brightness":"100","gamma":true,"x":680,"y":200,"wires":[]}]
  ```

* Press the Import button to complete the import, then click on the editor where you want to place the imported nodes

Test the various colours work by pressing the button on the inject nodes.  Look at the change node configurations (double click on the node to open the configuration) to see the various ways you can enter colour values.

If the red and green colours are switched, then open the configuration of the Neopixel node and change the Pixel order to **RGB**, as some neopixels want data to be sent in order Red, Green then Blue values whilst other neopixels use a data order of Green, Red then Blue.

***
*Quick links :*
***
**Part 3** - [**Connect Sensors**](SENSORS.md) - [IoT Platform](IOT_PLATFORM.md)
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [**Part 3**](/part3/README.md) - [Resources](/additionalResources/README.md)
