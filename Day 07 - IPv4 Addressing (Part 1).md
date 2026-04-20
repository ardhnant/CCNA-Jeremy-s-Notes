You watched a 40-minute networking lecture and want notes that capture everything important but still readable. So basically you want the distilled version without the instructor voice, repetition, or YouTube survival rituals. Fine. This is actually a good way to study CCNA, so I’ll cooperate.

I’m keeping the structure consistent with your previous notes so your brain doesn’t revolt later when revising.

---

# **1. Introduction to IPv4 Addressing**

IPv4 addressing operates at the **OSI Layer 3 (Network Layer)** and is responsible for communication **between different networks**, not just within a single LAN.

### Purpose of Layer 3

- Provides **logical addressing** using IP addresses
    
- Enables communication between devices on different networks
    
- Performs **path selection** (choosing best route to destination)
    
- Implemented by **routers**
    

### Layer 2 vs Layer 3 Addressing

|Feature|Layer 2 (Data Link)|Layer 3 (Network)|
|---|---|---|
|Address type|MAC Address|IP Address|
|Assignment|Manufacturer assigned|Manually or dynamically configured|
|Scope|Local network (LAN)|Between networks|
|Devices|Switch|Router|

---

# **2. Networks, LANs, and Routers**

### Single Network (Without Router)

- Switches extend a LAN but **do not create new networks**
    
- All devices share:
    
    - Same network address
        
    - Same broadcast domain
        
- Example:
    
    - Network: `192.168.1.0/24`
        
    - Hosts:
        
        - 192.168.1.1
            
        - 192.168.1.2
            
        - 192.168.1.3
            
        - 192.168.1.4
            

Broadcast behavior:

- Broadcast MAC address = `FF:FF:FF:FF:FF:FF`
    
- Switch forwards broadcast to all ports except incoming port
    
- All hosts receive the frame
    

---

### Multiple Networks (With Router)

Adding a router:

- Separates networks
    
- Creates different Layer 3 networks
    

Example:

- Network 1: `192.168.1.0/24`
    
- Network 2: `192.168.2.0/24`
    

Router requirements:

- Each interface needs an IP address in its connected network
    

Example:

- R1 G0/0 → 192.168.1.254
    
- R1 G0/1 → 192.168.2.254
    

Important:

- Broadcast traffic **does not cross routers**
    
- Broadcast domain ends at router interface
    

---

# **3. IPv4 Address Basics**

### IPv4 Characteristics

- Length: **32 bits**
    
- Equal to **4 bytes**
    
- Written as four decimal numbers separated by dots
    

Example:

```
192.168.1.254
```

Each section:

- 8 bits
    
- Called an **octet**
    

---

### IPv4 Structure

```
8 bits | 8 bits | 8 bits | 8 bits
```

Example:

```
192 = 11000000
168 = 10101000
1   = 00000001
254 = 11111110
```

---

# **4. Number Systems Overview**

## Decimal (Base 10)

- Uses digits 0–9
    
- Each position increases by power of 10
    

Example:

```
3294 = 3×1000 + 2×100 + 9×10 + 4×1
```

---

## Hexadecimal (Base 16)

- Uses digits 0–9 and A–F
    
- Each position increases by power of 16
    
- Common in networking and computing
    

---

## Binary (Base 2)

- Uses only 0 and 1
    
- Each bit doubles in value from right to left
    

Binary values per octet:

|Bit Position|Value|
|---|---|
|1|128|
|2|64|
|3|32|
|4|16|
|5|8|
|6|4|
|7|2|
|8|1|

---

# **5. Binary and Decimal Conversion**

## Binary → Decimal

Steps:

1. Write bit values above digits
    
2. Add values where bit = 1
    

Example:

```
10001111
= 128 + 8 + 4 + 2 + 1
= 143
```

---

## Decimal → Binary

Steps:

1. Start from 128
    
2. Subtract largest possible value
    
3. Write 1 if used, 0 if not
    
4. Continue until remainder is 0
    

Example:

```
127 = 01111111
```

---

### Binary Range of One Octet

- Minimum: `00000000` = 0
    
- Maximum: `11111111` = 255
    

Therefore:

- Each IPv4 octet range = **0–255**
    

---

# **6. Network Portion vs Host Portion**

IPv4 address consists of:

