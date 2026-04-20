## The Purpose of DHCP 

- DHCP is used to allow hosts to automatically learn various aspects of their network configuration, such as IP address, subnet mask, default gateway, DNS server, etc, without manual/static configuration.
- It is an essential part of modern networks.
	- When you connect a phone/laptop to WIFI, do you ask the network admin which IP address, subnet mask, default gateway, etc, the phone/laptop should use?
- Typically used for 'client devices' such as workstation (PCs), phones, etc.
- Devices such as routers, servers, etc, are usually manually configured.
- In small networks (such as home networks the router typically acts as the DHCP server for hosts in the LAN).
- In larger networks, the DHCP server is usually a Windows/Linux server.

![[Pasted image 20260320154222.png]]

`ipconfig /all`

- This PC was previously assigned this IP by the DHCP server, so it asked to receive the same address again this time.
- DHCP server 'lease' IP address to clients. These leases are usually not permanent, and the client must give up the address at the end of the lease.

## DHCP Release

![[Pasted image 20260320154439.png]]

`ipconfig /release`

![[Pasted image 20260320154606.png]]

- The DHCP Ack message can be either broadcast or unicast.
- 
>DHCP servers use UDP 67.
>DHCP clients use UDP 68.


![[Pasted image 20260320154706.png]]

`ipconfig /renew`

- This is used to get a new IP via DHCP.

![[Pasted image 20260320154858.png]]

## DHCP D-O-R-A

![[Pasted image 20260320154941.png]]

![[Pasted image 20260320155002.png]]


## DHCP Relay

- Some network engineers might choose to configure each router to act as the DHCP server for its connected LANs.
- However, large enterprises often choose to use a centralized DHCP server.
- If the server is centralized, it won't receive the DHCP client's broadcast DHCP messages. (broadcast messages don't leave the local subnet)
- To fix this, you can configure aa router to act as a **DHCP relay agent.**
- The router will forward the client's broadcast DHCP messages to the remote DHCP server as unicast messages.

![[Pasted image 20260320155459.png]]

![[Pasted image 20260320155522.png]]

## DHCP Server Configuration in IOS

![[Pasted image 20260320155546.png]]

![[Pasted image 20260320155702.png]]

![[Pasted image 20260320155718.png]]

![[Pasted image 20260320155821.png]]

- `helper-address` command must be activated in the broadcast domain, not on the interface towards the DHCP server.

![[Pasted image 20260320155954.png]]

![[Pasted image 20260320155836.png]]

## Command Summary

### Windows

`ipconfig /release`
`ipconfig /renew`

### IOS

**DHCP server**
`ip dhcp excluded-address {low-address} {high-address}`

`ip dhcp pool {pool-name}`

`netowork ip-addres {/prefix-length | subnet-mask}`

`dns-server <ip address>`

`lease {day hours minutes | infinite}`

`show ip dhcp binding`

**DHCP relay agent**
`ip helper-address <ip address>`

**DHCP client**
`ip address dhcp`


# **Quiz**

![[Pasted image 20260320160730.png]]

![[Pasted image 20260320160741.png]]

![[Pasted image 20260320160756.png]]

![[Pasted image 20260320160811.png]]

![[Pasted image 20260320160826.png]]

![[Pasted image 20260320160842.png]]

















