*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [**Part 3**](/part3/README.md) - [Resources](/additionalResources/README.md)
***
**Part 3** - [Connect Sensors](SENSORS.md) - [**IoT Platform**](IOT_PLATFORM.md)
***

# Part 3 - IoT platform

Before a device can send data to the Watson IoT platform, a device definition needs to to be created in the IoT platform. A device definition is made up of a device type, which is used to group similar devices together, and a devices ID, which uniquely identifies a specific device.

For this demonstration you will create 2 devices, both of the same device type. The first device ID is used to publish DHT data, the second device ID is used to receive commands to set the colour of the RGB LED:

* Device type : **raspi**
* Device ID : **RaspiMeshNode1**
* Device ID : **RaspiMeshNode2**

## Create a Device Instructions

1. From the IBM Cloud Dashboard, select the Resource list from the main menu, then expand the Services and select the Internet of Things Platform instance that was just created.  Then select the Launch button to launch the Internet of Things UI.

![Watson IoT Platform Service](/images/IBMCloud-WatsonIoTPlatform-Service.png)

2. Create a Device Type named **raspi** and a DeviceID named **RaspiMeshNode1**

    You can enter an Authentication token - or leave it blank and a secure token will be generated.

You MUST record the Security Token, as you will need this when connecting the devices to the IoT platform - this token is not recoverable once you finish the *Add Device* process.

![Watson IoT Platform Device](/images/IBMCloud-WatsonIoTPlatform-Device.png)

3. Create a second with the same device type, **raspi** and device ID **RaspiMeshNode2**

## Create a Node-RED Flow in the IBM Cloud application

While we're still working in IBM Cloud, let's launch your Internet of Things Starter Application Node-RED flow

1. Open the Node-RED editor.  From the IBM Cloud web user interface Main menu select Resource list, then expand the Cloud Foundry apps section and select your application.  To launch the Node-RED editor select the **Visit App URL** link from the top of the page.

2. The first time you visit the Node-RED application you will need to go through a security setup, where you will need to enter a username and password to protect the editor, again please record the username and password you enter, as you will need these when you need to access the Node-RED editor.

