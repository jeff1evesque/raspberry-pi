Raspberry Pi
============

###Definition

The [Raspberry Pi](http://en.wikipedia.org/wiki/Raspberry_Pi) is a credit-card sized computer containing at least 1 USB slot, which can be used for any USB device containing a [linux](http://en.wikipedia.org/wiki/Linux) driver.  Since the inception of the Raspberry Pi ([model A](http://www.raspberrypi.org/products/model-a/)) in February 2012, there have been two additional models made available, the [model B](http://www.raspberrypi.org/products/model-b/), and the [model B+](http://www.raspberrypi.org/products/model-b-plus/).

The major schematic are as follows:

                | Model A       | Model B       | Model B+       |
---------------:| --------------|:-------------:| --------------:|
**Processor**   | 700 MHz       | 700 MHz       | 700 MHz        |
**RAM**         | 256 MB        | 512 MB        | 512 MB         |
**USB Ports**   | Single USB    | Dual USB      | Quad USB       |

Numerous [*linux-based*](http://en.wikipedia.org/wiki/Linux) operating systems can be run on the [Raspberry Pi](http://en.wikipedia.org/wiki/Raspberry_Pi). The [Rasbian](http://www.raspbian.org/) is generally preferred, since it is tailored specific to the Pi.  Other operating systems such as [Risc](https://www.riscosopen.org/), [Plan 9](http://plan9.bell-labs.com/plan9/), [Android](http://www.android.com/), and [Arch](http://arch-os.com/) is just as savy.  Though, not recommended, linux distributions such as [Ubuntu](www.ubuntu.com) will work.  Keep in mind, the requirements of such operating systems tend to require more resources than the Pi has to offer (so grab an older distro).

###Overview

##Installation

###Linux Packages

The following packages need to be installed through terminal in Ubuntu:

```
# Python Package Manager:
sudo apt-get install python-setuptools
sudo easy_install pip

# LAMP (with phpmyadmin):
sudo apt-get install apache2
sudo apt-get install mysql-server mysql-client
sudo apt-get install php5 php5-mysql libapache2-mod-php5
sudo apt-get install phpmyadmin

# Python MySQL Driver:
sudo apt-get install python-mysqldb
```

**Note:** This project assumes [Raspbian](www.raspbian.org) as the operating system. Specifically, the Raspbian *Wheezy* distribution has been selected. To select the *jessie* distribution, modify the following:

```
sudo pico /etc/apt/sources.list
```

by changing the reference of `wheezy` to `jessie`. Then, simply upgrade the distribution (since it's newer):

```
sudo apt-get dist-upgrade
```

This will allow the use of MariaDB, instead of its predecessor MySQL.

##Configuration

###GIT

####GIT Submodule

###File Permission

##Testing / Execution

###Test Scripts
