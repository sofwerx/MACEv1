# MACE Network Identification

The mission of the NetworkID MACE sub-task was to identify devices based on network information, build a repository of device network signatures, and create a machine learning algorithm to identify specific devices. NetworkID has successfully completed the Reconnaissance and Discovery phase.

### Software Resources
NetworkID collates a number of open-source projects in order to operate:

* [Kismet](https://kismetwireless.net) - Wireless network and device detection, sniffer, wardriving tool.
* [Nmap Scripting Engine](https://nmap.org/book/nse.html) - Scripting, discovery, sophisticated version detection.
* [GISKismet](https://tools.kali.org/wireless-attacks/giskismet) - Export of Kismet files to referenced Google Earth KML format.
* [NetworkMiner](https://www.netresec.com/?page=Blog&month=2014-02&post=HowTo-install-NetworkMiner-in-Ubuntu-Fedora-and-Arch-Linux) - Network forensics tool for analyzing network traffic.
* [Aircrack-Suite](http://www.aircrack-ng.org/) - Preparing the NetworkID wireless adapter for monitoring.
* [GPSd](http://www.catb.org/gpsd/installation.html) - Daemon that receives data from a GPS receiver, provides data back to Kismet.
* [Google Earth](https://www.google.com/earth/) - 3D Mapping of the KML files generated.

### Hardware Resources
NetworkID has few hardware resources required. It's compact and portable, however, the resources can be scaled to achieve a more powerful range, scope, and degree of accuracy.

- USGlobalSat BU353-S4 USB GPS Receiver
- Raspberry Pi 3 Model B+ V1.2 (32GB SD Card)
- Panda Wireless PAU09 N600 Dual Band (2.4GHz and 5GHz) Wireless N USB Adapter with Dual 5dBi Antennas

## Installations and Dependencies

Note: Raspbian is the recommended default operating system for the NetworkID Scanner, it's a lightweight ARM architecture with little to no bloatware allowing for blank slate installations. 

### NetworkMiner + Mono Framework Installation

This installation of Mono may only work on Ubuntu and other Debian-based distributions. Users of other Linux flavors (such as the recommended Raspbian) as well as Mac OS X can download and install the Mono Framwork from www.mono-project.com/Downloads.

Step 1: Installing the Mono Framework:
```sh
$ sudo apt-get install libmono-system-windows-forms4.0-cil
$ sudo apt-get install libmono-system-web4.0-cil
$ sudo apt-get install libmono-system-net4.0-cil
$ sudo apt-get install libmono-system-runtime-serialization4.0-cil
$ sudo apt-get install libmono-system-xml-linq4.0-cil
```

Step 2: Preparing NetworkMiner:

```sh
$ wget www.netresec.com/?download=NetworkMiner -O /tmp/nm.zip
$ sudo unzip /tmp/nm.zip -d /opt/
$ cd /opt/NetworkMiner*
$ sudo chmod +x NetworkMiner.exe
$ sudo chmod -R go+w AssembledFiles/
$ sudo chmod -R go+w Captures/
```

Step 3: Running NetworkMiner 

```sh
$ mono NetworkMiner.exe
```

### Kismet + GPSd Installation

Step 1: Install Dependencies
```sh
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install gpsd gpsd-clients
$ sudo apt-get install libncurses5 libncurses5-dev
$ sudo apt-get install libnl1 libnl-dev
$ sudo apt-get install libpcap-dev libpcap0.8 libpcap0.8-dev
```
Step 2: Capability to Match MAC Addresses to Manufacturers (Optional) 
```sh
$ cd ~/Downloads
$ wget -O manuf "https://code.wireshark.org/review/gitweb?p=wireshark.git;a=blob_plain;f=manuf"
$ sudo cp manuf /etc/
```
Step 3: Download and Extract Kismet
```sh
$ wget https://www.kismetwireless.net/code/kismet-2016-07-R1.tar.xz
$ tar -xf kismet-2016-07-R1.tar.xz
$ cd kismet-2016-07-R1
```
Step 4: Make and Install Kismet
```sh
$ ./configure
$ make dep
$ make
```
Note: Installation with suid-root (Optional)
```sh
$ sudo make suidinstall
```
##### IMPORTANT NOTES:
1) ADD USER pi to GROUP Kismet (and to operate moving forward without sudo)
```sh
$ sudo usermod -a -G kismet pi
```
2) Before starting Kismet, make certain GPS is running and has a 3D fix.

```sh
$ sudo gpsd /dev/ttyUSB0 -F /var/run/gpsd.sock
```
3) Once a 3D fix is achieved verify with 'cgps'.

```sh
$ cgps
```
##### Reference Guide to Configure Kismet:
Note: There is no need to configure Kismet to run NetworkID. Use this as reference to open the hood if you run into problems running Kismet.
```sh
$ sudo mkdir /var/log/kismet
$ sudo chmod 777 /var/log/kismet
$ sudo nano /usr/local/etc/kismet.conf
```

### Aircrack-ng Installation
We use Aircrack-ng to switch the wireless adapter to monitor mode for the passive sniffing of traffic. Hypothetically, you could also use Aircrack-ng offensively as a follow-on capability.

Step 1) Install Dependencies
```sh
$ sudo apt-get update && \
sudo apt-get install libssl0.9.8 libssl-dev \
build-essential autoconf automake libtool \
pkg-config libnl-3-dev libnl-genl-3-dev \
libssl-dev libsqlite3-dev libpcre3-dev \
ethtool shtool rfkill zlib1g-dev libpcap-dev screen
```
Step 2) Download Aircrack-ng and Extract
```sh
$ wget https://download.aircrack-ng.org/aircrack-ng-1.3.tar.gz
$ tar -zxvf aircrack-ng-1.3.tar.gz
```
Step 3) Compile
```sh
$ cd aircrack-ng-1.3 && \
$ autoreconf -i && \
$ ./configure --enable-shared --with-experimental
$ make -j 4
```
Step 4: Install
```sh
$ make install
```

Available Binaries Post-Installation:
 >   airbase-ng 
 >   aircrack-ng 
 >   airdecap-ng 
 >   airdecloak-ng 
 >   aireplay-ng 
 >   airmon-ng 
 >   airodump-ng 
 >   airodump-ng-oui-update 
 >   airolib-ng 
 >   airserv-ng 
 >   airtun-ng 
 >   airventriloquist-ng

### GIS-Kismet Installation
```sh
$ cd ~/Downloads
$ git clone https://github.com/xtr4nge/giskismet.git
$ sudo apt-get install libxml-libxml-perl libdbi-perl libdbd-sqlite3-perl
$ cd giskismet
$ perl Makefile.PL
$ make
$ sudo make install
```

##### Sample Commands
Add data from a Kismet log file to a SQL-Lite database file:
```sh
$ giskismet -x /inputfile/Kismet-date.netxml --database /outputfile/wireless.dbl
```

Extract data from the SQL-Lite file to a KML file:
```sh
$ giskismet -q "select * from wireless" -o /outputfile/ex1.kml --database /outputfile/wireless.dbl
```

## Launch and Operation of NetworkID Scanner

Once all necessary packages are installed and properly in place follow this procedure to begin.
##### Launch GPS Network Tracking Capabilities
---
```sh
$ sudo systemctl start gpsd
$ sudo systemctl status gpsd
$ sudo gpsd /dev/ttyUSB0 -F /var/run/gpsd.sock
```
##### Begin Monitor Mode on Network Card
---
```sh
$ sudo airmon-ng check
$ sudo airmon-ng check kill
$ sudo airmon-ng start wlan0
$ sudo iwconfig
```
##### Navigate to your Chosen Folder for the Containment of the Scan Outputs
---
```sh
$ cd MACE-Scans
```
##### Initialize Kismet and Begin
---
```sh
$ kismet -c wlan0mon
```

This will sucessfully run the NetworkID Scanner collecting data over any period of time, you can navigate through a variety of different data views and options. Kismet is a versatile tool with many uses, look for outside resources as well as they can not all be covered within this piece of starting documentation.

### END OF DOCUMENT
Used at SOFWERX as part of Phase 1 - Reconnaisance and Discovery of the Military Applications of Cyber Effects (MACE) initiative. 2019. 
