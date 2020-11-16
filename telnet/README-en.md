# Telnet: Remote Login RT-Thread

## 1. Introduction


The [Telnet](https://baike.baidu.com/item/Telnet) protocol is an application layer protocol that is used in the Internet and the Local Area Network, using the form of a virtual terminal, to provide two-way, text string-based interaction. It belongs to the TCP/IP protocol families and it is a standard protocol and primary method of the Internet remote login service, often used for remote control of the web server, which allows the user to run the work on the remote host to the local host.

RT-Thread supports Telnet server, and when the Telnet client is successfully connected, it will be remotely connected to the device's Finsh/MSH for remote control of the device.



## 2. Usage



### 2.1 Start Telnet Server

You need to use the Finsh/MSH command on RT-Thread to start the Telnet server, the effect is shown as follow: 

```
msh />telnet_server
Telnet server start successfully
telnet: waiting for connection
msh />
```



### 2.2 Remote Login Telnet Server

Install terminal that supports Telnet clients on your local computer, such as putty, Xshell, SecureCRT, we'll use putty for demonstration here:

- Open putty, select `Telnet` mode;
- Enter the Telnet server address, click Open;
- After the connection is successful, RT-Thread Finsh/MSH can be used in the terminal;

![telnet_connect_cfg](../images/telnet_connect_cfg.png)

![telnet_connected](../images/telnet_connected.png)

### 2.3 Notes

- The device's local Finsh/MSH will not be available after a successful connection to the Telnet server. If you need to use it, disconnect the Telnet client;

- Telnet does not support auto-complete shortcut key of  `TAB` , and history look-up shortcut key of `Up`/`Down`;
- Currently the Telnet server only supports connecting to one client.

