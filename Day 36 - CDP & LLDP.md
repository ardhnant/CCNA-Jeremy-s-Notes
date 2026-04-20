## Layer 2 Discovery Protocols

- Layer 2 discovery protocols such as CDP and LLDP share information with and discover information about neighboring (connected) devices.
- The shared information includes host name, IP address, device type, etc.
- CDP is a Cisco proprietary protocol.
- LLDP is an industry standard protocol (IEEE 802.1AB).
- Because they share information about the devices in the network, they can be considered a security risk and are often not used. It is up to the network engineer/admin to decide if they want to use them in the network or not.

![[Pasted image 20260407193212.png]]


## Cisco Discovery Protocol

- CDP is a Cisco proprietary protocol.
- It is enabled on Cisco devices (routers, switches, firewalls, IP phones, etc) by default.
- CDP messages are periodically sent to multicast MAC address 0100.0CCC.CCCC.
- When a device receives a CDP message, it processes and discards the message. It does NOT forward it to other devices.
- By default, CDP messages are sent once every 60 seconds.
- By default, the CDP holdtime is 180 seconds. If a message isn’t received from a neighbor for 180 seconds, the neighbor is removed from the CDP neighbor table.
- CDPv2 messages are sent by default.

![[Pasted image 20260407193740.png]]

![[Pasted image 20260407193753.png]]

![[Pasted image 20260407193805.png]]

![[Pasted image 20260407193839.png]]

![[Pasted image 20260407193905.png]]

![[Pasted image 20260407193936.png]]

## CDP Configuration Commands 

![[Pasted image 20260407194005.png]]

## Link Layer Discovery Protocol

- LLDP is an industry standard protocol (IEEE 802.1AB).
- It is usually disabled on Cisco devices by default, so it must be manually enabled.
- A device can run CDP and LLDP at the same time.
- LLDP messages are periodically sent to multicast MAC address 0180.C200.000E.
- When a device receives an LLDP message, it processes and discards the message. It does NOT forward it to other devices.
- By default, LLDP messages are sent once every 30 seconds.
- By default, the LLDP holdtime is 120 seconds.
- LLDP has an additional timer called the ‘reinitialization delay’. If LLDP is enabled (globally or on an interface), this timer will delay the actual initialization of LLDP. 2 seconds by default.

## LLDP Configuration Commands 

![[Pasted image 20260407194140.png]]

## Link Layer Discovery Protocol

![[Pasted image 20260407194221.png]]

![[Pasted image 20260407194236.png]]

![[Pasted image 20260407194256.png]]

![[Pasted image 20260407194305.png]]

![[Pasted image 20260407194332.png]]

## LLDP `show` command summary

![[Pasted image 20260407194409.png]]

![[Pasted image 20260407194427.png]]

![[Pasted image 20260407194441.png]]

![[Pasted image 20260407194458.png]]

![[Pasted image 20260407194521.png]]

![[Pasted image 20260407194542.png]]

![[Pasted image 20260407194601.png]]

