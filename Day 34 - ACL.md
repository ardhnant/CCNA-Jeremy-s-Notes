## What are ACLs?

- ACLs (Access Control Lists) have multiple uses.
- ACL functions as a packet filter, instructing the router to permit or discard specific traffic.
- ACLs can filter traffic based on source/destination IP address, source/destination Layer 4 ports, etc.
- Requirement:
	- Hosts in 192.168.1.0/24 can access the 10.0.1.0/24 network.
	- Hosts in 192.168.2.0/24 cannot access the 10.0.1.0/24 network.
- ACLs are configured globally on the router.
- They are an ordered sequence of ACEs (Access Control Entries)

---
**ACL 1**
1. if source IP = 192.168.1.0/24, then permit
2. if source IP = 192.168.2.0/24, then deny
3. if source IP = any, then permit
---


![[Pasted image 20260317133716.png]]


- Configuring an ACL in global config mode will not make the ACL take effect.
- The ACL must be applied to an interface
- ACLs are applied either inbound or outbound.

**Requirements:**
- 192.168.1.0/24 can access 10.0.1.0/24
- 192.168.2.0/24 can't access 10.0.1.0/24 

![[Pasted image 20260317134521.png]]

- ACLs are made up of one or more ACEs.
- When the router checks a packet against the ACL, it processes the ACEs in order, from top to bottom.
- If the packet matches one of the ACEs in the ACL, the router the action and stops processing the ACL. All entries below the matching entry will be ignored.

> A maximum of one ACL can be applied to a single interface per direction. 
> **Inbound**: Maximum one ACL. **Outbound**: Maximum one ACL

## Implicit deny

- What will happen if a packet doesn't match any of the entries in an ACL?

![[Pasted image 20260317134908.png]]

--- 
**ACL 2:** 
1. if source IP = 192.168.1.0/24
2. if source IP = 192.168.0.0/16
3. (if source IP = any, then deny)
---

- There is an 'implicit deny' at the end all ACLs
- The implicit deny tells the router to deny all traffic that doesn't match any of the conigured entries in the ACL.

## ACL Types 

**Standard ACLs:** Match based on the source ip address only.
- Standard Numbered ACLs
- Standard Named ACLs

**Extended ACLs:** Match based on Source/Destination IP, Source/Destination port, etc.
- Extended Numbered ACLs
- Extended Named ACLs

## Standard Numbered ACLs

- Standard ACLs match traffic based only on the source IP address of the packet.
- Numbered ACLs are identified with a number (ie. ACL 1, ACL 2, etc)
- Different types of ACLs have different range of numbers that can be used. 
	- Standard ACLs can use 1-99 and 1300-1999.
- The basic command to configure a standard numbered ACL is:
`access-list <number> {deny|permit} <ip> <wildcard mask>`

![[Pasted image 20260317135943.png]]

![[Pasted image 20260317140048.png]]

Apply to an interface:
`ip access-group <number> {in|out}`

>Standard ACLs should be applied as close to the destination as possible.

---
**Requirements:**
- PC1 can access 192.168.2.0/24
- Other PCs in 192.168.1.0/24 can't acess 192.168.2.0/24

![[Pasted image 20260317140504.png]]

![[Pasted image 20260317140327.png]]

![[Pasted image 20260317140524.png]]


- Standard ACLs match traffic based only on the source IP address of the packet.
- Named ACLs are identified with a name (ie. 'BLOCK_BOB')
- Standard named ACLs are configured by entering 'standard named ACL config mode', and then configuring each entry within that config mode.

`ip access-list standard <acl-name>`
`[entry-number] {deny|permit} <ip> <wildcard-mask>`

![[Pasted image 20260317140920.png]]


![[Pasted image 20260317141027.png]]

---
**Requirements:**
- PCs in 192.168.1.0/24 can't access 10.0.2.0/24
- PC3 can't access 10.0.1.0/24. 
- Other PCs in 192.168.2.0/24 can access 10.0.1.0/24
- PC1 can access 10.0.1.0/24.
- Other PCs in 192.168.1.0/24 can't access 10.0.1.0/24.

![[Pasted image 20260317141559.png]]

![[Pasted image 20260317141633.png]]

![[Pasted image 20260317142304.png]]

- The router may re-order the /32 entries.
- This improves the efficiency of processing the ACL.
- It does not charge the effect to the ACL.
- This applies to both standard named and standard numbered ACLs.

![[Pasted image 20260317142458.png]]


# **Quiz**

![[Pasted image 20260317142532.png]]

![[Pasted image 20260317142602.png]]

![[Pasted image 20260317142620.png]]

![[Pasted image 20260317142638.png]]

![[Pasted image 20260317142653.png]]

![[Pasted image 20260317142758.png]]












