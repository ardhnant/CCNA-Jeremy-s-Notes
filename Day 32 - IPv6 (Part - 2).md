## Configuring IPv6 addresses (EUI-64)

![[Pasted image 20260312235324.png]]

- EUI stands for Exterior Unique Identifier
- (Modified) EUI-64 is a method of converting a MAC address (48 bits) into a 64-bit interface identifier.
- This interface identifier can then become the 'host portion' of a /64 IPv6 address.

![[Pasted image 20260312235515.png]]


## Configuring IPv6 addresses (EUI -64)

![[Pasted image 20260312235616.png]]

![[Pasted image 20260312235639.png]]

![[Pasted image 20260312235701.png]]


## Why invert the 7th bit?

- MAC addresses can be divided into two types:
	- UAA (Universally Administered Address)
	- Uniquely assigned to the device by the manufacturer

- LAA (Locally Administered Address)
	- Manually assigned by an admin (with the mac-address command on the interface) or protocol. Doesn't have to be globally unique.

- You can identity a UAA or LAA by the 7th bit of the MAC address, called the U/L bit (Universal/Local bit):
	- U/L bit set to 0 = UAA
	- U/L bit set to 1 = LAA

- In the context of IPv6 addresses/EUI-64, the meaning of the U/L bit is reversed: 
	- U/L bit set to 0 = the MAC address the EUI-64 interface ID was made from was an LAA 
	- U/L bit set to 1 = the MAC address the EUI-64 interface ID was made from was a UAA


## Global Unicast Addresses (GUA)

- Global unicast IPv6 addresses are public addresses which can be used over the internet.
- Routable.
- Must register to use them. Because they are public addresses, it is expected that they are globally unique.
- Originally defined as the 2000::/3 block (2000:: to 3fff:ffff:ffff:ffff:ffff:ffff.ffff.ffff).
- Now defined as all addresses which aren't reserved for other purposes.

![[Pasted image 20260313000948.png]]

## Unique Local Addresses (ULA) 

- Unique local IPv6 addresses are private addresses which cannot be used over the internet.
- You do not need to register to use them. They can be used freely within internal networks and don't need to be globally unique ( * ).
- Can't be routed over the Internet.
- Uses the address block (FC00:: to fdff:ffff:ffff:ffff:ffff:ffff.ffff.ffff) 
- However, a later update requires the 8th bit to be set to 1, so the first two digits must be FD.
- * The global ID should be unique so that addresses don't overlap when companies merge.

![[Pasted image 20260313001610.png]]


## Link local addresses 

- Link-local IPv6 addresses are automatically generated on IPv6-enabled interfaces.
- Use command `R1(config-if)#ipv6 enable` on an interface to enable IPv6 on an interface.
- Uses the addres block FE80::/10 (fe80:: to febf:ffff:ffff:ffff:ffff:ffff:ffff)
- However, the standard states that the 54 bits after fe80::/10 should be all 0, so you won't see link local addresses beginning with fe9, fea, or feb. Only fe8.
- The interface ID is generated using EUI-64 rules.

![[Pasted image 20260313002129.png]]

- Link-local means that these addresses are used for communication within a single link (subnet). Routers will not route packets with a link-local destination IPv6 address.
- NOT ROUTABLE.
- Common uses of link-local addresses:
	- routing protocol peerings (OSPFv3 uses link-local addresses for neighbor adjacencies)
	- next-hop addresses for static routes 
	- Neighbor Discovery Protocol (NDP, IPv6's replacement for ARP) uses link-local addresses to function 

![[Pasted image 20260313004024.png]]


## Multicast addresses 

- Unicast addresses are one-to-one.
	- One source to one destination.

- Broadcast addresses are one-to-all.
	- One source to all destination (within the subnet).

- Multicast addresses are one-to-many.
	- One source to multiply destination (that have joined the specific multicast group).

- IPv6 uses range FF00::/8 for multicast (ff00:: to ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff)
- IPv6 doesn't use broadcast (there is no 'broadcast address' in IPv6!)

![[Pasted image 20260317181948.png]]

## Multicast address scopes

- IPv6 defines multiple multicast 'scopes' which indicate how far the packet should be forwarded.
- The addresses in the precious slide all the 'link-local' scope (ff02), which stays in the local subnet.
- IPv6 multicast scopes:
	- **Interface-local** (ff01): The packet doesn't leave the local devices. Can be used to send traffic to a service within the local device.
	- **Link-local** (ff02): The packet remains in the local subnet. Routers will not route the packet between subnets.
	- **Site-local** (ff05): The packet can be forwarded by routers. Should be limited to a single physical location (not forwarded over a WAN)
	- **Organization-local**(ff08): Wider in scope than site-local (an entire company/organisation).
	- **Global**(ff0e): No boundaries. Possible to be routed over the internet.

![[Pasted image 20260313010641.png]]


## Multicast addresses

![[Pasted image 20260313010720.png]]

## Anycast addresses 

- Anycast is a new feature of IPv6.
- Anycast is 'one-to-one-of-many'
- Multiple routers are configured with the same IPv6 address.
	- They use a routing protocol to advertise the address.
	- When hosts sends packets to that destination address, routers will forwarding it to the nearest router configured with that IP address (based on routing metric).
- There is no specific address range for anycast addresses. Use a regular unicast address (global unicast, unique local) and specify it as any anycast address:

R1(config-if)#ipv6 address 2001:db8:1:1::9/128 anycast

### Anycast address configuration 

![[Pasted image 20260313013919.png]]

## Other IPv6 Address

- :: - The unspecified IPv6 address
	- Can be used when a device doesn't yet know it's IPv6 address.
	- IPv6 default routes are configured to ::/0
	- IPv4 equivalent 0.0.0.0

- :: 1 - The loopback address 
	- Used to test the protocol stack on the local device.
	- Messages sent to this address are processed within the local device, but not sent to other devices.
	- IPv4 equivalent: 127.0.0.0/8 address range

# **Quiz**

![[Pasted image 20260313014453.png]]

![[Pasted image 20260313014915.png]]


![[Pasted image 20260313014929.png]]

![[Pasted image 20260313015017.png]]


## Table summarys

| Type                     | Range / Prefix | Routable   | Assignment      | Structure                                                     | Scope       | Key Uses                  | Key Rule                    |
| ------------------------ | -------------- | ---------- | --------------- | ------------------------------------------------------------- | ----------- | ------------------------- | --------------------------- |
| **Global Unicast (GUA)** | `2000::/3`     | ✅ Yes      | ISP / registry  | 48-bit prefix + 16-bit subnet + 64-bit host                   | Global      | Internet communication    | Must be globally unique     |
| **Unique Local (ULA)**   | `fd00::/8`     | ❌ No       | Self-generated  | 8-bit prefix + 40-bit Global ID + 16-bit subnet + 64-bit host | Org-wide    | Internal networks, VPNs   | Should be unique (random)   |
| **Link-Local**           | `fe80::/10`    | ❌ No       | Auto-generated  | `fe80::` + 54 bits 0 + 64-bit host                            | Single link | NDP, routing, next-hop    | Always exists               |
| **Multicast**            | `ff00::/8`     | ⚠️ Special | Auto / protocol | `ff` + flags + scope + group ID                               | Varies      | One-to-many communication | Replaces broadcast          |
| **Anycast**              | No fixed range | ✅ Yes      | Manual config   | Same as unicast                                               | Depends     | Nearest node selection    | Same IP on multiple devices |















































