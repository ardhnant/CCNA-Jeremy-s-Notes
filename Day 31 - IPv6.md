## What about IPv5?

- 'Internet Stream Protocol' was developed in the late 1970s, but never actually introduced for public use.
- It was never called 'IPv5', but it used a value of 5 in the Version field of the IP header.
- So, when the successor to IPv4 was being developed, it was named IPv6.

## Hexadecimal

- Binary / Base 2 / 0b 
	eg. 0b10

- Decimal / Base 10 / 0d
	eg. 0,1,2,3,4,5,6,7,8,9

- Hexadecimal / Base 16 / 0x
	eg. 0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F

![[Pasted image 20260311214236.png]]

![[Pasted image 20260311214253.png]]

![[Pasted image 20260311214638.png]]

![[Pasted image 20260311214742.png]]

![[Pasted image 20260311214831.png]]

![[Pasted image 20260311214940.png]]


## Why IPv6?

- The main reason is that **three simply aren't enough IPv4 address available!**
- There are 4,294,967,296 (2^32) IPv4 address available.
- When IPv4 was being designed 30 years ago, the creator had no idea the Internet would be as large as it is today.
- VLSM, private IPv4 addresses, and NAT have been used to conserve the IPv4 address space. 
- Those are shor-term solutions.
- The long term solution is IPv6.

 - IPv4 address assignments are controlled by IANA (Inter Assigned Numbers Authority) 
 - IANA distributes IPv4 address space to various RIRs (Regional Internet Registries), which then assign them to companies that need them.

![[Pasted image 20260312121602.png]]

>On 24 September 2015 ARIN declared exhaustion of the ARIN IPv4 addresses pool.
>On 21 August 2020, LACNIC announced that it had made its final IPv4 allocation.


- An IPv6 address is **128 bits**.
- 4_the bits of an IPv4 address = 4_the number of possible addresses? **NO**
- Every additional bit **doubles** the number of possible addresses.
- There are 340,282,366,920,938,463,463,374,607,431,768,211,456 IPv6 addresses.  
	There are .................................................. **4,294,967,296** IPv4 addresses.
 

## Shortening (abbreviation) IPv6 addresses


![[Pasted image 20260312122110.png]]

![[Pasted image 20260312122132.png]]

![[Pasted image 20260312122152.png]]


## Finding the IPv6 prefix (global unicast addresses)

- Typically, an enterprise requesting IPv6 addresses from their ISP will receive a /48 block.
- Typically, IPv6 subnets use a /64 prefix length.
- That means an enterprise has 16 bits to use to make subnets.
- The remaining 64 bits can be used for hosts.

![[Pasted image 20260312122412.png]]


![[Pasted image 20260312122442.png]]

![[Pasted image 20260312122457.png]]

## Configuring IPv6 addresses 

![[Pasted image 20260312124606.png]]

![[Pasted image 20260312124707.png]]

![[Pasted image 20260312124623.png]]

- You must run the command `ipv6 unicast-routing` in order to configure ipv6 addresses.


# **Quiz**

![[Pasted image 20260312124856.png]]

![[Pasted image 20260312125000.png]]






















































































