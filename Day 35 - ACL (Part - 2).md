
## Configuring numbered ACLs with subcommands 

- In modern IOS you can also configure numbered ACLs in the exact same way as named ACLs:

![[Pasted image 20260317143125.png]]

- This is just a different way of configuring numbered ACLs. However, in the running-config the ACL will display as if it was configured using the traditional method.

![[Pasted image 20260317143318.png]]

- You can easily delete individual entries in the ACL with no `<entry-number>`
![[Pasted image 20260317143516.png]]

![[Pasted image 20260317143551.png]]

> When configuring/editing numbered ACLs from global config mode, you can't delete individual entries, you can only delete the entire ACL!


## Advantage of named ACL config mode

- You can easily delete individual entries in the ACL with no `sequence-number`
- You can insert new entries in between other by specifying the sequence number.

![[Pasted image 20260317143951.png]]

## Resequencing ACLs 

- There is a `resequencing` function that helps edit ACLs.
- The command is `ip access-list resequence <acl-id> <increment>`

![[Pasted image 20260317144200.png]]


## Extended ACLs 

- Extended ACLs function mostly the same as standard ACLs.
- They can be numbered or named, just like standard ACLs.  
    → Numbered ACLs use the following ranges: 100 – 199, 2000 – 2699
- They are processed from top to bottom, just like standard ACLs.
- However, they can match traffic based on more parameters, so they are more precise (and more complex) than standard ACLs.
- We will focus on matching based on these main parameters: **Layer 4 protocol/port, source address, and destination address.**

`access-list number [permit | deny] protocol src-ip dest-ip

 `ip access-list extended {name | number}`
`[seq-num] [permit | deny] protocol src-ip dest-ip`

### Matching the protocol

![[Pasted image 20260317145232.png]]

### Matching the src/dest IP address

![[Pasted image 20260317145308.png]]

> In extended ALCs, to specify a /32 source or destination you have to use the host option or specify the wildcard mask. You can't write the address without either of those.


![[Pasted image 20260317145547.png]]

![[Pasted image 20260317145612.png]]

### Matching the TCP/UDP port numbers

![[Pasted image 20260317145829.png]]

`deny tcp any host 1.1.1.1 eq 80`

- Deny all packets destined for IP address 1.1.1.1/32, TCP port 80.

- After the destination IP address and/or destination port numbers, there are many more options you can use to match (not necessary for the CCNA).

Some examples: 
- ack: match TCP ACK flag
- fin: match the TCP FIN flag
- syn: match TCP SYN flag
- ttl: match packets with a specific TTL value
- dscp: match packets with a specific DSCP value

>If you specific the protocol, source IP, source port, destination IP, destination port, etc, a packet must match. ALL of those values to match the ACL entry. Even if it matches all except one of the parameters, the packet won't match that entry of the ACL.

![[Pasted image 20260317153209.png]]

## Extended ACLs

![[Pasted image 20260317153319.png]]

---
**Requirements:**
- Hosts in 192.168.1.0/24 can't use HTTPS to access SRV1.
- Hosts in 192.168.2.0/24 can't access 10.0.2.0/24
- None of the hosts in 192.168.1.0/24 can ping 10.0.1.0/24 or 10.0.2.0/24.
---

- Extended ACLs should be applied as close to the source as possible, to limit how far the packets travel in the network before being denied.
- (Standard ACLs are less specific, so if they are applied close to the source there is a risk of blocking more traffic that intended)

![[Pasted image 20260317153915.png]]

![[Pasted image 20260317153938.png]]

![[Pasted image 20260317153958.png]]

![[Pasted image 20260317154012.png]]

![[Pasted image 20260317154028.png]]

# **Quiz**
![[Pasted image 20260317154057.png]]

![[Pasted image 20260317154113.png]]

![[Pasted image 20260317154132.png]]






















