# Ping

## 1、Introduction

Ping is a network tool used to test whether packets can reach a specific host through IP protocol. The packet loss rate (packet loss rate) and round trip time (round trip delay time) between the host and the host are estimated.

## 2、Usage

Ping supports access to IP address or domain name, and tests with FinSH/MSH command.  The result is similar to the following:

### 2.1 Ping domain name

```
msh />ping rt-thread.org
60 bytes from 116.62.244.242 icmp_seq=0 ttl=49 time=11 ms
60 bytes from 116.62.244.242 icmp_seq=1 ttl=49 time=10 ms
60 bytes from 116.62.244.242 icmp_seq=2 ttl=49 time=12 ms
60 bytes from 116.62.244.242 icmp_seq=3 ttl=49 time=10 ms
msh />
```

### 2.2 Ping IP

```
msh />ping 192.168.10.12
60 bytes from 192.168.10.12 icmp_seq=0 ttl=64 time=5 ms
60 bytes from 192.168.10.12 icmp_seq=1 ttl=64 time=1 ms
60 bytes from 192.168.10.12 icmp_seq=2 ttl=64 time=2 ms
60 bytes from 192.168.10.12 icmp_seq=3 ttl=64 time=3 ms
msh />

```
