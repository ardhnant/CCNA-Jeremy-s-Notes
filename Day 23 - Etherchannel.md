## What is EtherChannel?


![[Pasted image 20260306201941.png]]

('A' in ASW1 means Access layer switch 1 and 'D' in DSW1 means Distribution layer switch 1)

> Access Layer switch are the one connected to end hosts and Distribution layer switch are the one connected to other access layer switch.

- The connection between ASW1 and DSW1 is congested. More links are needed to increase the bandwidth so it can support all of the end hosts.

![[Pasted image 20260306202434.png]]

- If you connect two switches together with multiple links, all except one will be disabled by spanning tree.
- If all of ASW1 and DSW1, leading to broadcast storms.
- Other links will be unused unless the active link fails. In that case, one of the inactive links will start forwarding.

>When the bandwidth of the interfaces connected to end hosts is greater that the bandwidth of the connection to the distribution switch(es), this is called **oversubscription**. Some oversubscription is acceptable, but too much will cause congestion.


### The solution

![[Pasted image 20260306202708.png]]

- EtherChannel groups multiple interfaces together to act as a single interface.
- STP will treat this group as a single interface.

- A circle encircling all the cable which are connected towards ports symbolize EtherChannel.

> Traffic using the EtherChannel will be load balanced among the physical interfaces in the group. An algorithm is used to determine which traffic will use which physical interface. 

- Some other names of EtherChannel are:
	1. Port Channel
	2. LAG (Link Aggregation Group)


## EtherChannel Load-Balancing 

![[Pasted image 20260306203205.png]]

- EtherChannel load balances based on 'flows'.
- A flow is a communication between two nodes in the network.
- Frames in the same flow will be forwarded using the same physical interface.
- If frames in the same flow were forwarded using different physical interfaces, some frames may arrive at the destination out of order, which can cause problems.

![[Pasted image 20260306203439.png]]

- You can change the inputs used in the interface selection calculation.
- Inputs that can be used:
	1. Source MAC
	2. Destination MAC
	3. Source and Destination MAC
	4. Source IP
	5. Destination IP
	6. Source and Destination IP

### Command Line


![[Pasted image 20260306203817.png]]

![[Pasted image 20260306203851.png]]

![[Pasted image 20260306203909.png]]

> To view the etherchannel we use 'etherchannel' while to configure the etherchannel we use 'port-channel'

`show etherchannel load-balance`
`port-channel load-balance method`

## EtherChannel Configuration 

- There are three methods of EtherChannel configuration on Cisco switches:

	- PAgP (Port Aggregation Protocol)
		- Cisco proprietary protocol	
		- Dynamically negotiates the creation/maintenance of the EtherChannel. (like DTP does for trunks)

	- LACP (Link Aggregation Control Protocol)
		- Industry standard protocol (IEEE 802.3ad)
		- Dynamically negotiates the creation/maintenance of the EtherChannel. (like DTP does for trunks)

	- Static EtherChannel 
		- A protocol isn't used to determine if an EtherChannel should be formed.
		- Interfaces are statically configured to form and EtherChannel.

> Up to 8 interfaces can be formed into a single EtherChannel (LACP allows up to 16, but only 8 will be active, the other will be in standby mode, waiting for an active interface to fail)


## PAgP Configuration 

![[Pasted image 20260306205230.png]]

![[Pasted image 20260306205258.png]]

auto + auto = no EtherChannel 
desirable + auto = EtherChannel
desirable + desirable = EtherChannel

**Command**: channel-group _number_ mode _mode_

![[Pasted image 20260306205555.png]]

> The channel-group number has to match for member interfaces on the same switch. However, it **doesn't** have to match the channel-group number on the other switch.(channel-group 1 on ASW1 can form an EtherChannel with channel-group 2 on DSW1)


## LACP Configuration 


![[Pasted image 20260306205815.png]]

passive + passive = no EtherChannel 
active + passive = EtherChannel
active + active = EtherChannel

## Static EtherChannel Configuration

![[Pasted image 20260306205943.png]]

- On mode only works with on mode (on + desirable or on + active will not work)

## Manually configure the Negotiation Protocol

![[Pasted image 20260306210108.png]]

![[Pasted image 20260306210132.png]]

- Member interfaces must have matching configurations.  
	- Same duplex (full/half)  
	- Same speed  
	- Same switchport mode (access/trunk)  
	- Same allowed VLANs/native VLAN (for trunk interfaces)

- If an interface’s configurations do not match the others, it will be excluded from the EtherChannel.

## `show etherchannel summary`

![[Pasted image 20260306210424.png]]

![[Pasted image 20260306210454.png]]


## `show etherchannel port-channel`

![[Pasted image 20260306210550.png]]


![[Pasted image 20260306210610.png]]

![[Pasted image 20260306210631.png]]


## Layer 3 EtherChannel configuration

![[Pasted image 20260306210828.png]]

![[Pasted image 20260306210855.png]]

![[Pasted image 20260306210912.png]]






































