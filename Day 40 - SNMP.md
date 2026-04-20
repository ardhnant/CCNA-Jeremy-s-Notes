## Simple Network Management Protocol

- SNMP is an industry-standard framework and protocol that was originally released in 1988. 
- RFC 1065 - Structure and identification of management information that was originally for TCP/IP based internets.
- RFC 1066 - Management information base for network management of TCP/IP based internets 
- RFC 1067 - A simple network management protocol

- Don't let the 'Simple' in the name fool you!
- SNMP can be used to monitor the status of devices, make configuration changes, etc.
- There are two main types of devices in SNMP
	1. Managed Devices
		- These are the devices being managed using SNMP.
		- For example, network devices like routers and switches.
	2. Network Management Station (NMS)
		- This device(s) managing devices.
		- This is the SNMP 'server'

## SNMP Operations

- There are three main operations used in SNMP.
1. Managed devices can notify the NMS of events.

![[Pasted image 20260320161902.png]]

2. The NMS cas ask the managed devices for information about their current status.

![[Pasted image 20260320162731.png]]

3. The NMS can tell the managed devices to change aspects of hir configuration.
![[Pasted image 20260320162822.png]]

## SNMP Components

![[Pasted image 20260320162855.png]]



- The **SNMP Manager** is the software on the NMS that interact with the managed devices. 
	- It receives notifications, send requests for information, sends configuration changes, etc.
- The SNMP Application provides an interface for the network admin to interact with. 
	- Display alerts, statistics, charts, etc.

![[Pasted image 20260320163124.png]]

- The **SNMP Agent** is the SNMP software running on the managed devices that interacts with the SNMP Manager on the NMS.
	- It sends notification to/receives messages from the NMS.
- The **Management Information Base (MIB)** is the structure that contains the variables that are managed by SNMP.
	- Each variable is identified with an Object ID (OID).
	- Example variables: Interface status, traffic throughout, CPU usage, temperature, etc.

![[Pasted image 20260320163702.png]]

## SNMP OIDs

![[Pasted image 20260320163844.png]]

![[Pasted image 20260320163853.png]]

## SNMP Versions 

- Many versions of SNMP have been proposed/developed, however only three major versions have achieved wide-spread use:
**SNMPv1**
- The original versions of SNMP

**SNMPv2c**
- Allows the NMS to retrieve large amounts of information in a single request, so it is more efficient.
- 'c' refers to the 'community strings' used as passwords in SNMPv1, removed from SNMPv2, and then added back for SNMPv2c.

**SNMPv3** 
- A much more secure version of SNMP that supports strong encryption and authentication. Whenever possible, this version should be used!

## SNMP Messages

![[Pasted image 20260320164541.png]]

### SNMP 'Read' Messages

**Get** 
- A request sent from the manager to the agnt to retrieve the value of the variable (OID), or multiple variables. The agent will send a Response message with the current value of each variable.

**GetNext**
- A request sent form the manager to the agent to discover the available variables in the MIB.

**GetBulk** 
- A more efficient versions of the GetNext message (introduced in SNMPv2)

![[Pasted image 20260320164921.png]]

### SNMP 'Write' Messages

**Set** 
- A request sent from the manager to the agent to change the value of one or more variables. The agent will send a Response message with the new values.

![[Pasted image 20260320165102.png]]


### SNMP 'Notification' Messages

**Trap**
- A notification is sent to the manager, The manager does not send a Response messages to acknowledge that it received the Trap, so these messages are 'unreliable'.

**Inform**
- A notification messages that is acknowledged with a Response message.
- Originally used for communications between managers, but later update allow agents to send Inform messages to managers, too.

>SNMP Agent = UDP 161
>SNMP Manager = UDP 162


## SNMPv2c Configuration 

![[Pasted image 20260320165630.png]]

![[Pasted image 20260320165801.png]]

- In SNMPv1 and SNMPv2c, there is no encryption. The community and message contents are sent in plain-text. This is not secure, as the packets can easily be captured and read.

## QUIZ 

![[Pasted image 20260320165935.png]]

![[Pasted image 20260320165951.png]]

![[Pasted image 20260320170007.png]]

![[Pasted image 20260320170023.png]]

![[Pasted image 20260320170044.png]]

![[Pasted image 20260320170112.png]]