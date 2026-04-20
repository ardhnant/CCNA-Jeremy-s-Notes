
## What DTP(Dynamic Trunking Protocol) Is 

DTP is a **Cisco proprietary Layer 2 protocol** that allows two Cisco switches to **automatically negotiate whether a port becomes an access port or a trunk port**.

Instead of manually configuring:

```
switchport mode access
switchport mode trunk
```

DTP lets switches decide dynamically.

**Important**

- Cisco proprietary (works only between Cisco devices)
    
- Enabled by default on Cisco switch interfaces
    
- **Recommended: Disable it for security**
    

---

## Why DTP Is Dangerous

DTP can be exploited by attackers to:

- Force a trunk link
    
- Gain access to multiple VLANs
    

**Best Practice**

- Manually configure all ports
    
- Disable DTP using:
    


Or configure:

``` cisco
SW1(config-if)#switchport nonegotiate 
```


``` cisco
SW1(config-if)#switchport mode access
```

(which automatically disables DTP)

---

## Administrative Mode vs Operational Mode

![[Pasted image 20260226235530.png]]

Command:

```
SW1(config-if)#show interfaces g0/0 switchport
```

| Field               | Meaning                                |
| ------------------- | -------------------------------------- |
| Administrative Mode | What you configured                    |
| Operational Mode    | What the port is actually operating as |

Example:

- Admin = dynamic desirable
    
- Operational = trunk
    

---

## DTP Modes

![[Pasted image 20260226234439.png]]

### Dynamic Desirable

- Actively tries to form a trunk
    
- Sends DTP frames aggressively
   ![[Pasted image 20260226234934.png]]

Forms trunk with:

- `trunk`
![[Pasted image 20260226235038.png]]
- `dynamic desirable`
![[Pasted image 20260226234934.png]]
- `dynamic auto`
![[Pasted image 20260226235345.png]] 

Will NOT form trunk with:

- `access`
 ![[Pasted image 20260226235441.png]]
![[Pasted image 20260226235504.png]]

---

### Dynamic Auto

- Passive mode
    
- Will form trunk only if the other side tries
![[Pasted image 20260226235623.png]]


Forms trunk with:

- `trunk`

- `dynamic desirable`


Will NOT form trunk with:

- `dynamic auto`
![[Pasted image 20260226235640.png]]
- `access`
![[Pasted image 20260226235706.png]]

---

### Access Mode

- Forces access port
    
- Disables DTP negotiation
    
- Will never become trunk
    

---

### Trunk Mode

- Forces trunk
    
- Still sends DTP frames unless `switchport nonegotiate` is configured
    

---

## DTP Combination Table (Memorize This)

![[Pasted image 20260226235853.png]]

**Misconfig = trunk + access**  
DTP will not form a trunk with a router, PC, etc. The switchport will be in access mode.

---

## Default Modes (Very Important for Exam)

| Switch Type    | Default Mode      |
| -------------- | ----------------- |
| Older switches | dynamic desirable |
| Newer switches | dynamic auto      |

This explains the classic exam question:  
Replacing a new switch with an old one can suddenly create a trunk.

---

## DTP and Non-Switch Devices

DTP will **NOT form trunk with routers or PCs**.

For Router-on-a-Stick:  
You must manually configure:

```
switchport mode trunk
```

Dynamic modes will not work.

---

## Trunk Encapsulation Negotiation

- Switches that support both 802.1Q and ISL trunk encapsulations can use DTP to negotiate the encapsulation they will use.
- This negotiation is enabled by default, as the default truck encapsulation mode is: `switchport mode access encapsulation negotiate `
- ISL is favored over 802.Q, so if both switches support ISL it will be selected.
- DTP frames are sent in VLAN1 when using ISL, or in the native VLAN when using 802.1Q (the default native VLAN is VLAN1, however).

![[Pasted image 20260227000913.png]]


---

# **VTP (VLAN Trunking Protocol)**

## What VTP Is

- VTP allows you to configure VLANs on a central VTP server switch, and other switches (VTP clients) will synchronize their VLAN database to the server
- It is designed for large network with many VLANs, so that you don' have to configure each VLAN on every switch.
- It is rarely used, and it is recommended that you do not use it.
- There are three VTP versions: 1, 2, and 3.
- There  are three VTP modes: server, client and transparent.
- Cisco switches operate in VTP server mode by default.

Instead of creating VLANs on every switch:  
Create VLAN once on server → clients sync automatically.

---

## Important Reality

VTP is:

- Rarely used in modern networks
    
- Risky
    
- Potentially destructive
    

Recommended in real networks:

- Do not use VTP
    
- Use transparent mode
    
