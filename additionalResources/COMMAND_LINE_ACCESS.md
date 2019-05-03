*Quick links :*
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [**Resources**](/additionalResources/README.md)
***
**Resources** - [Setup](PREREQUISITES_AND_SETUP.md) - [Headerless Setup](HEADERLESS_SETUP.md) - [**Command line access**](COMMAND_LINE_ACCESS.md)
***

# Additional Resources - Command Line Access to a Raspberry Pi


## ssh access from windows

There are a number options when working on Windows to access the Raspberry Pi over the network using ssh:

- Install the [putty application](https://www.putty.org)
- On Windows 10, since autumn 2018, openssh is now included, so you can use the ssh command in a command prompt or powershell window (from a command line enter ```ssh -V``` to test if it is available on your system)
- On Windows 10 install the [Linux system for Windows](https://docs.microsoft.com/en-us/windows/wsl/install-win10), then choose the distribution you want to use from the Windows store to give access to a linux environment running within Windows.  Once you have the linux distibution of choice installed, you should update the distribution and install ssh.  E.g. for a Debian use the following commands: ```sudo apt-get update && sudo apt-get upgrade -y && sudo apt-get install -y ssh``` 

***
*Quick links :*
***
**Resources** - [Setup](PREREQUISITES_AND_SETUP.md) - [Headerless Setup](HEADERLESS_SETUP.md) - [**Command line access**](COMMAND_LINE_ACCESS.md)
***
[Home](/README.md) - [Part 1](/part1/README.md) - [Part 2](/part2/README.md) - [Part 3](/part3/README.md) - [**Resources**](/additionalResources/README.md)
