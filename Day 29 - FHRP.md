
## First Hop Redundancy Protocols

![[Pasted image 20260328132445.png]]

> A first hop redundancy protocol (FHRP) is a computer networking protocol which is designed to protect the default gateway used on a subnetwork by allowing two or more routers to provide backup for that address; in the even of failure of an active router, the backup router will take over the address, usually within a few seconds.


![[Pasted image 20260328132734.png]]

![[Pasted image 20260328132819.png]]

![[Pasted image 20260328132830.png]]

![[Pasted image 20260328132848.png]]

![[Pasted image 20260328132903.png]]

![[Pasted image 20260328132924.png]]

![[Pasted image 20260328133003.png]]

![[Pasted image 20260328133023.png]]

> Gratuitous ARP: ARP replies sent without being requested (no ARP request message was received).
> *the frames are broadcast to FFFF.FFFF.FFFF (normal ARP replies are unicast)*

![[Pasted image 20260328133218.png]]

![[Pasted image 20260328133236.png]]

>FHRPs are 'non-preemptive'. The current active router will not automatically give up its role, even if the former active router returns.
>*you can change this setting to make R1 'preempt' R2 and take back it's active role automatically*


- A virtual IP is configured on the two routers, and a virtual MAC is generated for the virtual IP (each FHRP uses a different format for the virtual MAC)
- An active router and a standby router are elected. (different FHRPs use different terms)
- End hosts in the network are configured to use the virtual IP as their default gateway.
- The active router replies to ARP requests using the virtual MAC address, so traffic destined for other networks will be sent to it.
- If the active router fails, the standby becomes the next active router. The new active router will send gratuitous ARP messages so that switches will update their MAC address tables. It now functions as the default gateway.
- If the old active router comes back online, by default it won’t take back its role as the active router. It will become the standby router.
- You can configure ‘preemption’, so that the old active router does take back its old role.

## HSRP (Hot Standby Router Protocol)

- Cisco proprietary.
- An active and standby router are elected.
   
- There are two versions: version 1 and version 2.  
    Version 2 adds IPv6 support and increases the number of groups that can be configured.
   
- Multicast IPv4 address:  
    v1 = 224.0.0.2  
    v2 = 224.0.0.102
   
- Virtual MAC address:  
    v1 = 0000.0c07.acXX (XX = HSRP group number)  
    v2 = 0000.0c9f.fXXX (XXX = HSRP group number)
   
- In a situation with multiple subnets/VLANs, you can configure a different active router in each subnet/VLAN to load balance.

![[Pasted image 20260328133740.png]]


## VRRP (Virtual Router Redundancy Protocol)

- Open standard
- A master and backup router are elected.
- Multicast IPv4 address: 224.0.0.18
- Virtual MAC address: 0000.5e00.01XX (XX = VRRP group number)
- In a situation with multiple subnets/VLANs, you can configure a different master router in each subnet/VLAN to load balance.

>HSRP can only support IPv4 while VRRP supports both version 4 and 6.

![[Pasted image 20260328133906.png]]

## GLBP (Gateway Load Balancing Protocol)

- Cisco proprietary
- Load balances among multiple routers within a single subnet
- An AVG (Active Virtual Gateway) is elected.
- Up to four AVFs (Active Virtual Forwarders) are assigned by the AVG (the AVG itself can be an AVF, too)
- Each AVF acts as the default gateway for a portion of the hosts in the subnet.
- Multicast IPv4 address: 224.0.0.102
- Virtual MAC address: 0007.b400.XXYY (XX = GLBP group number, YY = AVF number)

![[Pasted image 20260328134142.png]]


## Configuring HSRP

![[Pasted image 20260328134221.png]]

![[Pasted image 20260328134234.png]]

![[Pasted image 20260328134255.png]]

- The active router is determined in this order:
1. Highest priority (default 100)
	for eg. in 200 and 100.. router with priority 200 will become active.
2. Highest IP address

>Preempt causes the router to take the role of active router, even if another router already has the role.

![[Pasted image 20260328134428.png]]

>HSRP version 1 and 2 are not compatible. If R1 uses version 2, R2 must use version 2 also.

![[Pasted image 20260328134523.png]]

![[Pasted image 20260328134546.png]]


# **Quiz**

![[Pasted image 20260328134636.png]]

![[Pasted image 20260328134655.png]]

![[Pasted image 20260328134708.png]]

![[Pasted image 20260328134722.png]]

![[Pasted image 20260328134753.png]]

![[Pasted image 20260328134810.png]]  