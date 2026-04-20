## **1. What This Lesson Is About (Context)**

* Continuation of **Ethernet LAN Switching (Layer 2)**.
* Focus:

  * Ethernet frame details (minimum size, padding)
  * **ARP (Address Resolution Protocol)**
  * How IP + MAC work together inside a LAN
  * ARP tables and MAC address tables
  * Ping (ICMP) behavior
* Still **inside a LAN only**.
* Routing to other networks is **not here yet**.

---

## **2. Ethernet Frame Clarification (Important Exam Detail)**

### Ethernet Header (Strict Definition)

* Sometimes **Preamble + SFD are NOT counted** as part of the Ethernet header.
* Ethernet header (strict):

  * Destination MAC (6 bytes)
  * Source MAC (6 bytes)
  * Type (2 bytes)

### Sizes

| Component              | Size         |
| ---------------------- | ------------ |
| Ethernet Header        | 14 bytes     |
| Ethernet Trailer (FCS) | 4 bytes      |
| Header + Trailer       | **18 bytes** |

---

## **3. Minimum Ethernet Frame Size**

### Rule (Memorize This):

* **Minimum Ethernet frame size = 64 bytes**
* This **does NOT include**:

  * Preamble
  * SFD

### Payload Calculation

* 64 bytes (min frame)
* − 18 bytes (header + trailer)
* = **46 bytes minimum payload**

### Padding

* If payload < 46 bytes:

  * Padding is added
  * Padding bytes are **all zeroes (0x00)**

#### Example:

* Payload = 34 bytes
* Padding added = 12 bytes

---

## **4. Network Topology Used in This Lesson**

* Interfaces upgraded to **GigabitEthernet**:

  * G0/0, G0/1, G0/2
* MAC addresses are now **realistic**
* Same OUI for all PCs → same manufacturer
* Last 4 hex digits used for simplicity

---

## **5. Adding IP Addresses (Why This Matters)**

* Devices communicate using **IP addresses**
* Ethernet (switches) forward using **MAC addresses**
* IPs used:

  * PC1 → 192.168.1.1
  * PC2 → 192.168.1.2
  * PC3 → 192.168.1.3
  * PC4 → 192.168.1.4
* Network:

  * 192.168.1.0/24

---

## **6. The Missing Piece from Day 5: ARP**

* Users send traffic using **IP addresses**
* Ethernet requires **MAC addresses**
* PCs must translate:

  * IP → MAC

This is where **ARP** comes in.

---

## **7. ARP (Address Resolution Protocol)**

### What ARP Does

* Maps:

  * **Layer 3 address (IP)**
  * to **Layer 2 address (MAC)**

### ARP Uses Two Messages

| Message     | Type      | Purpose                 |
| ----------- | --------- | ----------------------- |
| ARP Request | Broadcast | Ask “Who has this IP?”  |
| ARP Reply   | Unicast   | Answer with MAC address |

---

## **8. Broadcast MAC Address**

* **FFFF.FFFF.FFFF**
* Used when:

  * Destination MAC is unknown
  * Frame must reach all hosts in the LAN

---

## **9. ARP Request Process (Step-by-Step)**

