*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [**Resources**](/additionalResources/README.md)
***
**Resources** - [Setup](PREREQUISITES_AND_SETUP.md) - [IoT Starter App](IOT_STARTER_APP.md) - [Headerless Setup](HEADERLESS_SETUP.md) - [**Command line access**](COMMAND_LINE_ACCESS.md)
***

# Additional Resources - Command Line Access to a Raspberry Pi

For the workshop you need to access the command line of all the Raspberry Pi boards you are setting up.  If you are using a keyboard, mouse and monitor, then you will be prompted with a login screen once the Pi has booted.  The default user name is **pi** and the initial password is set to **raspberry**.

If you completed a [headerless setup](HEADERLESS_SETUP.md) then you need to access the Raspberry Pi over the network.  If you are using an Ethernet connection, then you can either connect the Raspberry Pi directly to your laptop with an Ethernet cable or connect both the Raspberry Pi and your laptop to the same Ethernet network.

If you are going to connect over WiFi then you should have created the wpa_supplicant.conf file on the micro-SD card to configure the WiFi connection for the Raspberry Pi.  Your laptop needs to be connected to the same WiFi network.

The Raspberry Pi comes configured with a network discovery protocol called Zeroconf, which allows devices to discover each other on a network.  Apple uses Zeroconf, so this is built in to MacOS and is known as the Bonjour service.  Many linux distributions have Zeroconf enabled by default, if not, installing the **avahi** package adds it.  

Windows uses a different technology for network discovery, so to enable Windows systems to find a Raspberry Pi on the network you need to add some additional software.  The easiest way to do this is the add the [Apple Bonjour Print Services for Windows](https://support.apple.com/kb/dl999).  If you are an iTunes user and have iTunes installed, then you already have Bonjour installed.

Once you have Zeroconf installed on your system then you can find any host on the network that supports Zeroconf by using the system hostname with **.local** appended.  On first boot a Raspberry Pi has hostname **raspberrypi**, so it can be located on a network using **raspberrypi.local**.

To connect to your raspberry Pi **ssh** will be used.  On MacOS this is available out of the box.  Most Linux distributions also have an SSH client installed, if not the **ssh** package can be installed to add it.  See below for information about using ssh on Windows.

To use ssh on a command line use, the command is ```ssh <remote user>@<hostname>```.  So for a Raspberry Pi that has just been powered on for the fist time you can connect using ```ssh pi@raspberrypi.local```.

You should be prompted for a password, and on first boot the default password is **raspberry**.  You are now logged onto the raspberry pi and should see the command line prompt:

```text
ssh pi@raspberrypi.local
Warning: Permanently added 'raspberrypi.local,fe80::b894:9368:9db2:39c4%en0' (ECDSA) to the list of known hosts.
pi@raspberrypi.local's password:
Linux raspberrypi 4.14.98+ #1200 Tue Feb 12 20:11:02 GMT 2019 armv6l

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri May  3 19:27:44 2019 from fe80::3d1c:5b24:bfe4:f52a%wlan0

SSH is enabled and the default password for the 'pi' user has not been changed.
This is a security risk - please login as the 'pi' user and type 'passwd' to set a new password.

pi@raspberrypi:~ $
```

If you receive a message about a host key verification failure:

```text
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@       WARNING: POSSIBLE DNS SPOOFING DETECTED!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
The ECDSA host key for raspberrypi.local has changed,
and the key for the corresponding IP address fe80::b894:9368:9db2:39c4%en0
is unknown. This could either mean that
DNS SPOOFING is happening or the IP address for the host
and its host key have changed at the same time.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:BrfirIV7EBsrp1dw4O7lZaAF+VMUhZDsNe+X2IU74AM.
Please contact your system administrator.
Add correct host key in /Users/brian/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /Users/brian/.ssh/known_hosts:16
ECDSA host key for raspberrypi.local has changed and you have requested strict checking.
Host key verification failed.
```

It is simply saying that the identity of the host has changed since last time you accessed it.  When you first connect to a system using ssh it stores a signature for the system that gets checked each time you subsequently connect using ssh.  When you setup a new raspberry pi, the signature will be different for each new pi, but they all use the same hostname - **raspberrypi.local**, so SSH will raise an error.  

To get past this issue, you need to remove the signature.  These are stored in file **.ssh/knownhosts** in your user home directory.  Edit this file and remove the line for host raspberrypi.local.

There is a way to prevent this happening, you can add options to the command line:

```text
ssh -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" pi@raspberrypi.local
```

or to permanently disable checking you can create a file .ssh/config in your home directory and add the following content:

```text
Host raspberrypi.local
    StrictHostKeyChecking no
    UserKnownHostsFile=/dev/null
```

which disables storing the signature and signature verification for host raspberrypi.local for all future uses of ssh, so you no longer need to add the command line options when connecting.

## ssh access from windows

There are a number options when working on Windows to access the Raspberry Pi over the network using ssh:

- Install the [putty application](https://www.putty.org)
- On Windows 10, since autumn 2018, openssh is now included, so you can use the ssh command in a command prompt or powershell window (from a command line enter ```ssh -V``` to test if it is available on your system)
- On Windows 10 install the [Linux system for Windows](https://docs.microsoft.com/en-us/windows/wsl/install-win10), then choose the distribution you want to use from the Windows store to give access to a linux environment running within Windows.  As Raspbian is based on the Debian Linux distribution, it is a good option to choose.  Once you have the linux distribution of choice installed, you should update the distribution and install ssh.  E.g. for a Debian use the following commands: ```sudo apt-get update && sudo apt-get upgrade -y && sudo apt-get install -y ssh```

If using the *Linux subsystem for Windows* open the Linux application (which you will find under the distribution name in the start menu, so **Debian GNU/Linux** if you installed the Debian distribution) or if using *openssh* then open a command prompt or PowerShell window and use the ssh command as shown above for MacOS and Linux users.

If you are using the putty application then open the application and create the setup as shown below:

![putty](/images/putty.png)

Hit **open** to start the connection.  If you get a warning about the identity having changed, you can ignore it and continue connecting.  Once you connect you will be prompted for a username then a password.  The default user on a Raspberry Pi is **pi** and the initial password is set to **raspberry**.

***
*Quick links :*
***
**Resources** - [Setup](PREREQUISITES_AND_SETUP.md) - [IoT Starter App](IOT_STARTER_APP.md) - [Headerless Setup](HEADERLESS_SETUP.md) - [**Command line access**](COMMAND_LINE_ACCESS.md)
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [**Resources**](/additionalResources/README.md)
