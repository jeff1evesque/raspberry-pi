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

###SD Boot Partition

The [SD Card](http://en.wikipedia.org/wiki/Secure_Digital) is an important and integral part of the [Raspberry Pi](http://en.wikipedia.com/wiki/Raspberry_Pi). When a [power supply](http://www.raspberrypi.org/help/faqs/#power) is provided, the Raspberry Pi seeks a [boot loader](http://en.wikipedia.org/wiki/Booting#BOOT-LOADER) [partition](http://en.wikipedia.org/wiki/Disk_partitioning) from its SD Card slot. If either the SD Card, or boot loader partition is absent, the Raspberry Pi will not start-up.

To ensure the presence of a *boot loader* partition, an acceptable [linux distribution](http://en.wikipedia.org/wiki/Linux_distribution) [image](http://www.raspberrypi.org/documentation/installation/installing-images/) must be placed on the SD Card. The following are the necessary steps to add the Raspbian ([Wheezy](http://www.raspberrypi.org/debian-wheezy-public-beta/)) image:

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
/dev/disk2
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *7.8 GB     disk2
   1:                 DOS_FAT_32 SD-RPI                  7.8 GB     disk2s1

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

**Note:** [`dd`](http://linux.die.net/man/1/dd) *may* take up to [`20mins`](http://www.raspberrypi.org/forums/viewtopic.php?f=66&t=21995#p309093), with [`pv`](http://linux.die.net/man/1/pv) displaying a [transfer rate](http://en.wikipedia.org/wiki/Data_rate_units) between `3.9-5.23MiB/s`.

**Note:** if `bs=1m` was included *only* before the first pipe, or excluded entirely from the command, the [transfer rate](http://en.wikipedia.org/wiki/Data_rate_units) could vary between `660-740KiB/s`, or roughly 8x slower.

**Note:** if `sudo dd of=/dev/disk2` was used instead of `sudo dd of=/dev/rdisk2`, the [transfer rate](http://en.wikipedia.org/wiki/Data_rate_units) could be `1MiB/s`, or roughly 5x slower.

###USB System Partition

The system partition is the disk partition that contains the [operating system](http://en.wikipedia.org/wiki/Operating_system) folder, known as system root. By default, in Linux, operating system files are mounted at `/` (the [root directory](http://en.wikipedia.org/wiki/Root_directory)).

- http://en.wikipedia.org/wiki/System_partition_and_boot_partition#Common_definition

The Raspberry Pi requires the *boot loader* partition to be located on the SD card. However, the *system partition* can be stored on any another medium, such as the USB flash drive.

The steps required to install a system partition on a USB flash drive, is similar to the earlier configured *boot partition* on the SD card. Plug in the USB flash drive to a computer, format the drive to `MS-DOS (FAT)`, then open up terminal:

```
$ diskutil list
/dev/disk0
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *500.1 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:                  Apple_HFS Macintosh HD            499.2 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.0 MB   disk0s3
/dev/disk3
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *3.9 GB     disk3
   1:             Windows_FAT_32 boot                    58.7 MB    disk3s1
   2:                      Linux                         3.2 GB     disk3s2
/dev/disk4
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *64.0 GB    disk4
   1:                 DOS_FAT_32 USB-RPI-1               64.0 GB    disk4s1
$ diskutil unmountDisk /dev/disk4
Unmount of all volumes on disk4 was successful
$ sudo dd if=2014-09-09-wheezy-raspbian.img | sudo pv | sudo dd of=/dev/rdisk4s1 bs=1m
6400000+0 records in7MiB/s] [                                                                    <=>]
6400000+0 records out
3276800000 bytes transferred in 650.785056 secs (5035149 bytes/sec)
3.05GiB 0:10:50 [ 4.8MiB/s] [                                                                  <=>  ]
0+50007 records in
0+50007 records out
3276800000 bytes transferred in 650.881670 secs (5034402 bytes/sec)
```

Once the USB flash drive contains a system partition, specifically the *Raspbian* operating system, the boot loader partition on the SD card, needs to know where this system partition is located. In terminal, open `cmdline.txt` from the earlier configured SD Card (boot partition):

```
cd /Volumes/boot/
sudo pico cmdline.txt
```

**Note:** on linux distributions, navigate to `/media/boot/` subdirectory to modify `cmdline.txt`.

Next, modfiy the contents of `cmdline.txt`:

```
dwc_otg.lpm_enable=0 console=ttyAMA0,115200 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait
```

by changing the `root` variable as follows:

```
dwc_otg.lpm_enable=0 console=ttyAMA0,115200 console=tty1 root=/dev/sda2 rootfstype=ext4 elevator=deadline rootwait
```

This modifies the boot sequence, and tells the Raspberry Pi to boot the system partition from the USB flash drive, instead of the SD card. By default, the earlier configured SD card would boot the existing Raspbian operating system already on it. Now, after the Raspberry Pi has booted, the SD card could be removed, or unmounted. This means, the SD card is only needed during the initial boot.

###MySQL Database

The Raspberry Pi is [capable](http://www.raspberrypi.org/forums/viewtopic.php?f=32&t=6335) of various [database management systems](http://en.wikipedia.org/wiki/Database). Specifically, [MySQL](http://www.mysql.com) has been chosen in this repository. However, since [MariaDB](https://mariadb.org) is considered more optimized, and a fork off MySQL, an [upgrade](https://mariadb.com/kb/en/mariadb/documentation/getting-started/upgrading/upgrading-from-mysql-to-mariadb/), or switch is fairly simple.

To ensure better integrity of the database, login as `root`, and create a new user.

```
$ mysql -u root -p
mysql> CREATE USER 'authenticated'@'localhost' IDENTIFIED BY '[USER_PASSWORD]';
mysql> GRANT CREATE, DELETE, DROP, EXECUTE, SELECT, SHOW DATABASES ON *.* TO 'admin'@'localhost';
mysql> FLUSH PRIVILEGES'
```

**Note:** the created *mysql* user can be used within code, and helps prevent the `root` user from being compromised.

##Testing / Execution

###Test Scripts
