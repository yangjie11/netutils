# TFTP: Simple File Transfer Protocol

## 1. Introduction


TFTP is a protocol in the TCP/IP protocol family for simple file transfer between the client and the server, providing a non-complex and inexpensive file transfer service with a port number of 69, which is much lighter than the traditional FTP protocol and is suitable for small embedded products.

RT-Thread currently supports TFTP servers.

## 2. Usage

### 2.1 Install TFTP Client

The installation file is located in  `/tools/Tftpd64-4.60-setup.exe` , before using TFTP, install this software first.

### 2.2 Start TFTP Server

Before transferring files, you need to use the Finsh/MSH command on RT-Thread to start the TFTP server, the effect is shown as follow:

```
msh />tftp_server
TFTP server start successfully.
msh />
```

### 2.3 Transfer File

Open the `Tftpd64` software you just installed and configure it as follows: 

- Select `Tftp Client` ;
- In the  `Server interfaces`  drop-down box, be sure to select a network card that is in the same segment as RT-Thread;
- Fill in the IP address of the TFTP server. You can view it under RT-Thread's MSH using the 'ifconfig' command;
- Fill in TFTP server port number, which is defaulted to  `69` ；

![tftpd_cfg](../images/tftpd_cfg.png)

#### 2.3.1 Send File to RT-Thread

- In the `Tftpd64` software, select the file to send;

- `Remote File` is the server-side path to save files (including file names), and the options support relative and absolute paths. Because RT-Thread turns on the `DFS_USING_WORKDIR` option by default, the relative path is based on the directory that Finsh/MSH is currently in. Therefore, when using the relative path, it is important to switch the directory in advance;
- Click on `Put` button.

As shown in the following image, the file is sent to the directory that Finsh/MSH is currently in, using the Relative Path here:

![tftpd_put](../images/tftpd_put.png)

> Note: If  `DFS_USING_WORKDIR` is not turned on and `Remote File` is empty, the file is saved under the root path

#### 2.3.2 Receive File from RT-Thread

- In the `Tftpd64` software, fill in the saved file path for receiving (including file name);
- `Remote File` is the server-side file path wait for receiving (including the file name), the option supports the relative path and the absolute path. Because RT-Thread turns on the  `DFS_USING_WORKDIR` option by default, the relative path is based on the directory that Finsh/MSH is currently in. Therefore, when using the relative path, it is important to switch the directory in advance;
- Click on `Get`  button.

Save `/web_root/image.jpg` to your local area, as shown below, using the Absolute Path here :

```
msh /web_root>ls           ##Check if file exists
Directory /web_root:
image.jpg           10559                    
msh /web_root>
```

![tftpd_get](../images/tftpd_get.png)

## 3. FAQ

### 3.1 Put File to Micro-controller, the program enters hardfault :

hardfault log is similar as follows:

```
msh />psr: 0x61000000
 pc: 0x08009030
 lr: 0x08009031
r12: 0x00000000
r03: 0x20000208
r02: 0x20000208
r01: 0x2000fa44
r00: 0x00000000
hard fault on thread: tcpip
thread pri  status      sp     stack size max used left tick  error
------ ---  ------- ---------- ----------  ------  ---------- ---
tshell  20  suspend 0x00000140 0x00001000    07%   0x00000006 000
tcpip   10  close   0xfffffd0f 0x0000004b    100%   0x2000f9bf 000
etx     12  ready   0x00000098 0x00000400    14%   0x00000010 000
erx     12  ready   0x000000e0 0x00000400    53%   0x00000010 000
tidle   31  ready   0x00000048 0x00000100    34%   0x0000001c 000
```

You can see that the tcpip thread has a max used metric of 100%, indicating that its stack has overflowed. It is also possible that the line of information on the tcpip thread is garbled, due to the overflow of its thread stack.

The tcpip default configuration in lwIP is at:

`RT-Thread Components` → `Network stack` → `light weight TCP/IP stack` → ` (1024) the stack size of lwIP thread`

This option defaults to '1024' and is recommended to change to '2048'.