3. Once you have logged into the Node-RED editor import the sample flow:

    * From the Node-RED menu (☰), select Import then copy and paste the solution flow below into the Import nodes dialog

    ```JSON
    [{"id":"6af44dea.4dfdd4","type":"change","z":"8b17e254.75dfb","name":"Set LED Blue","rules":[{"t":"set","p":"payload","pt":"msg","to":"{\"r\":0,\"g\":0,\"b\":255}","tot":"json"}],"action":"","property":"","from":"","to":"","reg":false,"x":410,"y":110,"wires":[["de1d42d6.06b5a8","7e5c5ff5.e5338"]]},{"id":"f9f2b4ce.9a87e8","type":"switch","z":"8b17e254.75dfb","name":"","property":"payload.d.temperature","propertyType":"msg","rules":[{"t":"lte","v":"0","vt":"num"},{"t":"btwn","v":"0","vt":"num","v2":"10","v2t":"num"},{"t":"btwn","v":"10","vt":"num","v2":"25","v2t":"num"},{"t":"btwn","v":"25","vt":"num","v2":"30","v2t":"num"},{"t":"gte","v":"30","vt":"str"}],"checkall":"false","repair":false,"outputs":5,"x":220,"y":180,"wires":[["6af44dea.4dfdd4"],["e10c6189.4ae0e8"],["c127e2dc.1a3eb"],["2d959417.11746c"],["ff9ed698.210a78"]]},{"id":"e10c6189.4ae0e8","type":"change","z":"8b17e254.75dfb","name":"Set LED Turquoise","rules":[{"t":"set","p":"payload","pt":"msg","to":"{\"r\":0,\"g\":150,\"b\":150}","tot":"json"}],"action":"","property":"","from":"","to":"","reg":false,"x":420,"y":145,"wires":[["de1d42d6.06b5a8","7e5c5ff5.e5338"]]},{"id":"c127e2dc.1a3eb","type":"change","z":"8b17e254.75dfb","name":"Set LED Green","rules":[{"t":"set","p":"payload","pt":"msg","to":"{ \"r\" : 0, \"g\" : 255, \"b\" : 0 }","tot":"json"}],"action":"","property":"","from":"","to":"","reg":false,"x":410,"y":180,"wires":[["de1d42d6.06b5a8","7e5c5ff5.e5338"]]},{"id":"2d959417.11746c","type":"change","z":"8b17e254.75dfb","name":"Set LED Yellow","rules":[{"t":"set","p":"payload","pt":"msg","to":"{ \"r\" : 150, \"g\" : 150, \"b\" : 0 }","tot":"json"}],"action":"","property":"","from":"","to":"","reg":false,"x":410,"y":215,"wires":[["de1d42d6.06b5a8","7e5c5ff5.e5338"]]},{"id":"ff9ed698.210a78","type":"change","z":"8b17e254.75dfb","name":"Set LED RED","rules":[{"t":"set","p":"payload","pt":"msg","to":"{ \"r\" : 255, \"g\" : 0, \"b\" : 0 }","tot":"json"}],"action":"","property":"","from":"","to":"","reg":false,"x":410,"y":250,"wires":[["de1d42d6.06b5a8","7e5c5ff5.e5338"]]},{"id":"de1d42d6.06b5a8","type":"debug","z":"8b17e254.75dfb","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":680,"y":110,"wires":[]},{"id":"dc55e723.40de48","type":"debug","z":"8b17e254.75dfb","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":250,"y":60,"wires":[]},{"id":"ac6df159.2fdc1","type":"mqtt in","z":"8b17e254.75dfb","name":"MQTT In","topic":"iot-2/type/raspi/id/RaspiMeshNode1/evt/sensor/fmt/json","qos":"2","datatype":"json","broker":"f68470a.432c59","x":80,"y":120,"wires":[["dc55e723.40de48","f9f2b4ce.9a87e8"]]},{"id":"7e5c5ff5.e5338","type":"mqtt out","z":"8b17e254.75dfb","name":"MQTT Out","topic":"iot-2/type/raspi/id/RaspiMeshNode2/cmd/led/fmt/json","qos":"","retain":"","broker":"f68470a.432c59","x":730,"y":180,"wires":[]},{"id":"f68470a.432c59","type":"mqtt-broker","z":"","name":"NR security config","broker":"orgId.messaging.internetofthings.ibmcloud.com","port":"8883","tls":"1b634031.8806e8","clientid":"A:orgId:statusApp","usetls":true,"compatmode":false,"keepalive":"60","cleansession":true,"birthTopic":"","birthQos":"0","birthRetain":"false","birthPayload":"","closeTopic":"","closeQos":"0","closeRetain":"false","closePayload":"","willTopic":"","willQos":"0","willRetain":"false","willPayload":""},{"id":"1b634031.8806e8","type":"tls-config","z":"","name":"NR TLS config","cert":"","key":"","ca":"","certname":"","keyname":"","caname":"","servername":"orgId.messaging.internetofthings.ibmcloud.com","verifyservercert":true}]
    ```

    * Press the Import button to complete the import, then click on the editor where you want to place the imported nodes

After importing the flow you will need to add some credentials so the application can communicate with the IoT platform using the MQTT protocol.

4. Open the configuration of either of the MQTT Nodes, then press the pencil icon next to the server field to access the server configuration.

    The values should be set as detailed below, but the **orgId** needs to be changed to the unique 6 characted org ID for your instance of the platform.  You can find this at the top of the IoT console, or in the setting, then Identity section.

    ![orgId](/images/orgId.png)

    * Connection tab:
      * Server : **orgId**.messaging.internetofthings.ibmcloud.com
      * Port : 8883
      * Enable secure (SSL/TLS) connection - should be selected
        * Click the pencil icon next to the TLS configuration to open the security configuration
          * Verify server certificate should be ticked
          * Server Name should be the same value as the Server field on the connection time - **orgId**.messaging.internetofthings.ibmcloud.com
        * Click **Update** to close the TLC configuration panel
      * Client ID : A:**orgId**:statusApp
    * Security tab
      * The Username and Password are generated from the Internet of Things Web UI:  
        * In the IoT console, switch to the Apps section, then press **+ Generate API Key**  
          ![IoT Menu](/images/iotMenu.png)
          * Enter a description ("NR MQTT Nodes") then press Next
          * Select Standard Application for the Role then press Generate Key
      * The Username is the API Key from the newly generate API Key in the IoT platform console
      * The Password is the Authentication Token from the newly generated API Key in the IoT platform console

    * Open the MQTT In node configuration and set the topic to **iot-2/type/raspi/id/RaspiMeshNode1/evt/sensor/fmt/json**.  This topic subscribes to receive messages from the DHT sensor.

    * Open the MQTT Out node configuration and set the topic to **iot-2/type/raspi/id/RaspiMeshNode2/cmd/led/fmt/json**.  This topic specifies where the command to set the LED colour is sent, which is the device with the LED connected.

