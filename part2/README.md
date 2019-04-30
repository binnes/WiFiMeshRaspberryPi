*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [**Part 2**](/part2/README.md) - [Part 3](/part3/README.md) - [Resources](/additionalResources/README.md)
***

# Part 2
### Install Node.JS v10 on your Raspberry Pi following the [node install instructions](https://github.com/nodesource/distributions).
*Note: The DHT sensor install does not yet work with Node.JS v12*
```
$ sudo bash
$ curl -sL https://deb.nodesource.com/setup_10.x | bash -
$ exit
```
Next, install the nodejs package
```
$ sudo apt-get install -y nodejs
```

### Install Node-RED and required packages
```
$ sudo npm -g install node-red --unsafe-perms
$ sudo npm -g install  node-red-contrib-ibm-watson-iot
```
Next Install node-red-node-pi-neopixel pre-reqs
described in https://flows.nodered.org/node/node-red-node-pi-neopixel
* Select to continue (y), but don't perform a full install (N) and don't let it configure sound (N)

```
$ curl -sS get.pimoroni.com/unicornhat | bash
```
then
```
$ sudo npm -g install node-red-node-pi-neopixel
```
#### DHT Sensor install
Follow the BMC2835 install instructions at http://www.airspayce.com/mikem/bcm2835/index.html
```
$ wget http://www.airspayce.com/mikem/bcm2835/bcm2835-1.58.tar.gz
$ tar zxvf bcm2835-1.58.tar.gz
$ cd bcm2835-1.58
$ ./configure
$ make
$ sudo make check
$ sudo make install
```
Next install pre-reqs
```
$ sudo bash
# npm -g install nan
# npm -g install node-pre-gyp
# npm -g install node-gyp
# exit
```
Finally install node-red-contrib-dht-sensor
```
$ sudo npm install --unsafe-perm -g node-red-contrib-dht-sensor
```
### Reboot

### Start Node-RED
```
node-red
```
Open a browser on your laptop to the IP Address of the Raspberry Pi mesh device. Node-RED is running on Port 1880.
```
http://192.168.1.35:1880
```

## Connect the DHT-11
Connect three female to female jumper wires to the DHT-11 and then follow the [pinout diagram](https://pinout.xyz/#) to connect the jumpers to :
* Ground - Pin 9
* Data - Pin 15
* 3.3v Power - Pin 17

### Build this flow
![Node-RED DHT11 Flow](/images/Node-RED-DHT11-flow.png)
Import this flow

### Connect the NeoPixel
Connect three female to female jumper wires to the NeoPixel and then follow the [pinout diagram](https://pinout.xyz/#) to connect the jumpers to :
* Ground - Pin 6
* Data - Pin 12
* 3.3v Power - Pin 1

### Build this flow
![Node-RED NeoPixel Flow](/images/Node-RED-NeoPixel-flow.png)
Import this flow

### junk
I don't think this is necessary. I did these steps...
cd /usr/lib/node_modules
mkdir node-dht-sensor
cd node-dht-sensor
#  npm view  node-dht-sensor
wget https://registry.npmjs.org/node-dht-sensor/-/node-dht-sensor-0.1.0.tgz
tar zxvf node-dht-sensor-0.1.0.tgz
mv package/* .

# "/usr/bin/node" "/usr/bin/node-gyp" "configure"
npm install
***
*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [**Part 2**](/part2/README.md) - [Part 3](/part3/README.md) - [Resources](/additionalResources/README.md)
