# **1. Native VLAN in Router-on-a-Stick (ROAS)**

## **1.1 What is the Native VLAN?**

In an 802.1Q trunk:

- All VLAN traffic is **tagged**
    
- Except traffic in the **native VLAN**
    
- Native VLAN frames are **untagged**
    

### Why?

Because the 802.1Q tag is not inserted for native VLAN traffic.

---

## **1.2 Why Native VLAN Exists**

### Benefit:

- Untagged frames are slightly smaller
    
- Less overhead
    
- Slightly more efficient
    

### Risk:

- Can introduce security vulnerabilities
    
- Best practice:  
    → Set native VLAN to an **unused VLAN**
    

---

# **2. Configuring Native VLAN on a Router (ROAS)**

There are **two valid methods**.

---

## **Method 1 — Subinterface + dot1q native**

![[Pasted image 20260223113552.png]]
### Configuration:

```
interface g0/0.10
 encapsulation dot1q 10 native
 ip address 192.168.1.1 255.255.255.192
```

### What This Does:

- Creates subinterface for VLAN 10
    
- Marks VLAN 10 as native
    
- Untagged frames = VLAN 10
    
- Frames sent in VLAN 10 = untagged
    


---

## **Method 2 — Physical Interface IP**

No subinterface.

```
interface g0/0
 ip address 192.168.1.1 255.255.255.192
```

### What This Means:

- Physical interface handles native VLAN
    
- No encapsulation command required
    
- Untagged frames belong to native VLAN
    

---

## **Comparison**

| Feature                         | Method 1 | Method 2 |
| ------------------------------- | -------- | -------- |
| Uses Subinterface               | Yes      | No       |
| Uses encapsulation dot1q native | Yes      | No       |
| IP on physical interface        | No       | Yes      |
| Exam Valid                      | Yes      | Yes      |

---
## Wireshark of the native VLAN packet

![[Pasted image 20260223115315.png]]

![[Pasted image 20260223115352.png]]

As you can see the packet which was sent from the HR department with the IP .65 was tagged with a VID which is 20 while it has a 802.Q tag (0x8100) 

![[Pasted image 20260223115553.png]]

while in here the packet is going from R1 to .1 in VLAN 10 and the thing to notice here is that VLAN 10 is native VLAN on all interfaces 

![[Pasted image 20260223115717.png]]
that's why there is no dot1q tag in the header hence no VID 

---

# **4. Inter-VLAN Routing Methods**

There are **three methods total**.

---

## **4.1 Method 1 — Legacy (One Interface per VLAN)**

Router uses:

- Separate physical interface per VLAN
    

### Problem:

- Not scalable
    
- Router may run out of interfaces
    

---

## **4.2 Method 2 — Router-on-a-Stick (ROAS)**

Single trunk link between:

- Switch ↔ Router
    

Uses:

- Subinterfaces
    
- 802.1Q tagging
    

### Problem:

- All traffic must go:
    
    ```
    Switch → Router → Switch
    ```
    
- Can create congestion
    

---

## **4.3 Method 3 — Layer 3 Switch (Multilayer Switch)**

Most scalable method.

---

# **5. Layer 3 Switch (Multilayer Switch)**

![[Pasted image 20260226131620.png]]

## **5.1 What It Is**

A switch that can:

- Perform Layer 2 switching
    
- Perform Layer 3 routing
    

It is both:

- Switch
    
- Router
    

---

## **5.2 Capabilities**

| Feature        | Layer 2 Switch | Layer 3 Switch |
| -------------- | -------------- | -------------- |
| MAC forwarding | Yes            | Yes            |
| IP routing     | No             | Yes            |
| SVIs           | No             | Yes            |
| Static routes  | No             | Yes            |
| Default routes | No             | Yes            |
| Routed ports   | No             | Yes            |

---

# **6. Switch Virtual Interfaces (SVIs)**

