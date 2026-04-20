
## **1. Purpose of the IPv4 Header**

The **Internet Protocol Version 4 (IPv4)** header operates at **Layer 3 (Network Layer)** of the OSI model.

Its purpose is to:

- Allow data to be sent between devices on different networks
    
- Enable **routing** across networks, including across the Internet
    
- Provide the information routers need to forward packets to their destination
    

This lesson focuses specifically on the **IPv4 header fields**, not routing configuration itself. Practical routing configuration is covered in later lessons.

---

## **2. Encapsulation Review (Where the IPv4 Header Fits)**

Before understanding the IPv4 header, it is important to review how data is encapsulated.

### **Encapsulation Process**

1. Upper OSI layers generate data.
    
2. A **Layer 4 header** (TCP or UDP) is added.
    
    - Data + Layer 4 header = **Segment**
        
3. A **Layer 3 header (IP header)** is added.
    
    - Segment + Layer 3 header = **Packet**
        
4. A **Layer 2 header and trailer** are added.
    
    - Packet + Layer 2 header/trailer = **Frame**
        

These units are called **Protocol Data Units (PDUs)**.

|Layer|PDU Name|
|---|---|
|Layer 4|Segment|
|Layer 3|Packet|
|Layer 2|Frame|

In this lesson, the focus is on the **Layer 3 PDU (Packet)**.

---

## **3. IPv4 Header Overview**

The IPv4 header contains multiple fields used for routing and packet handling.

Important exam note:

- Memorizing exact bit positions or layout is usually unnecessary.
    
- Understanding the **purpose of each field** is essential.
    

The header is read:

- **Left to right**
    
- **Top to bottom**
    

---

## **4. IPv4 Header Fields**

![[Pasted image 20260220132323.png]]

### **4.1 Version Field**

- Length: **4 bits**
    
- Purpose: Identifies IP version.
    

Values:

- IPv4 → **4 (0100 in binary)**
    
- IPv6 → **6 (0110 in binary)**
    

Note:

- IPv5 existed only as an experimental protocol (Internet Stream Protocol) and was never publicly used.
    

For IPv4 packets, this field is always **4**.

---

### **4.2 Internet Header Length (IHL)**

- Length: **4 bits**
    
- Purpose: Indicates total length of the IPv4 header.
    

Important rule:

- Length is specified in **4-byte increments**.
    

Example:

- IHL = 5 → 5 × 4 bytes = **20 bytes**
    

Values:

- Minimum: 5 (20 bytes)
    
- Maximum: 15 (60 bytes)
    

Meaning:

- Minimum header length = 20 bytes (no options)
    
- Maximum header length = 60 bytes (40-byte options field)
    

---

### **4.3 DSCP (Differentiated Services Code Point)**

- Length: **6 bits**
    
- Used for **QoS (Quality of Service)**.
    

Purpose:

- Prioritizes delay-sensitive traffic such as:
    
    - Voice
        
    - Video streaming
        

Example:

- Voice traffic may be prioritized over web browsing during congestion.
    

---

### **4.4 ECN (Explicit Congestion Notification)**

- Length: **2 bits**
    

Purpose:

- Signals network congestion **without dropping packets**.
    

Normally:

- Congestion is indicated by packet loss.
    

ECN allows:

- Congestion notification without packet drops.
    

Requirement:

- Must be supported by both endpoints and network devices.
    

---

### **4.5 Total Length Field**

- Length: **16 bits**
    
- Indicates total size of the packet in **bytes**.
    

Includes:

- IPv4 header
    
- Layer 4 header
    
- Data
    

Important distinction:

|Field|Measures|
|---|---|
|IHL|Header length only|
|Total Length|Entire packet|

Values:

- Minimum: 20 bytes
    
- Maximum: 65,535 bytes
    

---

### **4.6 Identification Field**

- Length: **16 bits**
    

Purpose:

- Identifies fragments belonging to the same original packet.
    

When fragmentation occurs:

- All fragments share the same identification value.
    
- Allows reassembly at the destination.
    

Fragmentation occurs when:

- Packet size exceeds the **MTU (Maximum Transmission Unit)**.
    

Typical Ethernet MTU:

- **1500 bytes**
    

Fragments are reassembled by the **receiving host**.

---

### **4.7 Flags Field**

- Length: **3 bits**
    
- Controls fragmentation behavior.
    

Bits:

|Bit|Name|Function|
|---|---|---|
|0|Reserved|Always 0|
|1|DF (Don’t Fragment)|Prevents fragmentation when set|
|2|MF (More Fragments)|Set to 1 if more fragments follow|

Rules:

- MF = 1 → more fragments exist
    
- MF = 0 → last fragment or unfragmented packet
    

