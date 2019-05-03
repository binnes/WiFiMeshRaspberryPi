*Quick links :*
***
[Home](/README.md) - [**Part 1**](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [Resources](/additionalResources/README.md)
***
**Part 1** - [**Mesh Networks**](MESH.md) - [Build a Mesh network](PIMESH.md) - [Network access](ROUTE.md)
***

# Part 1 - Mesh Networks

![star topology](/images/star_topology.png)

The most common pattern for wireless networks is the star topology, where there is a central **Access Point** or transmitter station and all devices wanting to communicate on the network need to be in range of the access point.  

All communication on the network happens between a device and the access point, so traffic between 2 devices on the network must go via the access point.

If a device moves it can disconnect from an access point and connect to a different access point to maintain connectivity, but to be on the network it needs to be in range of an access point.

## Mesh network topology

![mesh topology](/images/mesh_topology.png)

With a mesh network there is no central access point or hub.  The network is formed by the devices making direct connections with other devices in range (neighbours).

The mesh protocol and the algorithms managing the network work out how to route data packets, so any device that is part of the network can talk to all other devices on the network, so long as there is an available route through the network.

In the mesh you will implement as you work through the following sections you will create a dynamic mesh network based on the [802.11s](https://en.wikipedia.org/wiki/IEEE_802.11s) open standard, which is built into the Linux kernel.  This mesh network standard does not require devices to be fixed and static.  

Additional devices can arrive and join the mesh network, devices can move location within the network, so connections will be lost and new neighbours found and devices may go out of range of all other devices and leave the network.  The mesh will automatically reconfigure itself as device connections change.

## Connectivity to the wider Internet

With the star topology the route from the wireless network to the wider internet is always via the central access point, so the access point needs to be connected to a network with access to the internet.

With a mesh network, at least one of the mesh devices needs to be a gateway to another network with Internet connectivity.  Many mesh protocols allow multiple gateway nodes to exist and the mesh will route to the nearest one to provide connectivity outside the mesh.

***
*Quick links :*
***
**Part 1** - [**Mesh Networks**](MESH.md) - [Build a Mesh network](PIMESH.md) - [Network access](ROUTE.md)
***
[Home](/README.md) - [**Part 1**](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [Resources**](/additionalResources/README.md)