## **6.1 What is an SVI?**

- SVIs (Switch Virtual Interfaces) are the virtual interfaces you can assign IP addresses to in a multilayer switch.
- Configure each PC to use the SVI (NOT the route) as their gateway address.
- To send traffic to different subnets/VLANs, the PCs will send trafic to the switch, and the switch will route the traffic.

![[Pasted image 20260226132215.png]]


---

## **6.2 Gateway Change**

In ROAS:

- Router = gateway
    

In Layer 3 switching:

- SVI = gateway
    

PCs must use:

- SVI IP as default gateway
    

---

###  Removing Interfaces

```cisco
R1(CONFIG)#no interface g0/0.10
R1(CONFIG)#no interface g0/0.20
R1(CONFIG)#no interface g0/0.30
R1(CONFIG)#default interface g0/0
```

# **7. Enabling Routing on a Layer 3 Switch**

## **7.1 Critical Command**

``` cisco
SW2(CONFIG)#ip routing
SW2(CONFIG-IF)#no switchport
```

- `ip routing` enables **Layer 3** routing on the switch. Without this routing won't work. 
- `no switchport` configures the switch from switchport to routed port, without this routing won't be possible. 
---

# **8. Routed Ports on Layer 3 Switch**

Convert switchport into routed interface.

## **Command**

```
SW2(config-if)#interface vlan 10
SW2(config-if)#ip address 192.168.1.193 255.255.255.252
```

---

## **Purpose**

- Used for point-to-point links
- No VLAN tagging
- Functions like router interface

---

# **9. Default Route on Layer 3 Switch**

To send traffic outside LAN:

```
ip route 0.0.0.0 0.0.0.0 192.168.1.194
```

Meaning:

- Any unknown destination → send to router

![[Pasted image 20260226133532.png]]

![[Pasted image 20260226133600.png]]

---

### **10. Setting up SVIs**

```cisco
SW2(CONFIG)#interface vlan10
SW2(config-if)#ip address 192.168.1.62 255.255.255.192
SW2(config-if)#no shutdown
SW2(config-if)#interface vlan20
SW2(config-if)#ip address 192.168.1.126 255.255.255.192
SW2(config-if)#no shutdown
SW2(config-if)#interface vlan30
SW2(config-if)#ip address 192.168.1.190 255.255.255.192
SW2(config-if)#no shutdown
``` 

- SVIs are shutdown by default, so remember to use no shutdown.



---

## **Required Conditions**

| Requirement                      | Explanation               |
| -------------------------------- | ------------------------- |
| VLAN must exist                  | Must be created on switch |
| At least one active port in VLAN | Access or trunk           |
| VLAN must not be shutdown        | VLAN itself enabled       |
| SVI must be no shutdown          | Enabled manually          |

---

## **Common Problem**

![[Pasted image 20260226134421.png]]

If SVI shows:

```
down/down
```

Likely causes:

- VLAN not created
    
- No active ports in VLAN
    

---

# **11. Removing Router-on-a-Stick**

When switching to Layer 3 switching:

### On Router:

```
no interface g0/0.10
default interface g0/0
```

Then configure new /30 IP.

---

# **12. Verification Commands**

| Command                 | Purpose                     |
| ----------------------- | --------------------------- |
| show ip interface brief | Check interface IP & status |
| show ip route           | Check routing table         |
| show interfaces trunk   | Check trunk ports           |
| show interfaces status  | See routed vs switchport    |

---

# **Absolute Must-Know Commands**

| Command                           | Purpose                     |
| --------------------------------- | --------------------------- |
| encapsulation dot1q X native      | Native VLAN on subinterface |
| ip routing                        | Enable routing on switch    |
| no switchport                     | Create routed port          |
| interface vlan X                  | Create SVI                  |
| no shutdown                       | Enable SVI                  |
| ip route 0.0.0.0 0.0.0.0 NEXT_HOP | Default route               |
