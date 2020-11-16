# NetIO: Network Throughput Testing Tool

## 1. Introduction

[NetIO](http://www.nwlab.net/art/netio/netio.html) is a tool for testing network performance on OS/2 2.x, Windows, Linux, and Unix. It tests net network throughput using data packets of different sizes in the TCP/UDP approach.

RT-Thread currently supports NetIO TCP servers.


## 2. Usage

### 2.1 Start NetIO Server

You need to use the Finsh/MSH command on RT-Thread to start the NetIO server, the result is shown as follow:

```
msh />netio_init
NetIO server start successfully
msh />
```



### 2.2 Install NetIO-GUI Testing Software

The installation file is located in `/tools/netio-gui_v1.0.4_portable.exe`, which is a green software, and the installation is actually a decompression process, just decompress the file into a new folder.


### 2.3  Test NetIO

Open the `NetIO-GUI` software you just installed and configure it as follows:

- Open `NetIO-GUI.exe` ;
- Select the`Client-Mode`, 'TCP' protocol;
- Fill in the IP address of the NetIO server. You can view it under RT-Thread's MSHusing the ifconfig command;
- Click on `Start measure`to start the test (make sure that the server can be pinged through the PC before testing);
- Wait for the test to end. After then, the receiving and sending test results for different data packets will be displayed in the result area.

![netio_tested](../images/netio_tested.png)
