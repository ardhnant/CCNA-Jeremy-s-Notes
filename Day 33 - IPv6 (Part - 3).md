## IPv6 Address Representation 

- An RFC (Request for Comments) is a publication from the ISOC (Internet Society) and associated organization like the IETF (Internet Engineering Task Force), and are the official documents of Internet specification, protocols, procedures, etc.
- RFC 5952 is '**A Recommendation for IPv6 Address Text Representation**'.
- Before this RFC, IPv6 address representation was more flexible 
	- You could remove leading 0s, or leave them
	- You could replace all-0 quaters with ::, or leave them
	- You could use upper-case 0xA, B, C, D, E, F or lower-case 0xa, b, c, d, e, f
- RFC 5962 suggests standardizing IPv6 address representation

## IPv6 address Representation 

- Leading 0s MUST removed 
	- 2001:0db8:0000:0000:0001:0f2a:4fff:fea3:00b1
						to 
	- 2001:db8::1:f2a:4fff:fea3:b1

- :: MUST be used to shorten the longest string of all 0 quaterts 
	- 2001:0000.0000.0000.0f2a:0000.0000:00b1
						to 
	- 2001::f2a:0:0:b1

- If there are two equal-length choices for the ::, use :: to shorten the one on the left.
	- 2001:0db8:0000:0000:0f2a:0000:0000:00b1
						to
	- 2001:db8::f2a:0:0:b1

- Hexadecimal characters (a,b,c,d,e,f) MUST be written using lower case, NOT upper-case(A,B,C,D,E,F)

>But even the Cisco devices write IPv6 in capital letter.. so take all of them with pinch of salt.. these are industry standard which are better way to represent IPv6 and recommended, doesn't mean the other way is wrong.


## IPv6 Header

![[Pasted image 20260313131718.png]]

**Version**
- Length - 4 bits
- Indicate the version of IP that is used.
- Fixed value of 6 (0b0110) to indicate IPv6.

**Traffic Class**
- Length - 8 bits
- Used for QoS (Quality of Services), to indicate high-priority traffic.
- For example IP phone traffic, live video calls, etc, will have a Traffic Class value which gives priority over other traffic.

**Flow Label**
- Length - 20 bits
- Used to identify specific traffic 'flows' (communications between a specific source and destination).

**Next Header**
- Length - 8 bits
- Indicate the type of the 'next header' (header of the encapsulated segment), for example TCP or UDP.

**Hop Limit**
- Length - 8 bits
- The value in this field is decremented by 1 by each router that forwards it. If it reaches 0, the packet is discarded.
- Same function as the IPv4 header's 'TTL' field.

**Source/Destination**
- Length - 128 bits each
- These fields contain the IPv6 addresses to the packet's source and the packet's intended destination.

## Solicited-Node Multicast Address

- An IPv6 solicited-node multicast address.

![[Pasted image 20260316153335.png]]

- To make a solicited node add ff02::1:ff to the last of destination IPv6 address. 
- For example the IP address `2001:db8::ef8:22ff:fe36:8500` with the mask of /64 
	- The last 6 hex digits are `36:8500` which we will add in `ff02::1:ff`
- This will work as a multicast address to find the destination device.

### Neighbor Discovery Protocol

- Neighbor Discovery Protocol (NDP) is a protocol used with IPv6.
- It has various function, and one of those function is to replace ARP, which is no longer used in IPv6.
- The ARP-like function of NDP uses ICMPv6 and solicited-node multicast addresses to learn the MAC address of other hosts.
(ARP in IPv4 uses broadcast messages)

- Two message types are used: 
	1) Neighbor Solicitation (NS) = ICMPv6 Type 135
	2) Neighbor Advertisement (NA) = ICMPv6 Type 136

![[Pasted image 20260316155057.png]]

![[Pasted image 20260316155113.png]]

## Neighbor Discovery Protocol 

- Another function of NDP allows hosts to automatically discover routers on the local network.
- Two messages are used for this process:
	1) Router Solicitation (RS) = ICMPv6 Type 133
		- Sent to multicast address ff02::2 (all routers).
		- Asks all routers on the local link to identify themselves.
		- Sent when an interface is enabled/host is connected to the network.
	2) Router Advertisement (RA) = ICMPv6 Type 134
		- Sent to multicast address ff02::1 (all nodes).
		- The router announces its presence, as well as other information about the link.
		- These messages are sent in response to RS messages.
		- They are also sent periodically, even if the router hasn't received on RS.

![[Pasted image 20260316161337.png]]

## SLAAC 

- Stands for **Stateless Address Auto-configuration**.
- Hosts use the RS/RA messages to learn the IPv6 prefix of the local link (ie. 2001:db8::/64), and then automatically generate an IPv6 address.
- Using the ipv6 address *prefix/prefix-length* eui-64 command, you need to manually enter the prefix.
- Using the `ipv6 address autoconfig` command, you don't need to enter the prefix. The device used NDP to learn the prefix used on the local link.
- The devices will use EUI-64 to generate the interface ID, or it will be randomly generated (depending on the device/maker).

![[Pasted image 20260316162151.png]]

## Duplicate Address Detection (DAD)

- One final point about NDP!
- Duplicate Address Detection (DAD) allows hosts to check if other devices on the local link the using the same IPv6 address.
- Any time an IPv6-enabled interface initializes (no shutdown command), or an IPv6 address is configured on an interface (by any method: manual, SLAAC, etc.), it performs DAD.
- DAD uses two messages you learned earlier: NS and NA.
- The host will send an NS to its own IPv6 address. If it doesn't get a reply, it knows the address is unique.
- If it gets a reply, it means another host on the network is already using the address.

## IPv6 Static Routing

- IPv6 routing works the same as IPv4 routing.
- However, the two processes are separate on the router, and the two routing tables are separate as well.
- IPv4 routing is enabled by default.
- IPv6 routing is disabled by default, and must be enabled with `ipv6 unicast-routing`.
- If IPv6 routing is disabled, the router will be able to send and receive IPv6 traffic, but will not router IPv6 traffic (=will not forward it between networks).

![[Pasted image 20260316164118.png]]

- A connected network route is automatically added for each connected network.
- A local host route is automatically added for each address configured on the router.
- Routes for link-local addresses are not added to the routing table.

![[Pasted image 20260316164430.png]]

## IPv6 Static Routing 

`ipv6 route destination/prefix-length {next-hop | exit-interface [next-hop]} [ad]`

- Directly attached static route: Only the exit interface is specified. ipv6 router destination/prefix-length exit-interface `R1(config)#ipv6 route 2001:db8:0:3::/64 g0/0`

>In IPv6, you CAN'T use directly attached static routes if the interface is an Ethernet interface.


- **Recursive** stating route: Only the next hop is specified.
	`ipv6 route destination/prefix-length next-hop`
	- eg - ipv6 route 2001:db8:0:3::/64 2001:db8:0:12::2

- **Fully specified** static route: Both the exit interface and next hop are specified. 
	`ipv6 route destination/prefix-length exit-interface next-hop`
	- eg - `ipv6 route 2001:db8:0:3::/64 g0/0 2001:db8:0:12::2`

![[Pasted image 20260316170333.png]]

## Link-Local Next-Hops

![[Pasted image 20260316170449.png]]

# **Quiz**

![[Pasted image 20260316170523.png]]


![[Pasted image 20260316170543.png]]

![[Pasted image 20260316170601.png]]

![[Pasted image 20260316170619.png]]

![[Pasted image 20260316170644.png]]

![[Pasted image 20260316170659.png]]