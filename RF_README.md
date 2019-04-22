# MACE RF Identification
***
The RF portion of the MACE project focuses on identifying devices based on three different types of communication protocols: Wifi, Bluetooth, and Zigbee. Each communication protocol has a different procedure for detection. 
## Wifi 
#### Dependancies:
[GNURadio Install tutorial](https://kb.ettus.com/Building_and_Installing_the_USRP_Open-Source_Toolchain_(UHD_and_GNU_Radio)_on_Linux)
[RFTap Encapsulation Block](https://github.com/rftap/gr-rftap.git)
[IEEE802.11](https://github.com/bastibl/gr-ieee802-11.git)
Wireshark 
 ```
        sudo add-apt-repository ppa:wireshark-dev/stable 
        sudo apt update
        sudo apt install wireshark
```
TShark
```
        sudo apt install tshark
```
    
#### Steps for detection:
1. Run GNURadio GRC file
    * Adjust channel and gain accordingly
2. To see all data packets live: Open Wireshark loopback
3. View data in various styles
* To see visual frequency offset run: 
```
        $tshark -i lo -q -l -Y 'wlan.ta' -T fields -e wlan.ta -e rftap.freqofs | \gawk '{printf("%s %*s\n",$1, ($2+50000)/3000,"*")}'
```
* To get 2 columns of data with unique MAC address:    
```
        tshark -r wifi1.pcap -T fields -e wlan.ta
        tshark -i lo -q -l -Y 'wlan.ta' -T fields -e wlan.ta -e rftap.freqofs > test.txt
        sort -u test.txt
```
* To view frequency offset from specific MAC address 
```
        $tshark -r wifi2.pcap -Y wlan.ta==d4:6a:91:75:d0:f5 -T fields -e wlan.ta -e rftap.freqofs
```
## Bluetooth
#### Dependancies:
[GNURadio Install tutorial](https://kb.ettus.com/Building_and_Installing_the_USRP_Open-Source_Toolchain_(UHD_and_GNU_Radio)_on_Linux)
[BLE Dump](https://github.com/drtyhlpr/ble_dump.git)
Wireshark
```
        sudo add-apt-repository ppa:wireshark-dev/stable 
        sudo apt update
        sudo apt install wireshark
```

#### Steps for detection:
1. Open terminal 
2. Enter BLE Dump directory
* To save to a PCAP File:
```
        $sed -i -e "s/message_sink_msgq_out,/message_queue,/" -e "s/message_sink_msgq_out = virtual_sink_msgq_in/self.message_queue = message_queue/" ./grc/gr_ble.py
        $./ble_dump.py -o /tmp/test.pcap
```
* To view live in Wireshark 
```
        mkfifo /tmp/file
        ./ble_dump.py -o /tmp/file
        wireshark -S -k -i /tmp/file
```
_This data can be pulled from pcap file and organized similarly to the Wifi procedure._
## Zigbee
_Detecting Zigbee signals was unsuccessful during this phase of project MACE. Below are a few examples that are supposed to successfully detect Zigbee signals and tranfer the data packets to wireshark. These examples will have to be modified for the source. After the data packets are detected and saved in a pcap file, tshark can be used similarly to the Wifi procedure._
#### Dependancies:
[GNURadio Install tutorial](https://kb.ettus.com/Building_and_Installing_the_USRP_Open-Source_Toolchain_(UHD_and_GNU_Radio)_on_Linux)
[RFTap Encapsulation Block](https://github.com/rftap/gr-rftap.git)
[IEEE802.15.4](https://github.com/bastibl/gr-ieee802-15-4.git)
[Gr-Foo](https://github.com/bastibl/gr-foo.git)
Wireshark 
 ```
        sudo add-apt-repository ppa:wireshark-dev/stable 
        sudo apt update
        sudo apt install wireshark
```
#### Examples
1. RFTap Example
2. IEEE802.15.4 Example
3. [Mountain Logic GRC Example](https://github.com/MountainLogic/GnuRadio-Wireshark-Example.git)
## Important Notes
* Be sure to read all information given in links above
* Source used is an Ettus B200Mini, GNURadio flow chart can be modified for various sources
* All GitHub repositories can be cloned using the following
```
    git clone https://github.com/...
    cd ...
    mkdir build
    cd build
    cmake ..
    make
    sudo make install
    sudo ldconfig
```
        