### VTP Server
 - Can add/modify/delete VLANs.
 - Store  the VLAN database in non-volatile RAM(NVRAM)
- Will increase the revision number every time a VLAN is added/modified/deleted.
- Will advertise the latest version of the VLAN database to trunk interfaces, and the VTP clients will synchronize their VLAN database to it.
- VTP servers also function as VTP clients.

**Therefore, a VTP server will synchronize to another VTP server with a higher revision number.**

### VTP clients
- Cannot add/modify/delete VLANs.
- Do not store the VLAN database in NVRAM. (in VTPv3, they do.)
- Will synchronize their VLAN database to the server with the highest revision number in their VTP domain.
- Will advertise their VLAN database, and forward VTP advertisements to other clients over thier trunk ports.

![[Pasted image 20260227002354.png]]

- VTPv1/v2 do not support the extended VLAN range (1006-4094). Only VTPv3 supports them.
---
## **VTP Domain**

Switches must share:

- Same VTP domain name
    
- Same VTP version
    

Default domain:  
NULL

If a switch with NULL domain receives advertisement:  
→ It joins the domain automatically.

![[Pasted image 20260227002758.png]]

- One danger of VTP: If you connect an old switch with a higher revision number to your network (and the VTP domain name matches), all switches in the domain will sync their VLAN database to that switch.


---

## **VTP Modes**

## Server Mode (Default)

- Can add, modify, delete VLANs
    
- Stores VLAN database in NVRAM
    
- Increments revision number when changes occur
    
- Advertises VLAN database
    
- Also acts as a client
    
![[Pasted image 20260227003704.png]]
---

## Client Mode

- Cannot create/modify/delete VLANs
    
- Syncs VLAN database from server
    
- Does NOT store VLAN database in NVRAM **(except v3)**
    
- Forwards advertisements
    

Command:

```
vtp mode client
```

---

## Transparent Mode

- Does not participate in the VTP domain (does NOT sync VLAN database)
    
- Maintains its own VLAN database in NVRAM. It can add/modify/delete VLANs but they won't be advertised to other switches.
    
- Can create VLANs locally
    
- Will forwards VTP advertisements that are in the same domain as it. 
    

Command:

```
vtp mode transparent
```

Best practice:  
Use transparent mode in production networks.

---

- Changing the VTP domain to an unused domain will reset the revision number to 0.
- Changing the VTP mode to transparent will also reset te revision number to 0.

---

# Revision Number (Critical Concept)

Every time VLAN is:

- Added
    
- Deleted
    
- Modified
    

Revision number increments.

Switch with **highest revision number wins**.

All switches sync to highest revision number in same domain.

---

## The Dangerous Scenario

If you connect an old switch with:

- Higher revision number
    
- Same VTP domain
    

Your entire VLAN database can be overwritten.

Result:

- VLANs disappear
    
- Hosts lose connectivity instantly
    

This is the primary reason VTP is avoided.

---

# Resetting VTP Revision Number

Two methods:

|Method|Command|
|---|---|
|Change domain to unused name|`vtp domain TEMP`|
|Change to transparent mode|`vtp mode transparent`|

Both reset revision to 0.

---

# VTP Advertisement Rules

- Sent only on trunk ports
    
- Not sent on access ports
    
- Transparent mode forwards advertisements (same domain)
    

---

# Verification Commands

|Command|Purpose|
|---|---|
|`show vtp status`|Version, mode, revision, domain|
|`show vlan brief`|VLAN database|
|`show interfaces trunk`|Trunk ports|
|`show interfaces switchport`|DTP status|

---

# What VTP Does NOT Do

VTP:

- Syncs VLAN database only
    

VTP does NOT:

- Assign ports to VLANs
    
- Configure access VLANs
    
- Configure trunks
    

You must still manually configure:

```
switchport access vlan 10
```

---

# Exam-Focused Summary

## DTP

- Cisco proprietary
    
- Negotiates trunk/access
    
- Modes: access, trunk, dynamic auto, dynamic desirable
    
- Disable in production
    
- Older switches default to dynamic desirable
    
- Newer switches default to dynamic auto
    

---

## VTP

- Cisco proprietary
    
- Synchronizes VLAN database
    
- Default mode = server
    
- Modes: server, client, transparent
    
- Revision number decides authority
    
- Highest revision number overwrites others
    
- Reset revision before adding old switch
    

---

If someone studies only these notes:

They will understand:

- How trunks form
    
- Why mismatches break connectivity
    
- How VLAN databases synchronize
    
- Why VTP can destroy a network
    
- Why DTP should be disabled
    
- What commands matter for verification
    

This is sufficient depth for CCNA-level understanding without unnecessary noise.