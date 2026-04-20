## **1. What is a LAN (Local Area Network)?**

A **LAN** is not just a group of devices in one location.

### **Precise Definition**

A LAN is:

> **A single broadcast domain**
---

### **Broadcast Domain**

A broadcast domain is:

- The group of devices that receive a **broadcast frame**
    
- A broadcast frame has a destination MAC address of:
    

```
FF:FF:FF:FF:FF:FF
```
**
When one device sends a broadcast:

- All devices in the same broadcast domain receive it.
    

---

### **Device Behavior with Broadcast Frames**

| Device | Behavior                                                           |
| ------ | ------------------------------------------------------------------ |
| Switch | Floods broadcast frames out all interfaces except the incoming one |
| Router | Receives broadcast but does NOT forward it                         |

Because routers do not forward broadcasts:

- Each router interface separates broadcast domains.
    

---

### **Key Rule**

Each router interface and everything connected to it forms **one broadcast domain**.

Therefore:

```
Number of broadcast domains = number of LANs
```

---

## **2. Example: Identifying Broadcast Domains**

![[Pasted image 20260214233318.png]]

In a network:

- PC → Switch → Router connections form a broadcast domain.
    
- Broadcasts stop at routers.
    

Example outcome:

- PC1, PC2, SW1, and one router interface = 1 broadcast domain
    
- Another switch group = another broadcast domain
    
- Router-to-router link = also a broadcast domain
    

Even a link between only two routers is technically a broadcast domain.

---

## **3. Problem with a Single LAN**

Example company network:

Departments:

- Engineering
    
- Sales
    
- Human Resources
    

All devices in:

```
192.168.1.0/24
```

### **Problems**

#### **1. Performance**

- Broadcast traffic is sent to ALL devices.
    
- Excessive broadcasts reduce network performance.
    

Sources of unnecessary traffic:

- Broadcast frames
    
- Unknown unicast flooding
    

---

#### **2. Security**

Devices in the same LAN:

- Communicate directly.
    
- Traffic does not pass through the router.
    

Result:

- Router or firewall policies cannot control internal communication.
    

---

## **4. Splitting Networks Using Subnets (Layer 3 Separation)**

Solution attempt:

Split departments into separate subnets:

|Department|Network|
|---|---|
|Engineering|192.168.1.0/26|
|HR|192.168.1.64/26|
|Sales|192.168.1.128/26|
![[Pasted image 20260214233632.png]]

Now:

- Traffic between departments must pass through the router.
    
- Security policies can be applied.
    

---

### **Router Requirement**

The router must have:

- One IP address in each subnet.
    
- One interface per subnet (in this basic design).
    

---

### **Inter-Subnet Communication Process**

![[Pasted image 20260214234204.png]]

When PC1 sends traffic to another subnet:

1. PC1 detects destination is outside its subnet.
    
2. Frame destination MAC = default gateway.
    
3. Router receives frame.
    
4. Router rewrites:
    
    - Source MAC
        
    - Destination MAC
        
5. Router forwards frame back to switch.
    
6. Switch delivers to destination host.
    

---

## **5. Remaining Problem (Layer 2 Issue)**

Even after subnetting:

- The switch operates at **Layer 2**.
    
- Switch only checks MAC addresses.
    
- Switch does NOT understand IP subnets.
    

Result:

Broadcast or unknown unicast frames are still flooded to all interfaces.

So:

- Multiple subnets
    
- Still one broadcast domain
    
- Still one LAN
    

This is inefficient and insecure.

---

## **6. VLANs (Virtual LANs)**

### **Definition**

A VLAN is:

> A method of logically separating a Layer 2 network into multiple broadcast domains.

Even if devices are connected to the same physical switch, VLANs allow logical separation.

---

### **Example VLAN Assignment**

|Department|VLAN|
|---|---|
|Engineering|VLAN 10|
|HR|VLAN 20|
|Sales|VLAN 30|

---

### **How VLAN Membership Works**

VLANs are configured:

- On **switch interfaces**
    
- Not on the end devices.
    

If a PC connects to an interface assigned to VLAN 10:

- That PC belongs to VLAN 10.
    

---

### **Switch Behavior with VLANs**

The switch treats each VLAN as:

```
A separate LAN
```

Therefore:

- Broadcasts stay inside the VLAN.
    
- Unknown unicast traffic stays inside the VLAN.
    
- Traffic is NOT forwarded between VLANs.
    

---

## **7. Broadcast Behavior with VLANs**

Example:

PC1 in VLAN10 sends a broadcast:

- Switch forwards only to interfaces in VLAN10.
    
- Devices in VLAN20 or VLAN30 do not receive it.
    

This improves:

- Performance
    
- Security
    

---

## **8. Communication Between VLANs**

Switches do NOT perform inter-VLAN routing (basic Layer 2 switches).

To communicate between VLANs:

1. Traffic must go to a router.
    
2. Router performs routing.
    
3. Router sends traffic back to switch.
    

This is called **inter-VLAN routing** (covered later).

---

### **Important Rule**

A switch never forwards traffic directly between VLANs.

Even if:

- Hosts are in the same subnet,
    
- Different VLANs prevent direct communication.
    

---

## **9. VLAN Summary Concepts**

- VLANs are configured per interface.
    
- VLANs logically separate hosts at Layer 2.
    
- Each VLAN is a separate broadcast domain.
    
- Broadcast traffic stays inside the VLAN.
    
- Router required for communication between VLANs.
    

---

## **10. Default VLANs on Cisco Switches**

Command:

```
show vlan brief
```

Displays:

- Existing VLANs
    
- Assigned interfaces
    

---

### **Default VLANs**

These exist automatically:

![[Pasted image 20260215000421.png]]


|VLAN|Purpose|
|---|---|
|1|Default VLAN|
|1002–1005|Legacy technologies (FDDI, Token Ring)|

Important:

- VLAN 1 and VLANs 1002–1005 cannot be deleted.
    
- All interfaces are in VLAN 1 by default.
    

---

## **11. Access Ports**

An **access port**:

- Belongs to one VLAN only.
    
- Typically connects to end devices (PCs, printers).
    

Configuration is usually automatic but should be set manually for reliability.

---

### **Access Port Commands**

![[Pasted image 20260215000804.png]]

```
interface range g1/0 - g1/3
switchport mode access
switchport access vlan 10
```

If VLAN does not exist:

```
%Access VLAN does not exist. Creating vlan 10.
```

The switch creates it automatically.

---

## **12. Creating and Naming VLANs**

Manual VLAN configuration:

![[Pasted image 20260215000921.png]]

```
vlan 10
name ENGINEERING
```

Example naming:

|VLAN|Name|
|---|---|
|10|ENGINEERING|
|20|HR|
|30|SALES|

Verify using:

```
show vlan brief
```

![[Pasted image 20260215000958.png]]

---

## **13. Broadcast Testing Example**

Command:

```
ping 255.255.255.255
```

Result:

- Broadcast reaches only devices in the same VLAN.
    

---

## **14. Purpose of VLANs (Exam Focus)**

### **1. Performance**

- Reduces unnecessary broadcast traffic.
    
- Prevents congestion.
    

### **2. Security**

- Limits which devices receive traffic.
    
- Enables control between departments.
    

Goal:

```
Traffic should not be sent where it is not needed.
```

[^1]: 