---

### **4.8 Fragment Offset**

- Length: **13 bits**
    

Purpose:

- Indicates the fragment’s position in the original packet.
    

Allows:

- Correct reassembly even if fragments arrive out of order.
    

---

### **4.9 Time To Live (TTL)**

- Length: **8 bits**
    

Purpose:

- Prevents infinite routing loops.
    

Operation:

- Each router decreases TTL by 1.
    
- Packet is dropped when TTL reaches 0.
    

Originally:

- Represented time in seconds.
    

In practice:

- Represents **hop count**.
    

Common default TTL:

- **64**
    

---

### **4.10 Protocol Field**

- Length: **8 bits**
    
- Identifies the encapsulated Layer 4 protocol.
    

Important protocol numbers:

|Protocol|Number|
|---|---|
|ICMP|1|
|TCP|6|
|UDP|17|
|OSPF|89|

Notes:

- ICMP is used by ping.
    
- OSPF is a dynamic routing protocol.
    

---

### **4.11 Header Checksum**

- Length: **16 bits**
    

Purpose:

- Detects errors in the IPv4 header only. --impo
    

Process:

1. Router calculates checksum.
    
2. Compares with value in header.
    
3. If mismatch → packet dropped.
    

Important:

- IP relies on encapsulated protocol to detect errors in encapsulated data.
    
- Both TCP/UDP have their own checksum fields to detect errors in the encapsulated data. --very impo
    

---

### **4.12 Source and Destination IP Address**

- Length: **32 bits each**
    

Purpose:

- Source IP → sender address
    
- Destination IP → intended receiver
    

These fields were covered in earlier lessons.

---

### **4.13 Options Field**

- Variable length:
    
    - 0 to 40 bytes (0–320 bits)
        

Notes:

- Rarely used in practice.
    
- Present only if IHL > 5.
    
- Not important for CCNA memorization.
    

---

## **5. Fragmentation Summary**

   

| **Field**                                | **Size in byte/bits** | **Description**                                                                                                                                                           |
| ---------------------------------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Version                                  | 4 bits                | IPv4 - 0100, IPv6 - 0110                                                                                                                                                  |
| Internet Header Length (IHL)             | 4 bits                | 4-byte increment, max - 15, min - 5, header only length                                                                                                                   |
| Differentiated Service Code Point (DSCP) | 6 bits                | used for Quality of Service, Prioritize sensitive traffic.                                                                                                                |
| Explicit Congestion Notification(ECN)    | 2 bits                | Signals network congestion without dropping packets, must be supported by both endpoints and network devices                                                              |
| Total Length Field                       | 16 bits               | Length of entire packet, min - 20 bytes, max - 65535 bytes                                                                                                                |
| Identification Field                     | 16 bits               | Identify fragment belonging to the same original packet, all fragment share the same identification value, fragmentation occur when exceeds MTU, typical MTU - 1500 bytes |
| Flags Field                              | 3 bits                | 0 - reserved, 1 - don't fragment, 2 - more fragment                                                                                                                       |
| Fragment Offset                          | 13 bits               | Indicate the fragment position in the original packet                                                                                                                     |
| Time to Live (TTL)                       | 8 bits                | Prevent infinite routing loops, decrease ttl by 1, default is 64                                                                                                          |
| Protocol Field                           | 8 bits                | Identifies the encapsulated layer 4 protocol, ICMP - 1, TCP - 6, UDP 17, OSPF 89                                                                                          |
| Header Checksum                          | 16 bits               | Detects error only in the IPv4 header only, relies on encapsulated protocol to detect errors in encapsulated data, TCP/UDP have their own field to detect errors          |
| Source and Destination IP address        | 32 bits each          |                                                                                                                                                                           |
| Options Field                            | 0 to 40 bytes         | Present only if IHL > 5, not impo for CCNA                                                                                                                                |

---

## **6. Wireshark Packet Analysis (Conceptual Understanding)**

Wireshark allows visualization of packet structure.

![[Pasted image 20260210182920.png]]

Observed in packet capture:

- Entire frame shown in hexadecimal
    
- Ethernet header section
    
- IPv4 header section
    
- Payload (example: ICMP)
    

Example observations:

- Version field = 4 (0100)
    
- IHL value of 5 = 20-byte header
    
- DSCP and ECN often set to 0
    
- Total length shows packet size
    
- Identification matches across fragments
    
- Flags show fragmentation state
    
- TTL decreases across hops
    
- Protocol value identifies ICMP, TCP, or UDP
    

Large ping example:

- Ping size increased to 10,000 bytes
    
- Packet fragmented into MTU-sized fragments
    
- MF bit set on non-final fragments
    
- Different fragment offsets observed
    

---

