## **1. Introduction to Subnetting**

### **What is Subnetting?**

Subnetting is the process of dividing a larger IP network into smaller networks called **subnets**.

Subnetting is:

- A major CCNA topic
    
- A core practical skill for network engineers
    
- Not difficult when learned step-by-step
    

Purpose of subnetting:

- Use IP address space efficiently
    
- Reduce wasted addresses
    
- Allow networks to be sized appropriately for requirements
    

---

## **2. Review — IPv4 Address Classes (Classful Addressing)**

Before CIDR, IPv4 addressing used **classes**.

IPv4 address:

- 32 bits total
    
- 4 octets (8 bits each)
    

### **Class Ranges**

|Class|First Octet Range|Leading Bits|Default Prefix|
|---|---|---|---|
|A|0–127|0|/8|
|B|128–191|10|/16|
|C|192–223|110|/24|
|D|224–239|1110|Special use|
|E|240–255|1111|Experimental|

### **Important Notes**

- Only **Class A, B, and C** are assignable to devices.
    
- Class D → multicast
    
- Class E → experimental
    
- Some ranges reserved (example: 127.0.0.0/8 loopback)
    

---

## **3. Default Network and Host Portions**

In classful addressing:

|Class|Network Portion|Host Portion|
|---|---|---|
|A|First octet|Last 3 octets|
|B|First 2 octets|Last 2 octets|
|C|First 3 octets|Last octet|

This determines:

- Number of networks available
    
- Number of hosts per network
    

---

### **Address Capacity by Class**

|Class|Number of Networks|Hosts per Network|
|---|---|---|
|A|128 (fewer usable)|16,777,216|
|B|16,384|65,536|
|C|2,097,152|256|

Formula:

```
Number of addresses = 2^(host bits)
```

Example:

- Class A → 24 host bits → 2²⁴ addresses
    

---

## **4. IP Address Allocation (IANA)**

IP addresses are assigned by:

- **IANA (Internet Assigned Numbers Authority)**
    

Organizations receive address blocks based on size:

- Large companies → Class A or B
    
- Small companies → Class C
    

---

## **5. Problem with Classful Addressing**

Classful addressing caused **massive address waste**.

### **Example 1 — Point-to-Point Network**

Network: `203.0.113.0/24`

Total addresses:

- 256 total
    
- Network address → 1
    
- Broadcast address → 1
    
- Router addresses → 2
    

Result:

- 252 addresses wasted
    

A point-to-point link only needs **2 addresses**.

---

### **Example 2 — Company Needs 5000 Hosts**

- Class C → too small (256 addresses)
    
- Class B → assigned (~65,000 addresses)
    

Result:

- About 60,000 addresses wasted
    

---

### **Why This Happened**

When IPv4 was designed:

- Internet growth was underestimated
    
- 4 billion addresses seemed enough
    
- Address exhaustion later became a major problem
    

---

## **6. CIDR (Classless Inter-Domain Routing)**

Introduced by the **IETF in 1993**.

CIDR removed class rules:

- Class A must be /8
    
- Class B must be /16
    
- Class C must be /24
    

Instead:

- Any prefix length can be used
    

Example:

```
/25, /26, /27, etc.
```

Benefits:

- Flexible network sizing
    
- Efficient address usage
    
- Less wasted address space
    

---

## **7. Subnetting Basics**

Subnetting:

- Divides a larger network into smaller networks
    
- Each smaller network is called a **subnet**
    

Key idea:

- Extend network portion
    
- Reduce host portion
    

Subnet mask:

- 1s → network bits
    
- 0s → host bits
    

---

## **8. Calculating Usable Hosts**

### **Formula**

```
Usable hosts = 2^(host bits) − 2
```

Subtract 2 because:

- Network address (all host bits = 0)
    
- Broadcast address (all host bits = 1)
    

---

### **Example — 203.0.113.0/24**

Host bits = 8

```
2^8 − 2 = 254 usable hosts
```

Too many for a point-to-point link.

---

## **9. Changing Prefix Length (Subnet Size Reduction)**

As prefix length increases:

- Network portion increases
    
- Host portion decreases
    
- Available addresses decrease
    

|Prefix|Subnet Mask|Host Bits|Usable Hosts|
|---|---|---|---|
|/25|255.255.255.128|7|126|
|/26|255.255.255.192|6|62|
|/27|255.255.255.224|5|30|
|/28|255.255.255.240|4|14|
|/29|255.255.255.248|3|6|
|/30|255.255.255.252|2|2|

---

## **10. Point-to-Point Networks**

### **/30 Networks**

- 4 total addresses
    
- 2 usable addresses
    
- Perfect for two routers
    

Example:

```
203.0.113.0/30
Range: .0 – .3
```

- .0 → network
    
- .1 → router
    
- .2 → router
    
- .3 → broadcast
    

No wasted addresses.

Remaining addresses from original /24 can be used for other subnets.

---

### **/31 Networks (Special Case)**

Normally:

```
2^1 − 2 = 0 usable hosts
```

But for point-to-point links:

- Network and broadcast addresses are unnecessary
    
- Both addresses can be assigned
    

Example:

```
203.0.113.0/31
```

- .0 → R1
    
- .1 → R2
    

More efficient than /30.

Cisco routers allow this with a warning.

---

## **11. /32 Prefix Length**

Subnet mask:

```
255.255.255.255
```

Characteristics:

- No host bits
    
- Represents a single IP address
    

Not used for normal interfaces.

Common use:

- Static routes to a specific host
    

---

## **12. CIDR Notation**

CIDR notation:

```
/prefix-length
```

Example:

```
/25, /26, /27
```

Represents number of network bits.

Previously only dotted decimal masks were used.

---

## **13. Purpose of Subnetting (Key Idea)**

Instead of assigning:

```
203.0.113.0/24
```

to one network,

we divide it into smaller subnets:

- /30 or /31 for point-to-point links
    
- Larger subnets for LANs
    

Result:

- Efficient address usage
    
- Reduced waste
    
- Flexible network design
    

---

## **14. Subnetting Design Example**

Given:

```
192.168.1.0/24
```

Requirements:

- 4 networks
    
- 45 hosts per network
    
- Router interface included
    

### **Step 1 — Required Addresses**

Per subnet:

- 45 hosts
    
- +2 (network + broadcast)
    

Required = 47 addresses

Total needed:

```
47 × 4 = 188 addresses
```

Available:

```
256 addresses
```

Enough space exists.

---

### **Step 2 — Choose Subnet Size**

Test prefix lengths:

|Prefix|Usable Hosts|Enough?|
|---|---|---|
|/30|2|No|
|/29|6|No|
|/28|14|No|
|/27|30|No|
|/26|62|Yes|

Result:

- /26 chosen
    

Notes:

- Subnets may contain unused addresses
    
- Extra space allows future growth
    

---

## **15. Subnetting Method Hint**

Process:

1. Find broadcast address of subnet
    
2. Next address becomes next subnet’s network address
    
3. Repeat for remaining subnets
    

---

## **16. Key Takeaways (Day 13)**

- CIDR removes strict class rules
    
- Subnetting divides networks into smaller parts
    
- Host formula = **2^host_bits − 2**
    
- Increasing prefix length reduces host count
    
- /30 commonly used for point-to-point
    
- /31 more efficient for point-to-point links
    
- /32 represents a single host
    
- Subnetting improves address efficiency
    
