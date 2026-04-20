## Network Topology 

![[Pasted image 20260407203509.png]]

![[Pasted image 20260407203422.png]]


![[Pasted image 20260407203541.png]]

## Switch Configuration 

![[Pasted image 20260407204258.png]]

![[Pasted image 20260407204236.png]]

![[Pasted image 20260407204346.png]]

## WLC Initial Setup

![[Pasted image 20260407204615.png]]

![[Pasted image 20260407204634.png]]

![[Pasted image 20260407204658.png]]

![[Pasted image 20260407204721.png]]


## Accessing the GUI

![[Pasted image 20260407204808.png]]

- Login with the username/password you made earlier while setting up the WLC.

![[Pasted image 20260407205556.png]]

## WLC Configuration

![[Pasted image 20260407205628.png]]

### WLC Port/Interface 

- WLC ports are the physical ports that cables connect to.
- WLC interfaces are the logical interfaces within the WLC (ie. SVIs on a switch).
- WLCs have a few different kinds of ports:
	- Service port: A dedicated management port. Used for out-of-band management. Must connect to a switch access port because it only supports one VLAN. This port can be used to connect to the device while it is booting, perform system recovery, etc.
	- Distribution system port: These are the standard network ports that connect to the ‘distribution system’ (wired network) and are used for data traffic. These ports usually connect to switch trunk ports, and if multiple distribution ports are used they can form a LAG.
	- Console port: This is a standard console port, either RJ45 or USB.
	- Redundancy port: This port is used to connect to another WLC to form a high availability (HA) pair.


![[Pasted image 20260407205914.png]]

![[Pasted image 20260407205931.png]]

![[Pasted image 20260407205942.png]]

WLCs have a few different kinds of interfaces:
- Management interface: Used for management traffic such as Telnet, SSH, HTTP, HTTPS, RADIUS authentication, NTP, Syslog, etc. CAPWAP tunnels are also formed to/from the WLC’s management interface.
- Redundancy management interface: When two WLCs are connected by their redundancy ports, one WLC is ‘active’ and the other is ‘standby’. This interface can be used to connect to and manage the ‘standby’ WLC.
- Virtual interface: This interface is used when communicating with wireless clients to relay DHCP requests, perform client web authentication, etc.
- Service port interface: If the service port is used, this interface is bound to it and used for out-of-band management.
- Dynamic interface: These are the interfaces used to map a WLAN to a VLAN. For example, traffic from the ‘Internal’ WLAN will be sent to the wired network from the WLC’s ‘Internal’ dynamic interface.

## WLC Configuration 

![[Pasted image 20260407210155.png]]


![[Pasted image 20260407210216.png]]

![[Pasted image 20260407210240.png]]

![[Pasted image 20260407210400.png]]

![[Pasted image 20260407210436.png]]

![[Pasted image 20260407210533.png]]

![[Pasted image 20260407210616.png]]



![[Pasted image 20260407210655.png]]

![[Pasted image 20260407210732.png]]

![[Pasted image 20260407210802.png]]

![[Pasted image 20260407210840.png]]

- Web Authentication: After the wireless clients gets an IP address and tries to access a web page, they will have to enter a username and password to authenticate.
- Web Passthrough: Similar to the above, but no username or password are required. A warning or statement is displayed and the client simply has to agree to gain access to the Internet.
- The Conditional and Splash Page web redirect options are similar, but additionally require 802.1X layer 2 authentication.

![[Pasted image 20260407211108.png]]

![[Pasted image 20260407211130.png]]

![[Pasted image 20260407211158.png]]

![[Pasted image 20260407211214.png]]

- Then apply after you are done with all your configuration.

![[Pasted image 20260407211308.png]]

![[Pasted image 20260407211432.png]]

![[Pasted image 20260407211445.png]]

![[Pasted image 20260407211502.png]]

![[Pasted image 20260407211515.png]]

![[Pasted image 20260407211532.png]]

![[Pasted image 20260407211553.png]]

 ![[Pasted image 20260407211702.png]]

![[Pasted image 20260407211719.png]]


![[Pasted image 20260407211844.png]]

![[Pasted image 20260407211858.png]]

![[Pasted image 20260407211959.png]]

![[Pasted image 20260407212110.png]]

![[Pasted image 20260407212124.png]]

![[Pasted image 20260407212140.png]]

![[Pasted image 20260407212158.png]]

![[Pasted image 20260407212219.png]]

![[Pasted image 20260407212239.png]]

![[Pasted image 20260407212412.png]]

![[Pasted image 20260407212437.png]]


# **Quiz**

![[Pasted image 20260407212538.png]]

![[Pasted image 20260407212550.png]]

![[Pasted image 20260407212603.png]]

![[Pasted image 20260407212618.png]]

![[Pasted image 20260407212636.png]]

![[Pasted image 20260407212713.png]]





