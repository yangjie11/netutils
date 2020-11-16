# iperf：Tools for testing network bandwidth

## 1、Introduction

Iperf is a tool for testing network performance. Iperf can test the maximum TCP and UDP bandwidth performance, has a variety of parameters and UDP characteristics, can be adjusted as needed, and can report bandwidth, delay jitter and packet loss.

## 2、Usage

Iperf uses the master-slave architecture, that is, one end is the server and the other end is the client. The iperf component package provided by us implements the TCP server mode and the client mode, and does not support UDP testing temporarily. The following will specifically explain the use of the two modes.

### 2.1 iperf Server mode

#### 2.1.1 Get IP address

You need to use the FinSH/MSH command on RT-Thread to obtain the IP address. The result is similar to the following:

```
msh />ifconfig
network interface: e0 (Default)
MTU: 1500
MAC: 00 04 9f 05 44 e5 
FLAGS: UP LINK_UP ETHARP
ip address: 192.168.12.71
gw address: 192.168.10.1
net mask  : 255.255.0.0
dns server #0: 192.168.10.1
dns server #1: 223.5.5.5
```

- Record the obtained IP address 192.168.12.71 (record according to the actual situation)

#### 2.1.2 Start iperf server

You need to use the FinSH/MSH command on RT thread to start the iperf server. The result is similar to the following:

```
msh />iperf -s -p 5001
```

- `-s` means start as server
- `-p` indicates listening to port 5001

#### 2.1.3 Install jperf test software

The installation file is located in `/tools/jperf.rar`, which is a green software. The installation is actually a decompression process. Just unzip it to a new folder.

#### 2.1.4 jperf test

Open the jperf.bat software and configure it as follows:

- Select client mode
- Enter the IP address 192.168.12.71 just obtained (fill in according to the actual address)
- Change port number to 5001
- Click `Run lperf!` To start the test
- Wait for the test to finish. During the test, the test data will be displayed on the shell interface and JPerf software.

![iperfs](./images/iperfs.png)

### 2.2 iperf Client mode

#### 2.2.1 Get the IP address of the PC

Use the ipconfig command in the command prompt window of the PC to obtain the IP address of the PC, and record that the IP address of the PC is 192.168.12.45 (recorded according to the actual situation).

#### 2.2.2 Install jperf test software

The installation file is located in `/tools/jperf.rar`, which is a green software. The installation is actually a decompression process. Just unzip it to a new folder.

#### 2.2.3 Turn on the jperf server

Open the `jperf.bat` software and configure it as follows:

- Select server mode

- Modify port number to 5001

- Click `Run lperf!`  to open the server

#### 2.2.4 Start iperf client

You need to use the FinSH/MSH command on RT thread to start the iperf client. The result is similar to the following:

```
msh />iperf -c 192.168.12.45 -p 5001
```

- `-c` means to start as a client, followed by the IP address of the PC running the server

- `-p` for port 5001

- Wait for the test to finish. During the test, the test data will be displayed on the shell interface and JPerf software.


![iperfc](../images/iperfc.png)

 

