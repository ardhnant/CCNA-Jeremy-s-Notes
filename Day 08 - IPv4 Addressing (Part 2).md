## **1. IPv4 Address Classes

IPv4 addresses were historically divided into **classes** based on the number of network and host bits.

### **Class Ranges**

|Class|First Octet Range|Default Prefix|Network Bits|Host Bits|
|---|---|---|---|---|
|A|0–127|/8|8|24|
|B|128–191|/16|16|16|
|C|192–223|/24|24|8|

### **Important Notes**

- **127.x.x.x**
    
    - Reserved for **loopback addresses**
        
    - Not usable for normal networks
        
- **0.x.x.x**
    
    - Reserved
        
    - Not usable for host assignment
        
- Practical Class A usable range:
    
    - **1–126**
        

---

### **Leading Bit Patterns**

|Class|Leading Bits|
|---|---|
|A|0|
|B|10|
|C|110|

These bits identify the class in binary.

---

## **2. Network Portion vs Host Portion**

An IPv4 address consists of:

- **Network portion** → identifies the network
    
- **Host portion** → identifies devices within the network
    

The division depends on the **prefix length**.

Example:

```
192.168.1.0/24
```

- First 24 bits → network
    
- Last 8 bits → host portion
    

---

## **3. Address Types in a Network**

Each network contains special addresses that cannot be assigned to hosts.

### **1. Network Address (Network ID)**

- Host portion = all 0s
    
- Identifies the network itself
    
- Cannot be assigned to devices
    

Example:

```
192.168.1.0/24
```

---

### **2. Broadcast Address**

- Host portion = all 1s
    
- Used to send traffic to all hosts in the network
    
- Cannot be assigned to devices
    

Example:

```
192.168.1.255/24
```

---

### **3. Usable Host Addresses**

All addresses between:

```
Network address + 1
Broadcast address − 1
```

---

## **4. Maximum Number of Hosts**

### **Formula**

```
Maximum usable hosts = 2^N − 2
```

Where:

- **N = number of host bits**
    
- Subtract 2 because:
    
    - Network address
        
    - Broadcast address
        

---

### **Examples**

|Network|Host Bits|Total Addresses|Usable Hosts|
|---|---|---|---|
|/24|8|256|254|
|/16|16|65,536|65,534|
|/8|24|16,777,216|16,777,214|

---

## **5. Finding Network, Broadcast, and Usable Addresses**

### **Procedure**

#### Step 1 — Identify prefix length

Determines network and host portions.

#### Step 2 — Network Address

Set all host bits to **0**.

#### Step 3 — Broadcast Address

Set all host bits to **1**.

#### Step 4 — First Usable Address

```
Network address + 1
```

#### Step 5 — Last Usable Address

```
Broadcast address − 1
```

---

### **Example 1 — Class C**

```
192.168.1.0/24
```

- Network address → 192.168.1.0
    
- Broadcast address → 192.168.1.255
    
- First usable → 192.168.1.1
    
- Last usable → 192.168.1.254
    
- Max hosts → 254
    

---

### **Example 2 — Class B**

```
172.16.0.0/16
```

- Network → 172.16.0.0
    
- Broadcast → 172.16.255.255
    
- First usable → 172.16.0.1
    
- Last usable → 172.16.255.254
    
- Max hosts → 65,534
    

---

### **Example 3 — Class A**

```
10.0.0.0/8
```

- Network → 10.0.0.0
    
- Broadcast → 10.255.255.255
    
- First usable → 10.0.0.1
    
- Last usable → 10.255.255.254
    
- Max hosts → 16,777,214
    

---

## **6. Configuring IP Addresses on Cisco Routers**

Routers connect multiple networks. Each interface must have an IP address from its connected network.

Example topology:

|Network|PC Address|Router Interface Address|
|---|---|---|
|10.0.0.0/8|10.0.0.1|10.255.255.254|
|172.16.0.0/16|172.16.0.1|172.16.255.254|
|192.168.0.0/24|192.168.0.1|192.168.0.254|

