
## The importance of time

- All devices have an internal clock (routers, switches, your PC, etc)
- In Cisco IOS, you can view the time with the `show clock` command.
![[Pasted image 20260319085542.png]]

>The default time zone is UTC (coordinated universal time)

- If you use the `show clock detail` command, you can see the time source.
![[Pasted image 20260319085704.png]]

> '*   = time is not considered authoritative
>The hardware calendar is the default time source.

- The internal hardware clock of a device will drift over time, so it is not the ideal time souce.
- From a CCNA perspective, the most important reason to have accurate time on a device is to have accurate logs for troubleshooting.
- Syslog, the protocol used to keep device logs, will covered in a later video.

## `show logging`

![[Pasted image 20260319090108.png]]

![[Pasted image 20260319090122.png]]

## Manual time configuration 

- You can manually configure the time on the device with clock set command.
![[Pasted image 20260319090218.png]]

- Although the hardware calendar (built-in clock) is the defult time source, the hardware clock and software clock are separate and can be configure separately.

## Hardware Clock (Calendar) Configuration 

- You can manually configure the hardware clock with the calendar set command.
![[Pasted image 20260319090439.png]]

- Typically you will want to synchronize the 'clock' and 'calendar'.
- Use the command `clock update-calendar` to sync the calendar to the clock's time.
- Use the command `clock read-calendar` to sync the clock to the calendar's time.

![[Pasted image 20260319090705.png]]


## Configuring the Time Zone

- You can configure the time zone with `clock timezone` command.

![[Pasted image 20260319091049.png]]


## All the commands yet

- `show clock`
- `show clock detail`
- `clock set hh:mm:ss {day|month} {month|day} year`
- `show calendar`
- `calendar set hh:mm:ss {day|month} {month|day} year`
- `clock timezone *name* <hours-offset> [minutes-offset] `


## Network Time Protocol

- Manually configuring the time on devices is not scalable.
- The manually configured clocks will drift, resulting in inaccurate time.
- NTP (Network Time Protocol) allows automatic syncing of time over a network.
 - NTP clients request the time from NTP servers.
 - A device can be an NTP server and an NTP client at the same time.
 - NTP allows accuracy of time within ~1 millisecond it the NTP server is in the same LAN, or within ~ 50 millisecond if connecting to the NTP server over a WAN/the Internet.
 - Some NTP servers are 'better' than other. The 'distance' of an NTP server from the original **reference clock** is called **stratum**

- NTP uses UDP port 123 to communicate.

## Reference Clocks

- A reference clock is usually a very accurate time device like an atomic clock or a GPS clock Reference clocks are stratum 0 within the NTP hierarchy.
- NTP servers directly connected to reference clocks are stratum 1.

![[Pasted image 20260319092608.png]]

![[Pasted image 20260319093034.png]]

- Reference clocks are stratum 0.
- Stratum 1 NTP servers get their time from reference clocks.
- Stratum 2 NTP servers get their time from stratum 1 NTP servers.
- Stratum 3 NTP servers get their time from stratum 2 NTP servers.
- Stratum 15 is the maximum. Anything above that is considered unreliable.
- Devices can also 'peer' with devices at the same stratum to provide more accurate time.

- An NTP client can sync to multiple NTP servers.
- NTP servers which get their time directly from reference clocks are also called primary servers.
- NTP servers which get their time from other NTP servers are called secondary servers. They operate in the server mode and client mode at time same time.

## NTP Configuration 

![[Pasted image 20260319124251.png]]


![[Pasted image 20260319124302.png]]

- To make a preferred server 
![[Pasted image 20260319124340.png]]


![[Pasted image 20260319124402.png]]

- * = current server in use.

![[Pasted image 20260319124453.png]]

- Stratum 2 because R1 is synchronizing its time to Google's NTP servers, it automatically becomes an NTP server itself (stratum level 1 higher than Google's NTP servers). Now other devices can synchronize their time to R1.

![[Pasted image 20260319124643.png]]

- NTP uses only UTC time zone. You must configure the appropriate time zone on each device.
- Configures the router to update the hardware clock (calendar) with the time learned via NTP.
- The hardware clock tracks the date and time on the device even if it restarts, power is lost etc. When the system is restarted the hardware clock is used to initialize the software clock.

![[Pasted image 20260319124915.png]]

![[Pasted image 20260319124926.png]]

![[Pasted image 20260319124951.png]]

- Server with low stratum are preferred.

## Configuring NTP server mode

![[Pasted image 20260319125037.png]]

![[Pasted image 20260319125104.png]]

![[Pasted image 20260319125113.png]]

> The default stratum of the NTP master command is 8.

## Configuring NTP symmetric active mode

![[Pasted image 20260319125411.png]]

![[Pasted image 20260319125421.png]]

## Configuring NTP Authentication 

- NTP authentication can be configured, although it is optional.
- It allows NTP clients to ensure they only sync to the intended servers.
- To configure NTP authentication:
`ntp authenitcate` - Enable authentication 
`ntp authentication-key <key-number> md5 <key>` - Create NTP authentication key(s)
`ntp trusted-key <key-number>` - Specify the trusted key(s)
`ntp server <ip addr> key <key number>` - Specify which key to use for the server. This command isn't needed on the server (R1).

![[Pasted image 20260319125904.png]]

![[Pasted image 20260319125920.png]]


### NTP Command Summary

![[Pasted image 20260319130027.png]]


# **Quiz**

![[Pasted image 20260319130135.png]]

![[Pasted image 20260319130147.png]]

![[Pasted image 20260319130254.png]]

![[Pasted image 20260319130400.png]]





> For the 4th question.. stratum on the server becomes 1 less then what is in the command for server.. the stratum configured on the R1 will be for the primary client.