5. Once the config has been updated and deployed the status for the MQTT nodes should change to **connected**

This flow subscribes to the Raspberry Pi Mesh Node sensor MQTT data. As this temperature data arrives, the flow will determine the temperature thresholds and send a command back to the mesh node to change the LED

![Node-RED Cloud Send Mesh Alert Flow](/images/Node-RED-Cloud-Send-Mesh-Alert-flow.png)

## Create a Node-RED flow to send DHT data to the IBM IoT platform

On the Raspberry Pi with the DHT sensor connected, a Node-RED flow will read the sensor value every 10 seconds, then format the data into a JSON format string to send to the IoT platform.

1. Open Node-RED running on the Raspberry Pi with the DHT sensor connected **http://host_name.local:1880** ot **http://IP_address:1880**.

2. Import the sample flow to read the DHT sensor and send the data to the IoT platform:

    * From the Node-RED menu (☰), select Import then copy and paste the solution flow below into the Import nodes dialog

      ```JSON
      [{"id":"596ef8b1.de5a08","type":"inject","z":"7e5ededd.5355d8","name":"Poll every 10 seconds","topic":"","payload":"true","payloadType":"bool","repeat":"10","crontab":"","once":false,"onceDelay":0.1,"x":160,"y":520,"wires":[["4cb554e.f139bac"]]},{"id":"303ba704.e31158","type":"debug","z":"7e5ededd.5355d8","name":"","active":false,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":810,"y":500,"wires":[]},{"id":"e9ae4d54.9155b8","type":"comment","z":"7e5ededd.5355d8","name":"Reformat DHT data ","info":"Reformat DHT data into a MQTT payload that\nWatson IoT Platform Quickstart will plot\n\nConvert:\nmsg.\n    topic: \"rpi-dht11\"\n    payload: \"20.00\"\n    humidity: \"33.00\"\n    isValid: true\n    errors: 2\n    sensorid: \"dht11\"\n    \nto msg.payload = \n{\n    \"d\": {\n        \"temperature\": \"20.00\",\n        \"humidity\": \"33.00\"\n    }\n}","x":560,"y":480,"wires":[]},{"id":"4cb554e.f139bac","type":"rpi-dht22","z":"7e5ededd.5355d8","name":"DHT Sensor","topic":"rpi-dht11","dht":"11","pintype":"2","pin":"15","x":370,"y":520,"wires":[["13b702b5.19966d"]]},{"id":"13b702b5.19966d","type":"change","z":"7e5ededd.5355d8","name":"Build IoT MQTT payload","rules":[{"t":"set","p":"temperature","pt":"msg","to":"payload","tot":"msg"},{"t":"set","p":"payload","pt":"msg","to":"{\"d\":{\"temperature\":0,\"humidity\":0}}","tot":"json"},{"t":"set","p":"payload.d.temperature","pt":"msg","to":"temperature","tot":"msg"},{"t":"set","p":"payload.d.humidity","pt":"msg","to":"humidity","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":580,"y":520,"wires":[["303ba704.e31158","1b6a945.e0f77ec"]]},{"id":"1b6a945.e0f77ec","type":"mqtt out","z":"7e5ededd.5355d8","name":"","topic":"iot-2/evt/sensor/fmt/json","qos":"","retain":"","broker":"b391809e.6ff5f","x":850,"y":540,"wires":[]},{"id":"b391809e.6ff5f","type":"mqtt-broker","z":"","name":"SensorMQTTconfig","broker":"orgId.messaging.internetofthings.ibmcloud.com","port":"8883","tls":"545dd0a8.4e7538","clientid":"d:orgId:raspi:RaspiMeshNode1","usetls":true,"compatmode":false,"keepalive":"60","cleansession":true,"birthTopic":"","birthQos":"0","birthRetain":"false","birthPayload":"","closeTopic":"","closeQos":"0","closeRetain":"false","closePayload":"","willTopic":"","willQos":"0","willRetain":"false","willPayload":""},{"id":"545dd0a8.4e7538","type":"tls-config","z":"","name":"SensorTLSconfig","cert":"","key":"","ca":"","certname":"","keyname":"","caname":"","servername":"orgId.messaging.internetofthings.ibmcloud.com","verifyservercert":true}]
      ```

    * Press the Import button to complete the import, then click on the editor where you want to place the imported nodes