---

## **7. Interface Status Concepts**

### **show ip interface brief Output Fields**

|Field|Meaning|
|---|---|
|Interface|Interface name|
|IP Address|Assigned IP address|
|OK?|Legacy validation field|
|Method|How IP was assigned (manual, DHCP, etc.)|
|Status|Layer 1 status (physical)|
|Protocol|Layer 2 status|

---

### **Status vs Protocol**

|Column|OSI Layer|Meaning|
|---|---|---|
|Status|Layer 1|Physical connection state|
|Protocol|Layer 2|Data link functioning|

Important rules:

- If Layer 1 is down → Layer 2 must be down
    
- Possible states:
    
    - up / up → working
        
    - administratively down → interface disabled
        

---

### **Default Behavior**

- Router interfaces are **shutdown by default**
    
- Switch interfaces are **not shutdown by default**
    

---

## **8. Interface Configuration Process**

### **Basic Steps**

1. Enter privileged mode
    
2. Enter global configuration mode
    
3. Enter interface configuration mode
    
4. Assign IP address and subnet mask
    
5. Enable interface
    

---

### **Subnet Mask Reminder**

|Prefix|Subnet Mask|
|---|---|
|/8|255.0.0.0|
|/16|255.255.0.0|
|/24|255.255.255.0|

---

## **9. Interface Information Commands**

### **show interfaces**

Displays:

- Layer 1 status
    
- Layer 2 status
    
- MAC address
    
- Interface hardware info
    
- IP address
    
- Detailed statistics
    

Notes:

- **BIA** = Burned-In Address (physical MAC address)
    
- MAC address can be manually changed but rarely is.
    

---

### **show interfaces description**

Displays:

- Interface
    
- Status
    
- Protocol
    
- Configured description
    

Interface descriptions help identify connections.

---

## **10. Interface Descriptions**

Optional but recommended.

Purpose:

- Identify connection purpose
    
- Improve troubleshooting
    
- Useful in large networks
    

Example description:

```
Connected to Switch1
```

---

## **11. Important Exam Concepts**

- Network address = host bits all 0
    
- Broadcast address = host bits all 1
    
- First usable = network + 1
    
- Last usable = broadcast − 1
    
- Hosts = 2^host_bits − 2
    
- Router interfaces must be enabled manually
    

---

## **12. Cisco CLI Commands Summary**

|Command|Mode|Purpose|
|---|---|---|
|enable (en)|User EXEC|Enter privileged EXEC mode|
|configure terminal (conf t)|Privileged EXEC|Enter global config mode|
|interface g0/0 (int g0/0)|Global Config|Enter interface config mode|
|ip address A.B.C.D MASK|Interface Config|Assign IP address|
|no shutdown (no shut)|Interface Config|Enable interface|
|show ip interface brief|Privileged EXEC|Quick interface status & IP check|
|show interfaces|Privileged EXEC|Detailed interface info|
|show interfaces g0/0|Privileged EXEC|Detailed info for specific interface|
|show interfaces description|Privileged EXEC|View interface descriptions|
|description TEXT|Interface Config|Add interface description|
|do COMMAND|Config Mode|Run EXEC command without exiting|
|?|Any|Context-sensitive help|


---


## **21. Quiz Logic (What CCNA Expects)**

You must be able to instantly find:

- Network address
    
- Broadcast address
    
- Max hosts
    
- First usable
    
- Last usable
    

Without hesitation.

---

## **22. Sample Mental Drill**

Given:

```
155.200.201.141/16
```

Answer:

- Network: 155.200.0.0
    
- Broadcast: 155.200.255.255
    
- Hosts: 65,534
    
- First: 155.200.0.1
    
- Last: 155.200.255.254
    

---

## **23. Final Day 8 Takeaways**

- Classes still matter conceptually
    
- Host calculation = **2^N − 2**
    
- Network = all host bits 0
    
- Broadcast = all host bits 1
    
- Router interfaces must be enabled
    
- `show ip interface brief` is your best friend
    