![Image](https://marvel-b1-cdn.bc0a.com/f00000000310757/www.fortinet.com/content/dam/fortinet/images/cyberglossary/what-is-arp.jpg)

![Image](https://media.geeksforgeeks.org/wp-content/uploads/20251010151409017059/system_.webp)

### Scenario: PC1 → PC3

1. PC1 knows:

   * PC3 IP: 192.168.1.3
   * Does NOT know MAC
2. PC1 sends **ARP Request**

   * Destination MAC: FFFF.FFFF.FFFF
   * Destination IP: 192.168.1.3
3. Switches:

   * Learn PC1’s MAC (dynamic)
   * Flood frame out all ports (broadcast)
4. All PCs receive request:

   * PC2 & PC4 ignore
   * PC3 accepts (IP matches)

---

## **10. ARP Reply Process**

1. PC3 sends **ARP Reply**

   * Destination MAC: PC1’s MAC
   * Source MAC: PC3’s MAC
2. Frame is **unicast**
3. Switches:

   * Learn PC3’s MAC
   * Forward normally (known unicast)
4. PC1 receives reply
5. PC1 stores entry in **ARP table**

---

## **11. ARP Table (Host Side)**

* Stores:

  * IP → MAC mappings

### Viewing ARP Table

| OS                      | Command    |
| ----------------------- | ---------- |
| Windows / Linux / macOS | `arp -a`   |
| Cisco IOS               | `show arp` |

### ARP Entry Types

| Type    | Meaning         |
| ------- | --------------- |
| Static  | Preconfigured   |
| Dynamic | Learned via ARP |

---

## **12. Ping (ICMP) Basics**

* Used to test **reachability**
* Measures **round-trip time**
* Uses **ICMP**

### Ping Messages

| Message           | Direction         |
| ----------------- | ----------------- |
| ICMP Echo Request | Sender → Receiver |
| ICMP Echo Reply   | Receiver → Sender |

---

## **13. Why First Ping Often Fails**

* ARP must happen **before** ping can work
* First ping may fail because:

  * MAC address not known yet
* After ARP resolution:

  * Subsequent pings succeed

---

## **14. Cisco IOS Ping Behavior**

* Default:

  * 5 echo requests
  * 100 bytes each
* Symbols:

  * `!` = success
  * `.` = failure
* Displays:

  * Success rate
  * Min / Avg / Max RTT

---

## **15. Wireshark Observations (Important)**

![Image](https://media.geeksforgeeks.org/wp-content/uploads/20220808161040/ARPWiresharkfilter.png)

![Image](https://learningnetwork.cisco.com/servlet/rtaImage?eid=ka06e000000dMym\&feoid=00N3i00000D6DDX\&refid=0EM6e000004EAJh)

### Ethernet Type Values

| Protocol | Hex    |
| -------- | ------ |
| IPv4     | 0x0800 |
| ARP      | 0x0806 |
| IPv6     | 0x86DD |

### Padding Example

* Ping size: 36 bytes
* Minimum payload: 46 bytes
* Padding added: 10 bytes (20 hex zeroes)

---

## **16. MAC Address Table (Switch Side)**

### View Command

```
show mac address-table
```

### Fields

| Field       | Meaning          |
| ----------- | ---------------- |
| VLAN        | Virtual LAN ID   |
| MAC Address | Learned address  |
| Type        | Dynamic / Static |
| Ports       | Interface        |

---

## **17. MAC Address Aging**

* Default:

  * **5 minutes**
* If no traffic:

  * Entry removed
* Entry is relearned automatically when traffic resumes

---

## **18. Clearing MAC Address Table (Cisco Switch)**

### Clear ALL dynamic entries

```
clear mac address-table dynamic
```

### Clear specific MAC address

```
clear mac address-table dynamic address <mac>
```

### Clear entries on a specific interface

```
clear mac address-table dynamic interface <interface>
```

---
Packet switching - Data is broken into small chunks called _packets_
- Each packet travels independently through the network
- Routers decide the best path **hop by hop**

Frame rewrite means:
- The Layer 2 header (MAC addresses) is changed at each hop
- The IP packet inside stays the same (mostly 👀)
- Happens when a router forwards a packet to the next device

Forwarding of packets from source to destination or node to node while travelling to destination is called **packet hopping.**

---

# **Quiz Questions and Answers**

## **1. You see long zero bytes at the end of an Ethernet payload after a 36-byte ping. Why?**

**Reasoning:**
Ethernet enforces a minimum payload size.

---

## **2. Which message is sent to all hosts on the local network?**

**Reasoning:**
Only one protocol requires broadcasting to discover a MAC address.

---

## **3. Which fields appear in a Cisco switch MAC address table?**

**Reasoning:**
The switch tracks more than just MAC and port.

---

## **4. Which frames are flooded by a switch?**

**Reasoning:**
Flooding only happens when the destination cannot be directly resolved.

---

## **5. Which command clears dynamic MAC entries on one interface?**

**Reasoning:**
Cisco provides multiple granular clearing options.

---

## **Answers**

1. **Answer:** **B. Padding bytes**
2. **Answer:** **A. ARP request**
3. **Answer:** **C. VLAN, MAC address, type, and ports**
4. **Answer:** **A. Broadcast and unknown unicast**
5. **Answer:** **D. clear mac address-table dynamic interface interface-id**

---
