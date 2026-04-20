## **1. Switch Interfaces Overview**

This lesson focuses on **switch interfaces**, how they behave, and how they differ from router interfaces.

Previously:

- Router interfaces were configured with IP addresses.
    
- `show ip interface brief` was used to verify Layer 1 and Layer 2 status.
    
- Router interfaces are **shutdown by default**.
    

In this lesson:

- Focus is on **Layer 1 characteristics** of switch interfaces:
    
    - Speed
        
    - Duplex
        
    - Interface status
        
    - Interface counters and errors
        

---

## **2. Router Interfaces vs Switch Interfaces**

### **Router Interfaces**

- Shutdown by default.
    
- Require manual enabling using:
    

```
no shutdown
```

- Default state:
    

```
administratively down/down
```

---

### **Switch Interfaces**

- NOT shutdown by default.
    
- Automatically become active when connected to another device.
    
- Default states:
    

|Condition|State|
|---|---|
|Connected to device|up/up|
|Not connected|down/down|

Important:

- **down/down ≠ shutdown**
    
- It only means no physical connection exists.
    

---

### **IP Address Behavior**

- Switch interfaces (Layer 2 switchports) do **not require IP addresses**.
    
- IP addressing on switches is introduced later with multilayer switching.
    
- Ignore IP address column for now.
    

---

## **3. Switch Interface Density**

Key physical difference:

- Routers → few interfaces.
    
- Switches → many interfaces.
    

Reason:

- Switches connect many end hosts (PCs, printers, etc.).
    
- Example:
    
    - Router: few SFP or RJ45 ports.
        
    - Switch: dozens of RJ45 ports.
        

---

## **4. Example Topology Used**

Single LAN:

```
192.168.1.0/24
```

Devices:

- 1 Router (R1)
    
- 2 Switches (SW1, SW2)
    
- 4 PCs
    

Focus device:

- SW1
    
- Interfaces F0/1 to F0/4 connected
    
- Remaining interfaces unused
    

---

## **5. show ip interface brief on Switches**

Displays:

|Column|Meaning|
|---|---|
|Status|Layer 1 (Physical)|
|Protocol|Layer 2 (Data Link)|

Typical outputs:

|State|Meaning|
|---|---|
|up/up|Interface working|
|down/down|No cable connected|
|administratively down/down|Interface shutdown|

Key difference:

- Router → administratively down by default
    
- Switch → up/up when connected
    

---

## **6. `show interfaces status` Command**

Used to quickly check switch interface information.

### **Fields Explained**

|Field|Meaning|
|---|---|
|Port|Interface name|
|Name|Interface description|
|Status|Connection state|
|VLAN|Assigned VLAN|
|Duplex|Full or half duplex|
|Speed|Interface speed|
|Type|Interface type|

---

### **Status Values**

|Status|Meaning|
|---|---|
|connected|Device connected|
|notconnect|No device connected|
|disabled|Interface shutdown|

Note:

- Status here differs from `show ip interface brief`.
    
- "disabled" ≈ administratively down.
    

---

### **VLAN Field**

- Default VLAN = 1.
    
- Some interfaces may show **trunk**.
    
- VLANs and trunks are covered later.
    

---

## **7. Speed and Duplex**

### **Speed**

Data transmission rate:

- 10 Mbps
    
- 100 Mbps
    
- 1000 Mbps (Gigabit)
    

FastEthernet interfaces support:

- 10 or 100 Mbps
    

---

### **Duplex**

Determines if sending and receiving can occur simultaneously.

| Mode        | Behavior                             |
| ----------- | ------------------------------------ |
| Half Duplex | Cannot send and receive at same time |
| Full Duplex | Can send and receive simultaneously  |

Modern networks use **full duplex**.

---

### **Default Behavior**

- Speed = auto
    
- Duplex = auto
    

Interfaces negotiate best supported settings automatically.

Output examples:

|Value|Meaning|
|---|---|
|a-full|Auto negotiated full duplex|
|a-100|Auto negotiated speed 100 Mbps|

---

## **8. Manual Speed and Duplex Configuration**

Normally not required, but useful for troubleshooting.

### **Commands**

```
interface f0/1
speed 100
duplex full
description Connected to R1
```


#### For speed
![[Pasted image 20260209182156.png]]

#### For duplex
![[Pasted image 20260209182229.png]]

After manual configuration:

- Values no longer show auto negotiation.
    
- Output shows fixed speed and duplex.
    

Best practice:

- Leave autonegotiation enabled unless troubleshooting.
    

---

## **9. Interface Descriptions**

Descriptions help identify connections.

Example:

```
description Connected to R1
```

Appears in:

```
show interfaces status
```

Useful for:

- Documentation
    
- Troubleshooting
    
- Large networks
    

---

## **10. Disabling Unused Interfaces (Security Practice)**

Switch ports are enabled by default, which can be a security risk.

Recommended practice:

- Shutdown unused interfaces.
    

---

### **Interface Range Command**

Configure multiple interfaces at once:
To select more then 1 interface you can do `interface range f0/5 - 12`
![[Pasted image 20260209182456.png]]
```
interface range f0/5 - 12
description UNUSED
shutdown
```

---

### **Non-Consecutive Interface Ranges**

Possible syntax:
![[Pasted image 20260209182556.png]]

```
interface range f0/5 - 6 , f0/9 - 12
```

Allows selective configuration.

---

## **11. Full Duplex vs Half Duplex (Detailed)**

### **Half Duplex**

- Cannot send and receive simultaneously.
    
- Device must wait before transmitting.
    
- Used with hubs.
    

Problem:

- Collisions occur when multiple devices transmit simultaneously.
    

---

### **Hub Behavior**

- Operates at Layer 1.
    
- Simply repeats incoming signals.
    
The problem occurs when 2 devices are trying to send packets on same time(they will collide).. that's why half duplex are used in hub to avoid collison 

---

### **CSMA/CD**

Used in half-duplex networks.

Stands for:

```
Carrier Sense Multiple Access with Collision Detection
```

Process:

1. Device listens before sending.
    
2. Sends only if medium is idle.
    
3. If collision occurs:
    
    - Sends jamming signal.
        
    - Waits random time.
        
    - Retries transmission.
        

---

### **Full Duplex**

- Send and receive simultaneously.
    
- No waiting required.
    
- Used in switch-based networks.
    
- Collisions are rare and usually indicate misconfiguration.
    

---

## **12. Collision Domains**

With hubs:

- Entire network = one collision domain.
    

With switches:

- Each interface = separate collision domain.
    

Result:

- Improved performance.
    
- Full duplex operation possible.
    

---

## **13. Speed and Duplex Autonegotiation**

Interfaces advertise capabilities to neighbors and agree on:

- Highest common speed
    
- Best duplex mode
    

Example:

|Device Capability|Negotiated Result|
|---|---|
|Ethernet|10 Mbps full|
|FastEthernet|100 Mbps full|
|GigabitEthernet|1000 Mbps full|

Switch adjusts to match connected device.

---

### **When Autonegotiation Is Disabled on One Side**

Behavior:

1. Switch tries to detect speed.
    
2. If speed detected:
    
    - Use slowest supported speed.
        
3. Duplex rules:
    
    - 10 or 100 Mbps → half duplex
        
    - 1000 Mbps → full duplex
        

Problem:

- Can cause **duplex mismatch**.
    

---

### **Duplex Mismatch Result**

- Collisions occur.
    
- Poor network performance.
    

Best practice:

- Enable autonegotiation on both sides.
    

---

## **14. Interface Counters and Errors**

Viewed using:

```
show interfaces
```

Usually checked per interface due to large output.

---

### **Important Counters**

| Counter       | Meaning                       |
| ------------- | ----------------------------- |
| Packets       | Total packets received        |
| Bytes         | Total data received           |
| Runts         | Frames smaller than 64 bytes  |
| Giants        | Frames larger than 1518 bytes |
| CRC           | Frames failing CRC check      |
| Frame         | Incorrect frame format        |
| Input Errors  | Total input-related errors    |
| Output Errors | Failed transmitted frames     |

These counters exist on both switches and routers.

---

## All the commands we learned in this lecture

| Commands                            | Description                                                                   |
| ----------------------------------- | ----------------------------------------------------------------------------- |
| show ip interface brief             | display status, protocol and state                                            |
| show interfaces status              | display port, name, status, vlan, duplex, speed and type                      |
| speed 100                           | to set speed 100 of any interface                                             |
| duplex full                         | to set duplex as full of any interface                                        |
| interface f0/1                      | to select the interface with name f0/1                                        |
| description archgod                 | to put archgod in the description of selected interface                       |
| interface range f0/5 - 12           | to select all the interface between and including f0/5 and f0/12              |
| interface range f0/5 - 6 , f0/9 -12 | to select all the interface between and including f0/5 and 6 & f0/9 and f0/12 |

