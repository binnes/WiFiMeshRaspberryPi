*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [**Part 2**](/part2/README.md) - [Part 3](/part3/README.md) - [Resources](/additionalResources/README.md)
***
**Part 2** - [WiFi Gateway](WIFIGW.md) - [**WiFi Bridge**](WIFIBRDG.md)
***

# Part 2 - WiFi Access Point bridged to mesh

1. Install additional software to run the access point using command ```sudo apt-get install -y hostapd```
2. Edit file /etc/hostapd/hostapd.conf as root user, you may need to create this, and set the user as:

```text

```

3. Edit file /etc/default/hostapd so it looks like :

```text

```

notice the .... options.

***
*Quick links :*
***
**Part 2** - [WiFi Gateway](WIFIGW.md) - [**WiFi Bridge**](WIFIBRDG.md)
***
[Home](/README.md) - [Part 1](/part1/README.md) - [**Part 2**](/part2/README.md) - [Part 3](/part3/README.md) - [Resources**](/additionalResources/README.md)