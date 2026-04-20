## ARP Review

- ARP is used to learn the MAC address of another device with a known IP address.
- For example, a PC will use ARP to learn the MAC address of it's default gateway to communicate with external networks.
- Typically it is a two message exchange: request and reply

![[Pasted image 20260330130851.png]]

![[Pasted image 20260330131326.png]]

![[Pasted image 20260330131401.png]]

![[Pasted image 20260330131439.png]]


## Gratuitous ARP 

- A Gratuitous ARP message is an ARP reply that is sent without receiving an ARP request.
- It is sent to the broadcast MAC address.
- It allows other devices to learn the MAC address of the sending device without having to send ARP requests.
- Some devices automatically send GARP messages when an interface is enables, IP address is changed, etc.

![[Pasted image 20260330131716.png]]

## Dynamic ARP Inspection 

- DAI is a security feature of switches that is used to filter ARP messages received on untrusted ports.
- DAI only filters ARP messages that aren't affected.
- All ports are untrusted by default.
	- Typically, all ports connected to other network devices (switches, routers) should be configured as trusted, while interfaces connected to end hosts should remain untrusted.

![[Pasted image 20260330190030.png]]

![[Pasted image 20260330190101.png]]


## ARP Poisoning (Man in the middle)

- Similar to DHCP poisoning, ARP poisoning involves an attacker manipulating target's ARP tables so traffic is sent to the attacker.
- To do this, the attacker can send gratuitous ARP messages using another devices's IP address.
- Other devices in the network will receive the GARP and update their ARP tables, causing them to send traffic to the attacker instead of the legitimate destination.

![[Pasted image 20260330190450.png]]

![[Pasted image 20260330190538.png]]

## Dynamic ARP Inspection Operation 

- DAI inspects the sender MAC and sender IP fields of ARP messages received on untrusted ports and checks that if there is a matching entry in the DHCP snooping binding table
	- If there is a matching entry, the ARP message is forwarded normally
	- If there isn’t a matching entry, the ARP message is discarded

![[Pasted image 20260330190800.png]]

- DAI doesn’t inspect messages received on trusted ports. They are forwarded as normal
- ARP ACLs can be manually configured to map IP addresses and MAC addresses for DAI to check
	- Useful for hosts that don’t use DHCP

- DAI can be configured to perform more in-depth checks also, but these are optional

- Like DHCP snooping, DAI also supports rate-limiting to prevent attackers from overwhelming the switch with ARP messages
	- DHCP snooping and DAI both require work from the switch’s CPU
	- Even if the attacker’s messages are blocked, they can overload the switch CPU with ARP messages

## DAI Configuration 

![[Pasted image 20260330191004.png]]

![[Pasted image 20260330191028.png]]

- DHCP snooping requires two commands to enable  it:
	`ip dhcp snooping`
	`ip dhcp snooping vlan <vlan number>`

- DAI only requires one:
	`ip arp inspection vlan <vlan number>`

## `show ip arp inspection interfaces`

![[Pasted image 20260330191255.png]]

- DAI rate limiting is enabled on untrusted ports by default with a rate of 15 packets per second.
- It is disabled on trusted ports by default. DHCP snooping rate limiting is disabled on all interfaces by default.


- DHCP snooping rate limiting is configured like this: x packets per second.
- The DAI burst interval allows you to configure rate limiting like this: x packets per  y seconds.

### DAI Rate Limiting 

![[Pasted image 20260330191849.png]]

## DAI Optional Checks

![[Pasted image 20260330191932.png]]

- **dst-mac**: Enables validation of the destination MAC address in the Ethernet header against the target MAC address in the ARP body for ARP responses. The device classifies packets with different MAC addresses as invalid and drops them
    
- **ip**: Enables validation of the ARP body for invalid and unexpected IP addresses. Addresses include 0.0.0.0, 255.255.255.255, and all IP multicast addresses. The device checks the sender IP addresses in all ARP requests and responses. The device checks the target IP addresses only in ARP responses.
    
- **src-mac**: Enables validation of the source MAC address in the Ethernet header against the sender MAC address in the ARP body for ARP requests and responses. The device classifies packets with different MAC addresses as invalid and drops them

![[Pasted image 20260330192304.png]]

## ARP ACLs

![[Pasted image 20260330192349.png]]

![[Pasted image 20260330192413.png]]


## Command Review 

![[Pasted image 20260330192437.png]]