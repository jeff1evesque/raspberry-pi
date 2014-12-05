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

The following packages need to be installed through terminal:

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

**Note:** This project assumes [Raspbian](http://www.raspbian.org) (i.e. [Wheezy](http://raspberrypi.org/debian-wheezy-public-beta/)) as the operating system. To upgrade to the newer [Jessie](http://www.raspberrypi.org/forums/viewtopic.php?f=66&t=47944) distribution, modify the following:

```
sudo pico /etc/apt/sources.list
```

by changing any reference(s) of [`wheezy`](http://www.raspberrypi.org/debian-wheezy-public-beta/) to [`jessie`](http://www.raspberrypi.org/forums/viewtopic.php?f=66&t=47944). Then, upgrade the distribution:

```
sudo apt-get dist-upgrade
```

The above *upgrade* would allow the use of [MariaDB](https://mariadb.org), in place of its predecessor [MySQL](http://www.mysql.com). Therefore, the above:

```
...
sudo apt-get install mysql-server mysql-client
...
```

would be replaced by the following:

```
...
sudo apt-get install mariadb-server mariadb-client
...
```

##Configuration

###GIT

####GIT Submodule

###File Permission

###SD Card

The [SD Card](http://en.wikipedia.org/wiki/Secure_Digital) is an important and integral part of the [Raspberry Pi](http://en.wikipedia.com/wiki/Raspberry_Pi). When a [power supply](http://www.raspberrypi.org/help/faqs/#power) is provided, the Raspberry Pi seeks a [boot loader](http://en.wikipedia.org/wiki/Booting#BOOT-LOADER) [partition](http://en.wikipedia.org/wiki/Disk_partitioning) from its SD Card slot. If either the SD Card, or boot loader partition is absent, the Raspberry Pi will not start-up.

To ensure the presence of a *boot loader* partition, an acceptable [linux distribution](http://en.wikipedia.org/wiki/Linux_distribution) [image](http://www.raspberrypi.org/documentation/installation/installing-images/) must be placed on the SD Card. The following are the necessary steps to add the Raspbian (Wheezy) image:

```
# create temporary directory
$ sudo mkdir /raspbian/
$ cd /raspbian/

# install 'Raspbian' image (not on SD card)
$ sudo wget -P /raspbian/ http://downloads.raspberrypi.org/raspbian_latest
$ ls
raspbian_latest
$ sudo unzip raspbian_latest
$ ls
2014-09-09-wheezy-raspbian.img          raspbian_latest

# get partition listing
$ diskutil list
/dev/disk0
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *1.0 TB     disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:                  Apple_HFS Macintosh HD            999.3 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
/dev/disk1
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     Apple_partition_scheme                        *27.9 MB    disk1
   1:        Apple_partition_map                         32.3 KB    disk1s1
   2:                  Apple_HFS src/sqlitebrowser       27.8 MB    disk1s2
/dev/disk2
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *7.8 GB     disk2
   1:                 DOS_FAT_32 SD-RPI                  7.8 GB     disk2s1
/dev/disk4
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *64.0 GB    disk4
   1:                 DOS_FAT_32 USB-RPI-1               32.0 GB    disk4s1
   2:                 DOS_FAT_32 USB-RPI-2               32.0 GB    disk4s2

# copy 'Raspbian' image to SD card
$ sudo diskutil unmountDisk /dev/disk2
$ sudo dd if=2014-09-09-wheezy-raspbian.img | sudo pv | sudo dd of=/dev/rdisk2 bs=1m
6400000+0 records in1MiB/s] [                                                                        <=>       ]
6400000+0 records out
3276800000 bytes transferred in 646.064640 secs (5071938 bytes/sec)
3.05GiB 0:10:46 [4.84MiB/s] [                                                                          <=>     ]
0+50000 records in
0+50000 records out
3276800000 bytes transferred in 646.141940 secs (5071332 bytes/sec)

# delete temporary directory
$ sudo rm -rf /raspbian/
```

**Note:** the last command *may* take up to [`20mins`](http://www.raspberrypi.org/forums/viewtopic.php?f=66&t=21995#p309093), with [`pv`](http://linux.die.net/man/1/pv) having a transfer range between `3.9-5.23MiB/s`.

**Note:** if `bs=1m` was included only before the first pipe, or if it was completely excluded, the transfer rate would have varied between `660-740KiB/s`.

**Note:** if `sudo dd of=/dev/disk2` was used instead of `sudo dd of=/dev/rdisk2`, the transfer rate would have been roughly `1MiB/s`.

##Testing / Execution

###Test Scripts
