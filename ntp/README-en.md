# NTP: Network Time Protocol

## 1. Introduction

NTP is the Network Time Protocol, which is a protocol used to synchronize the time of individual computers on a network.

The NTP client is implemented on RT-Thread, and once connected to the network, you can get the current UTC time and update it to the RTC.



## 2. Usage

### 2.1 Access UTC Time

UTC Time also known as World Uniform Time, World Standard Time, International Coordination Time. Take Beijing time as example, it is UTC plus 8 hours, 8 hours more than UTC time, or understood to be 8 hours earlier.

API: `time_t ntp_get_time(void)`

|Parameter                                    |Description|
|:-----                                  |:----|
|return                                  |`>0`: Current UTC Time，`=0`: Fail to access time|


Sample code：

```C
#include <ntp.h>

void main(void)
{
    time_t cur_time;

    cur_time = ntp_get_time();
    
    if (cur_time)
    {
        rt_kprintf("NTP Server Time: %s", ctime((const time_t*) &cur_time));
    }
}
```



### 2.2 Access Your Local Time

The current time zone can be set in `menuconfig`, which is currently defaulted to  `8`

API: `time_t ntp_get_local_time(void)`

|Parameter                                    |Description|
|:-----                                  |:----|
|return                                  |`>0`: Current Local Time，`=0`：Fail to access time|

The API is used in a similar way to `ntp_get_time()` 



### 2.3 Synchronize Local Time to RTC

If you turn on the RTC device, you can also use the command and API as shown below to synchronize  NTP's local time to the RTC device.

The result of Finsh/MSH command is shown as follow:

```
msh />ntp_sync
Get local time from NTP server: Sat Feb 10 15:22:33 2018
The system time is updated. Timezone is 8.
msh />
```

API: `time_t ntp_sync_to_rtc(void)`

|Parameter                                    |Description|
|:-----                                  |:----|
|return                                  |`>0`: Current Local Time，`=0`: Fail to sync time|



## 3. Notes

- Using the NTP API to execute will occupy more thread stacks, ensure that the stack space（≥1.5K） is sufficient when used;

- NTP API method does not support reentrant , and when using it concurrently, be aware of the lock.

  

