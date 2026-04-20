## Static NAT

- Static NAT involves statistically configuring one-to-one mappings of private IP addresses to public IP addresses.
- When traffic from the internal host is sent to the outside network, the router will translate the source address.

![[Pasted image 20260325174208.png]]

- However, this one-to-one mapping also allows external hosts to access the internal host via the inside global address. 

![[Pasted image 20260325175048.png]]


## Dynamic NAT 

- In dynamic NAT, the router dynamically maps inside local addresses to inside global addresses as needed.

- An ACL is used to identify which traffic should be translated.
	- If the source IP is permitted by the ACL, the source IP will be translated.
	- If the source IP is denied by the ACL, the source IP will not be translated. "**the traffic will NOT be dropped!**"

- A NAT pool is used to define the available inside global addresses.

![[Pasted image 20260325175454.png]]

![[Pasted image 20260325175517.png]]

- Although they are dynamically assigned, the mappings are still one-to-one (one inside local IP address per inside global IP address).
- If they aren't enough inside global IP addresses available (=all are currently being used), it is called 'NAT pool exhaustion'.
	- If a packet from another inside host arrives and needs NAT but there are no available addresses, the router will drop the packet.
	- The host will be unable to access outside networks until one of the inside global IP addresses becomes available.
- Dynamic NAT entries will time out automatically if not used, or you can clear them manually.

## NAT Pool Exhaustion

![[Pasted image 20260325180008.png]]

![[Pasted image 20260325180036.png]]

## Dynamic NAT Configuration

![[Pasted image 20260325180128.png]]

![[Pasted image 20260325180156.png]]

![[Pasted image 20260325180215.png]]

## PAT (NAT Overload)

- PAT (aka NAT Overload) translates both the IP address and the port number (if necessary).
- By using a unique port number for each communication flow, a single public IP address can be used by many different internal hosts. (port number are 16 bits = over 65,000 available port numbers).
- The router will keep track of which inside local address is using which inside global address and port.
- Because many inside hosts can share a single public IP, PAT is very useful for preserving public IP addresses, and it is used in networks all over the world.

![[Pasted image 20260325181425.png]]

## PAT Configuration (pool)

![[Pasted image 20260325181504.png]]

![[Pasted image 20260325181527.png]]

### PAT Configuration (interface)

![[Pasted image 20260325181603.png]]

![[Pasted image 20260325181622.png]]


## Command Review

![[Pasted image 20260325181650.png]]


# **Quiz**

![[Pasted image 20260326115744.png]]

![[Pasted image 20260326115808.png]]

![[Pasted image 20260326115823.png]]

![[Pasted image 20260326115842.png]]

![[Pasted image 20260326115855.png]]

![[Pasted image 20260326115917.png]]