- **Network portion** → identifies network
    
- **Host portion** → identifies device within network
    

Determined by **prefix length**.

---

## Prefix Length Notation

Written using slash:

```
/24
```

Meaning:

- First 24 bits = network portion
    
- Remaining bits = host portion
    

Example:

```
192.168.1.254/24
```

- Network portion → 192.168.1
    
- Host portion → 254
    

---

### Common Prefix Examples

|Prefix|Network Bits|Host Bits|Network Portion|
|---|---|---|---|
|/8|8|24|First octet|
|/16|16|16|First two octets|
|/24|24|8|First three octets|

Devices in same network:

- Same network portion
    
- Different host portion
    

---

# **7. IPv4 Address Classes**

IPv4 addresses historically divided into classes based on first octet.

## Class Ranges

| Class | First Bits | First Octet Range | Default Prefix |
| ----- | ---------- | ----------------- | -------------- |
| A     | 0          | 0–126*            | /8             |
| B     | 10         | 128–191           | /16            |
| C     | 110        | 192–223           | /24            |
| D     | 1110       | 224–239           | Multicast      |
| E     | 1111       | 240–255           | Experimental   |

*127 reserved for loopback.

---

### Class Characteristics

| Class | Networks | Hosts per Network          |
| ----- | -------- | -------------------------- |
| A     | Few      | Very large (~16.7 million) |
| B     | Medium   | Medium (~65k)              |
| C     | Many     | Small (~256)               |

Actual usable hosts:

```
Total hosts − 2
```

because:

- First address = Network address
    
- Last address = Broadcast address
    

Example:

- Class C usable hosts = 254
    

---

# **8. Loopback Addresses**

Range:

```
127.0.0.0 – 127.255.255.255
```

Purpose:

- Test local network stack
    
- Traffic never leaves device
    
- Device sends traffic to itself
    

Example:

```
127.0.0.1
```

Characteristics:

- Used for testing connectivity
    
- Round-trip time ≈ 0 ms
    

---

# **9. Prefix Length vs Netmask**

Two ways to represent network size.

## Prefix Length (CIDR)

```
192.168.1.0/24
```

## Netmask (Dotted Decimal)

|Prefix|Netmask|
|---|---|
|/8|255.0.0.0|
|/16|255.255.0.0|
|/24|255.255.255.0|

Rule:

- Network bits = all 1s
    
- Host bits = all 0s
    

Both represent the same information.

---

# **10. Network and Broadcast Addresses**

## Network Address

- Host portion = all 0s
    
- Identifies network itself
    
- Cannot be assigned to host
    

Example:

```
192.168.1.0/24
```

---

## Broadcast Address

- Host portion = all 1s
    
- Sends traffic to all hosts in network
    
- Cannot be assigned to host
    

Example:

```
192.168.1.255/24
```

Broadcast packet behavior:

- Destination IP = broadcast address
    
- Destination MAC = FF:FF:FF:FF:FF:FF
    

---

## Usable Address Range Example

Network:

```
192.168.1.0/24
```

|Type|Address|
|---|---|
|Network|192.168.1.0|
|First usable|192.168.1.1|
|Last usable|192.168.1.254|
|Broadcast|192.168.1.255|

---

# **11. Key Concepts Summary**

- IPv4 addresses are 32-bit logical addresses
    
- Written in dotted decimal for readability
    
- Consist of network and host portions
    
- Prefix length determines network size
    
- Routers separate networks and stop broadcasts
    
- Binary understanding is essential for subnetting
    
- Network address = all host bits 0
    
- Broadcast address = all host bits 1
    

---

# **12. Commands Used in the Video**

The lecture barely uses commands because it’s theory-heavy, but one appears.

|Command|Purpose|Description|
|---|---|---|
|`ping <IP address>`|Test connectivity|Sends ICMP echo request to verify reachability. Used with loopback addresses to test local TCP/IP stack|

---

These notes now line up with your previous OSI notes logically. Next videos will start tying this into routing decisions and subnetting, where people suddenly discover binary wasn’t optional after all. You’re doing it in the correct order, which is rare and mildly impressive.

## **Quiz Focus (Day 7)**

- Binary ↔ Decimal conversion
    
- Octet ranges
    
- Prefix length meaning
    
- Network vs host identification
    

No shortcuts. This is muscle memory work.

---
