# tcpdump

## 1. Introduction

This is an RT-Thread-based gadget for capturing IP messages, and the captured data can be saved from the file system or imported into the PC via the rdb tool, parsed with wireshark software.



### 1.1. Dependence

- Dependence on [optparse](https://github.com/liu2guang/optparse) package
- Dependence on [dfs](https://www.rt-thread.org/document/site/rtthread-development-guide/rtthread-manual-doc/zh/1chapters/12-chapter_filesystem/) file system
- Dependence on [env](https://www.rt-thread.org/document/site/rtthread-development-guide/rtthread-tool-manual/env/env-user-manual/) tool
- RT-Thread 3.0+, no dependence on BSP
  

### 1.2. Obtain Method
Enable tcpdump using menuconfig, as follows: 

```
  RT-Thread online packages --->
      IOT internet of things --->
          [*] netutils: Networking utilities for RT-Thread  --->
          [*]   Enable tcpdump tool
          [ ]     Enable tcpdump data to print on the console
          [*]     Enable tcpdump debug log output
```

After saving the menuconfig configuration, download the package using the `pkgs --update` command

> Note: Debug information is not recommended to close



## 2. Usage



### 2.1. tcpdump Command Meaning

```
-i: Specify the network interface for listening
-m: Select save mode (file system or rdb)
-w: user-specified file name xx.pcap
-p: Stop capturing packet
-h: Help message
```



### 2.2. Command Specification

```
msh />tcpdump -h

|>------------------------- help -------------------------<|
| tcpdump [-p] [-h] [-i interface] [-m mode] [-w file]     |
|                                                          |
| -h: help                                                 |
| -i: specify the network interface for listening          |
| -m: choose what mode(file-system or rdb) to save the file|
| -w: write the captured packets into an xx.pcap file      |
| -p: stop capturing packets                               |
|                                                          |
| e.g.:                                                    |
| specify network interface and select save mode \         |
| and specify filename                                     |
| tcpdump -ie0 -mfile -wtext.pcap                          |
| tcpdump -ie0 -mrdb -wtext.pcap                           |
|                                                          |
| -m: file-system mode                                     |
| tcpdump -mfile                                           |
|                                                          |
| -m: rdb mode                                             |
| tcpdump -mrdb                                            |
|                                                          |
| -w: file                                                 |
| tcpdump -wtext.pcap                                      |
|                                                          |
| -p: stop                                                 |
| tcpdump -p                                               |
|                                                          |
| -h: help                                                 |
| tcpdump -h                                               |
|                                                          |
| write commands but no arguments are illegal!!            |
| e.g.: tcpdump -i / -i -mfile  / -i -mfile -wtext.pcap    |
|>------------------------- help -------------------------<|

msh />
```



## 3. Save the Capture Packet Using the File System

> Here we mount the sd-card to the file system



### 3.1. Preparation Before Capturing Packet

Insert sd-card before powering up on the development board

- If the mount succeeds, the prompt will be: 

```
SD card capacity 31023104 KB
probe mmcsd block device!
found part[0], begin: 10485760, size: 29.580GB
File System initialized!
```

- If the mount fails, the prompt will be:

```
sdcard init fail or timeout: -2!
```

- The mount succeeds and the `sd0` device can be seen after entering`list_device` , which is shown as follow:

```
msh />list_device
device         type         ref count
------ -------------------- ---------
sd0    Block Device         1       
e0     Network Interface    0             
usbd   USB Slave Device     0                   
rtc    RTC                  1       
spi4   SPI Bus              0       
pin    Miscellaneous Device 0       
uart1  Character Device     3       
msh />
```



### 3.2. Check Before Capturing Packet

> Please confirm the IP address of the board before capturing packet

Enter `ifconfig` in msh />, as follows:

```
msh />
network interface: e0 (Default)
MTU: 1500
MAC: 00 04 9f 05 44 e5 
FLAGS: UP LINK_UP ETHARP BROADCAST
ip address: 192.168.1.137
gw address: 192.168.1.1
net mask  : 255.255.255.0
dns server #0: 192.168.1.1
dns server #1: 0.0.0.0
msh />
```



### 3.3. Start Capturing Packet

Enter `tcpdump -ie0 -mfile -wtext.pcap `In msh />, as follows:

```
msh />tcpdump -ie0 -msd -wtext.pcap
[TCPDUMP]select  [e0] network card device
[TCPDUMP]select  [file-system] mode
[TCPDUMP]save in [text.pcap]
[TCPDUMP]tcpdump start!
msh />
```

- Using the capturing packet command creates a thread with a priority of 12.
- Enter the  `list_thread`  command to view the running thread with the name  `tdth`, as follows:

```
thread   pri  status      sp     stack size max used left tick  error
-------- ---  ------- ---------- ----------  ------  ---------- ---
tdth      12  suspend 0x000000ac 0x00000800    08%   0x0000000a 000
tshell    20  ready   0x00000070 0x00001000    22%   0x00000003 000
rp80       8  suspend 0x0000009c 0x00000400    15%   0x0000000a 000
phy       30  suspend 0x00000070 0x00000200    28%   0x00000001 000
usbd       8  suspend 0x00000098 0x00001000    03%   0x00000014 000
tcpip     10  suspend 0x000000b4 0x00000400    39%   0x00000014 000
etx       12  suspend 0x00000084 0x00000400    12%   0x00000010 000
erx       12  suspend 0x00000084 0x00000400    34%   0x00000010 000
mmcsd_de  22  suspend 0x0000008c 0x00000400    49%   0x00000013 000
tidle     31  ready   0x00000054 0x00000100    32%   0x0000001a 000
main      10  suspend 0x00000064 0x00000800    35%   0x00000010 000
msh />
```



### 3.4. Capture Packet Test

> Using the  [ping ](https://github.com/RT-Thread-packages/netutils/blob/master/ping/README.md) command for the capture packet test, the `ping`  command needs to be enabled in the menuconfig configuration, as follows:

```
  RT-Thread online packages --->
      IOT internet of things --->
          [*] Enable Ping utility
```

After saving the menuconfig configuration, download the package using the  `pkgs --update` command



#### 3.4.1. ping  Domain Name

Enter  `ping rt-thread.org` in msh/>, as follows:

```
msh />ping rt-thread.org
60 bytes from 116.62.244.242 icmp_seq=0 ttl=49 time=11 ticks
60 bytes from 116.62.244.242 icmp_seq=1 ttl=49 time=10 ticks
60 bytes from 116.62.244.242 icmp_seq=2 ttl=49 time=12 ticks
60 bytes from 116.62.244.242 icmp_seq=3 ttl=49 time=10 ticks
msh />
```



#### 3.4.2. ping IP

Enter  `ping 192.168.1.121` in msh />, as follows:

```
msh />ping 192.168.1.121
60 bytes from 192.168.10.121 icmp_seq=0 ttl=64 time=5 ticks
60 bytes from 192.168.10.121 icmp_seq=1 ttl=64 time=1 ticks
60 bytes from 192.168.10.121 icmp_seq=2 ttl=64 time=2 ticks
60 bytes from 192.168.10.121 icmp_seq=3 ttl=64 time=3 ticks
msh />
```



### 3.5. Stop Capturing Packet

Enter `tcpdump -p` in msh />, as follows:

```
msh />tcpdump -p
[TCPDUMP]tcpdump stop and tcpdump thread exit!
msh />
```



### 3.6. Check Results

Enter  `ls` in msh />  to see the results of the save, as follows:

```
msh />ls
Directory /:
System Volume Information<DIR>                    
text.pcap         1012                     
msh />
```



### 3.7. After Capturing Packet

Copy xx.pcap files saved in sd-card to a PC using a card reader and use the packet-captured software wireshark to analyze the network flow directly.



## 4. Capture Packet Files Import PC Via rdb Tool 



### 4.1. Start Capturing Packet


Enter  `tcpdump -ie0 -mrdb -wtext.pcap` in msh />, as follows:

```
msh />tcpdump -ie0 -mrdb -wtext.pcap
[TCPDUMP]select  [e0] network card device
[TCPDUMP]select  [rdb] mode
[TCPDUMP]save in [text.pcap]
[TCPDUMP]tcpdump start!
msh />
```



### 4.2. Capture Packet Test

Please refer to 3.4



### 4.3. Stop Capturing Packet

Enter  `tcpdump -p` in msh />, as follows:

```
msh />tcpdump -p
[TCPDUMP]tcpdump stop and tcpdump thread exit!
msh />
```



### 4.4. Check Results

Enter  `ls` in msh />  to see the results of the save, as follows:

```
msh />ls
Directory /:
System Volume Information<DIR>                    
text.pcap         1012                     
msh />
```

### 4.5. After Capturing Packet

Import xx.pcap files to a PC using the rdb tool and use the packet-captured software wireshark to analyze the network flow directly.



## 5. Notes

- The tcpdump tool needs to turn on the sending and receiving thread of lwip.
- Enter`tcpdump -p` can end the packet capture.



## 6. Contact

* Thanks to [liu2guang](https://github.com/liu2guang)  for creating optprase package
* Thanks to [uestczyh222](https://github.com/uestczyh222) for making the rdb tool and rdb upper computer
* Maintained by [never](https://github.com/neverxie)
* Github：https://github.com/RT-Thread-packages/netutils
