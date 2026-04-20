## Private IPv4 Addresses (RFC 1918)

- IPv4 doesn’t provide enough addresses for all devices that need an IP address in the modern world.
- The long-term solution is to switch to IPv6.
- There are three main short-term solutions:
	1. CIDR
	2. Private IPv4 addresses
	3. NAT
- RFC 1918 specifies the following IPv4 address ranges as private:

10.0.0.0/8 (10.0.0.0 to 10.255.255.255) → Class A  
172.16.0.0/12 (172.16.0.0 to 172.31.255.255) → Class B  
192.168.0.0/16 (192.168.0.0 to 192.168.255.255) → Class C

- RFC 1918 specifies the following IPv4 address ranges as private:
	- 10.0.0.0/8 (10.0.0.0 to 10.255.255.255)
	- 172.16.0.0/12 (172.16.0.0 to 172.31.255.255)
	- 192.168.0.0/16 (192.168.0.0 to 192.168.255.255)
- You are free to use these addresses in your networks. They don't have to be globally unique.

>Private IP address cannot be used over the Internet

**Two problems:**
1. Duplicate addresses
2. Private IP addresses can't be used over the Internet, so the PCs can't access the Internet

![[Pasted image 20260325094621.png]]


## Network Address Translation (NAT)

- Network Address Translation (NAT) is used to modify the source and/or destination IP addresses of packets.
- There are various reasons to use NAT, but the most common reason is to allow hosts with private IP addresses to communicate with other hosts over the Internet.

![[Pasted image 20260325094834.png]]


### Static NAT

- Static NAT involves statically configuring one-to-one mappings of private IP addresses to public IP addresses.

- An inside local IP address is mapped to an inside global IP address.  
    - Inside Local = The IP address of the inside host, from the perspective of the local network  
    *the IP address actually configured on the inside host, usually a private address
    
    - Inside Global = The IP address of the inside host, from the perspective of outside hosts  
    *the IP address of the inside host after NAT, usually a public address
	
	- Outside Local = The IP address of the outside host, from the perspective of the local network
	
	- Outside Global = The IP address of the outside host, from the perspective of the outside network

![[Pasted image 20260325095454.png]]

>Static NAT allows devices with private IP addresses to communicate over the Internet. However, because it requires a one-to-one IP address mapping, it doesn't help preserve IP addresses.


## Static NAT Configuration 

![[Pasted image 20260325095728.png]]

![[Pasted image 20260325095801.png]]

### `show ip nat translations` 

![[Pasted image 20260325100110.png]]

> Unless **destination NAT** is used, these two addresses will be the same.

### `show ip nat statistics`

![[Pasted image 20260325100232.png]]

## Command Review

![[Pasted image 20260325100308.png]]


# **Quiz**

![[Pasted image 20260325100333.png]]

![[Pasted image 20260325100344.png]]

![[Pasted image 20260325100405.png]]

![[Pasted image 20260325100428.png]]

![[Pasted image 20260325100447.png]]

![[Pasted image 20260325100513.png]]