3. Once you have imported the flow you need to update the MQTT configuration, similar to what you did in the previous step:

    * Connection tab:
      * Server : **orgId**.messaging.internetofthings.ibmcloud.com
      * Port : 8883
      * Enable secure (SSL/TLS) connection - should be selected
        * Click the pencil icon next to the TLS configuration to open the security configuration
          * Verify server certificate should be ticked
          * Server Name should be the same value as the Server field on the connection time - **orgId**.messaging.internetofthings.ibmcloud.com
        * Click **Update** to close the TLC configuration panel
      * Client ID : d:orgId:raspi:RaspiMeshNode1
    * Security tab
      * Username : use-token-auth
      * Password : This is the security token for the device **RaspiMeshNode1** - you should have recorded this token when you created the device on the IoT platform

    * In the MQTT config set the topic to **iot-2/evt/sensor/fmt/json**

![Node-RED Mesh node Alert Flow](/images/Node-RED-IoT-Sensor-flow.png)

## Create a Node-RED Flow on the mesh node to receive LED Alerts

On the Raspberry Pi with the LED connected, a Node-RED flow will subscribe to receive commands specifying the colour the LED should be showing - the commands are created by the Node-RED application running in the cloud.

1. Open Node-RED running on the Raspberry Pi with the DHT sensor connected **http://host_name.local:1880** ot **http://IP_address:1880**.

2. Import the sample flow to read the DHT sensor and send the data to the IoT platform:

    * From the Node-RED menu (☰), select Import then copy and paste the solution flow below into the Import nodes dialog

      ```JSON
      [{"id":"49223ab3.adac44","type":"debug","z":"1096425e.432946","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":370,"y":220,"wires":[]},{"id":"eb607213.034688","type":"rpi-neopixels","z":"1096425e.432946","name":"Set LED Color","pixels":"1","bgnd":"","fgnd":"","wipe":"40","mode":"pcent","rgb":"grb","brightness":"100","gamma":true,"x":620,"y":260,"wires":[]},{"id":"69bb1824.256198","type":"template","z":"1096425e.432946","name":"Build LED Alert Color","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"{{payload.r}},{{payload.g}},{{payload.b}}","output":"str","x":400,"y":260,"wires":[["eb607213.034688"]]},{"id":"ebb32fef.b9f248","type":"mqtt in","z":"1096425e.432946","name":"","topic":"iot-2/cmd/led/fmt/json","qos":"2","datatype":"json","broker":"44518c40.2db504","x":140,"y":220,"wires":[["49223ab3.adac44","69bb1824.256198"]]},{"id":"44518c40.2db504","type":"mqtt-broker","z":"","name":"ledMQTTbrokerConfig","broker":"orgId.messaging.internetofthings.ibmcloud.com","port":"8883","tls":"909c71c5.a1822","clientid":"d:orgId:raspi:RaspiMeshNode2","usetls":true,"compatmode":false,"keepalive":"60","cleansession":true,"birthTopic":"","birthQos":"0","birthRetain":"false","birthPayload":"","closeTopic":"","closeQos":"0","closeRetain":"false","closePayload":"","willTopic":"","willQos":"0","willRetain":"false","willPayload":""},{"id":"909c71c5.a1822","type":"tls-config","z":"","name":"ledTLSconfig","cert":"","key":"","ca":"","certname":"","keyname":"","caname":"","servername":"orgId.messaging.internetofthings.ibmcloud.com","verifyservercert":true}]
      ```

    * Press the Import button to complete the import, then click on the editor where you want to place the imported nodes

3. Once you have imported the flow you need to update the MQTT configuration, similar to what you did in the previous step:

    * Connection tab:
      * Server : **orgId**.messaging.internetofthings.ibmcloud.com
      * Port : 8883
      * Enable secure (SSL/TLS) connection - should be selected
        * Click the pencil icon next to the TLS configuration to open the security configuration
          * Verify server certificate should be ticked
          * Server Name should be the same value as the Server field on the connection time - **orgId**.messaging.internetofthings.ibmcloud.com
        * Click **Update** to close the TLC configuration panel
      * Client ID : d:orgId:raspi:RaspiMeshNode2
    * Security tab
      * Username : use-token-auth
      * Password : This is the security token for the device **RaspiMeshNode2** - you should have recorded this token when you created the device on the IoT platform

    * In the MQTT config set the topic to **iot-2/cmd/led/fmt/json**

![Node-RED Mesh node Alert Flow](/images/Node-RED-IoT-Alert-flow.png)

***
*Quick links :*
***
**Part 3** - [Connect Sensors](SENSORS.md) - [**IoT Platform**](IOT_PLATFORM.md)
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [**Part 3**](/part3/README.md) - [Resources](/additionalResources/README.md)
