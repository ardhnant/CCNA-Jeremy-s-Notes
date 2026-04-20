- DHCP snooping is a security feature of switches that is used to filter DHCP messages received on untrusted ports.
- DHCP snooping only filters messages. Non-DHCP messages aren't affected.
- All ports are untrusted by default.
	- Usually, uplink ports are configured as trusted ports and downlink port remain as untrusted.

![[Pasted image 20260328101102.png]]

![[Pasted image 20260328101135.png]]

## DHCP Starvation 

- An example of a DHCP-based attack is a DHCP starvation attack.
- An attacker uses spoofed MAC addresses to flood DHCP Discover messages.
- The target server's DHCP pool becomes full, resulting in a denial-of-service to other devices.

![[Pasted image 20260328101629.png]]


## DHCP Poisoning (Man in the middle attack)

• Similar to ARP Poisoning, DHCP Poisoning can be used to perform a Man-in-the-Middle attack.  
• A spurious DHCP server replies to clients’ DHCP Discover messages and assigns them IP addresses, but makes the client use the spurious server’s IP as the default gateway.  
*Clients usually accept the first OFFER message they receive.* 
• This will cause the client to send traffic to the attacker instead of the legitimate default gateway.  
• The attacker can then examine/modify the traffic before forwarding it to the legitimate default gateway.

![[Pasted image 20260328101842.png]]

then 

![[Pasted image 20260328101902.png]]

then 

![[Pasted image 20260328101921.png]]

![[Pasted image 20260328101936.png]]

## DHCP Messages

- When DHCP Snooping filters messages, differentiate between DHCP servers messages and DHCP clients messages.

- Messages sent by DHCP Servers:
	- OFFER
	- ACK
	- NAK = Opposite of ACK, used to decline a client's REQUEST

- Messages sent by DHCP Clients:
	- DISCOVER
	- REQUEST
	- RELEASE = Used to tell the server that the client no longer needs it's IP address
	- DECLINE = Used to decline the IP address offered by DHCP server

## How the DHCP Snooping Table is Formed

The table is built dynamically by inspecting DHCP traffic passing through the switch.

#### Step-by-step process:

1. **Client sends DHCPDISCOVER (broadcast)**
    - Arrives on an **untrusted port** (access port where clients connect).
2. **Switch forwards request**
    - The switch allows DHCP client messages from untrusted ports.
3. **DHCP server sends DHCPOFFER / DHCPACK**
    - These messages must arrive on a **trusted port** (uplink toward DHCP server).
4. **Switch validates DHCP response**
    - The switch checks that the DHCP reply is coming from a trusted interface.
    - If it arrives on an untrusted port, it is dropped.
5. **Binding creation**
    - When a valid **DHCPACK** is seen, the switch extracts:
        - Client MAC address
        - Assigned IP address
        - VLAN
        - Incoming port (where client is connected)
        - Lease time
    - A new entry is added to the DHCP snooping table.
6. **Entry maintenance**
    - Entries expire when the lease time ends.
    - They are updated if the client renews the lease.

## DHCP Snooping Operations

- If a DHCP message is received on a **trusted port**, forward it as normal without inspection.
- If a DHCP message is received on an **untrusted port**, inspect it and act as follows:  
    - If it is a **DHCP Server** message, discard it.  
	
    - If it is a **DHCP Client** message, perform the following checks:
		 **DISCOVER/REQUEST messages:** Check if the frame’s source MAC address and the DHCP message’s CHADDR fields match. Match = forward, mismatch = discard
		 **RELEASE/DECLINE messages:** Check if the packet’s source IP address and the receiving interface match the entry in the DHCP Snooping Binding Table. Match = forward, mismatch = discard
  
- When a client successfully leases an IP address from a server, create a new entry in the DHCP Snooping Binding Table.

![[Pasted image 20260328105121.png]]

![[Pasted image 20260328105137.png]]


### DHCP Snooping Rate-Limiting

- DHCP snooping can limit the rate at which DHCP messages are allowed to enter an interface.
- If the rate of DHCP messages crosses the configured limit, the interface is err-disabled.
- Like with Port Security, the interface can be manually re-enabled, or automatically re-enabled with errdisable recovery.

![[Pasted image 20260328105245.png]]

![[Pasted image 20260328105308.png]]


>Rate-Limiting can be very useful to protect against DHCP exhaustion attacks.


### DHCP Options 82 (Information Option)

- Option 82, also known as the 'DHCP relay agent information option' is one of many DHCP options.
- It provides additional information about which DHCP relay agent received the client's message, on which interface, in which VLAN, etc.
- DHCP relay agents can add Option 82 to messages they forward to the remote DHCP server. With DHCP snooping enabled, by default Cisco switches will add Option 82 to DHCP messages they receive from clients, even if the switch isn't acting as a DHCP relay agent.
- By default, Cisco switches will drop DHCP messages with Option 82 that are received on an untrusted port.

![[Pasted image 20260328131946.png]]

![[Pasted image 20260328132001.png]]

![[Pasted image 20260328132016.png]]

![[Pasted image 20260328132030.png]]

## Command Review

![[Pasted image 20260328132051.png]]


# **Quiz**

![[Pasted image 20260328132121.png]]

![[Pasted image 20260328132131.png]]

![[Pasted image 20260328132142.png]]

![[Pasted image 20260328132154.png]]

![[Pasted image 20260328132204.png]]

![[Pasted image 20260328132218.png]]



















