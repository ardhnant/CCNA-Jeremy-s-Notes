
| Speed    | Name         | AKA        | Max Length | Official IEEE Standard |
| -------- | ------------ | ---------- | ---------- | ---------------------- |
| 10 Mbps  | Ethernet     | 10BASE-T   | 100 m      | 802.3i                 |
| 100 Mbps | FastEthernet | 100BASE-T  | 100 m      | 802.3u                 |
| 1 Gbps   | Gigabit      | 1000BASE-T | 100 m      | 802.3ab                |
| 10 Gbps  | 10-Gig       | 10GBASE-T  | 100 m      | 802.3an                |

| Speed   | Name        | Fiber   | Max Distance | Official IEEE Standard |
| ------- | ----------- | ------- | ------------ | ---------------------- |
| 1 Gbps  | 1000BASE-LX | MMF/SMF | 550 m / 5 km | 802.3z                 |
| 10 Gbps | 10GBASE-SR  | MMF     | 400 m        | 802.3ae                |
| 10 Gbps | 10GBASE-LR  | SMF     | 10 km        | 802.3ae                |
| 10 Gbps | 10GBASE-ER  | SMF     | 30 km        | 802.3ae                |

| OSI Layer | PDU Name |
| --------- | -------- |
| 7–5       | Data     |
| 4         | Segment  |
| 3         | Packet   |
| 2         | Frame    |
| 1         | Bits     |

| TCP/IP Layer | Equivalent OSI Layers | Description                           |
| ------------ | --------------------- | ------------------------------------- |
| Application  | 7, 6, 5               | Protocols for user apps (HTTP, DNS)   |
| Transport    | 4                     | TCP/UDP                               |
| Internet     | 3                     | IP addressing + routing               |
| Link         | 2 & 1                 | MAC addressing, physical transmission |


Terminal Settings

| Parameter    | Value    |
| ------------ | -------- |
| Baud (speed) | 9600 bps |
| Data bits    | 8        |
| Parity       | None     |
| Stop bits    | 1        |
| Flow control | None     |


Ethernet Header Fields

| Field                       | Size    | Purpose                       |
| --------------------------- | ------- | ----------------------------- |
| Preamble                    | 7 bytes | Clock synchronization         |
| SFD (Start Frame Delimiter) | 1 byte  | Marks start of frame          |
| Destination MAC             | 6 bytes | Receiver                      |
| Source MAC                  | 6 bytes | Sender                        |
| Type / Length               | 2 bytes | L3 protocol OR payload length |
| FCS                         | 4 bytes | Error detection (CRC)         |


| Hex    | Decimal | Protocol |
| ------ | ------- | -------- |
| 0x0800 | 2048    | IPv4     |
| 0x86DD | 34525   | IPv6     |
| 0x0806 |         | ARP      |
| 0x8100 |         | 802.1Q   |


Class Ranges

| Class | First Bits | First Octet Range | Default Prefix |
| ----- | ---------- | ----------------- | -------------- |
| A     | 0          | 0–126*            | /8             |
| B     | 10         | 128–191           | /16            |
| C     | 110        | 192–223           | /24            |
| D     | 1110       | 224–239           | Multicast      |
| E     | 1111       | 240–255           | Experimental   |
_127.0.0.0/8 reserved for loopback._

#### IPv4 Header

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


|VLAN|Purpose|
|---|---|
|1|Default VLAN|
|1002–1005|Legacy technologies (FDDI, Token Ring)|


#### VLAN Tag (802.1Q)

|Field|Subfield|Length|Value / Range|Purpose|
|---|---|---|---|---|
|TPID (Tag Protocol Identifier)|—|16 bits (2 bytes)|0x8100|Identifies the frame as IEEE 802.1Q tagged (marks where the VLAN tag begins)|
|TCI (Tag Control Information)|PCP (Priority Code Point)|3 bits|0–7|Defines Class of Service (CoS) for traffic prioritization|
|TCI (Tag Control Information)|DEI (Drop Eligible Indicator)|1 bit|0 or 1|Indicates if frame can be dropped during congestion|
|TCI (Tag Control Information)|VID (VLAN ID)|12 bits|0–4095|Identifies the VLAN number|

|Type|Range|
|---|---|
|Normal VLANs|1 – 1005|
|Extended VLANs|1006 – 4094|

#### Bridge ID structure

| Component                    | Size    |
| ---------------------------- | ------- |
| Bridge Priority              | 4 bits  |
| Extended System ID (VLAN ID) | 12 bits |
| MAC Address                  | 48 bits |
![[Pasted image 20260408180730.png]]

![[Pasted image 20260408180739.png|697]]

| Parameter / Scenario                | STP (802.1D)                             | RSTP (802.1w)                                   |
| ----------------------------------- | ---------------------------------------- | ----------------------------------------------- |
| **Hello Time**                      | 2 sec (root sends BPDUs)                 | 2 sec (every switch sends BPDUs)                |
| **Max Age**                         | 20 sec (BPDU expires)                    | ~6 × Hello = 12 sec (logical, rarely relied on) |
| **Forward Delay**                   | 15 sec                                   | 15 sec (used only in fallback cases)            |
| **Blocking → Forwarding**           | 30 sec (15 listening + 15 learning)      | Immediate (if Proposal/Agreement succeeds)      |
| **Root Port Failure Detection**     | 20 sec (Max Age expiry)                  | ~3 × Hello ≈ 6 sec worst case (often faster)    |
| **Root Failure Convergence**        | 50 sec (20 + 15 + 15)                    | <1 sec typical (≤6 sec worst case detection)    |
| **Alternate Path Activation**       | Not immediate (needs full recalculation) | Immediate (alternate port precomputed)          |
| **Indirect Failure (Backbone)**     | 20–50 sec (unless BackboneFast)          | <1–6 sec (no special feature needed)            |
| **Point-to-Point Link Convergence** | Timer-based (~30 sec)                    | ~1 ms to sub-second (Proposal/Agreement)        |
| **Edge Port (PortFast)**            | Immediate (with PortFast only)           | Immediate by default (edge port)                |
| **Shared Link (Half-duplex)**       | 30 sec                                   | Falls back to 30 sec                            |
| **BPDU Generation**                 | Root only (others relay)                 | Every switch generates BPDUs                    |
| **Failure Detection Method**        | Timer expiration (Max Age)               | Missing BPDUs (event-driven)                    |
| **Topology Change Propagation**     | TCN + slow propagation                   | TC flag, immediate propagation                  |
| **MAC Table Update**                | Aging reduced to 15 sec                  | Immediate/rapid flush                           |
#### IEEE Standards


| Protocol                    | Standard |
| --------------------------- | -------- |
| STP                         | 802.1D   |
| RSTP                        | 802.1w   |
| Multiple STP                | 802.1s   |
| Per-VLAN Spanning Tree Plus | 802.1D   |
| Rapid PVST+                 | 802.1w   |
![[Pasted image 20260408181345.png]]


| Protocol / Topic      | Multicast Address | Purpose / Logic                                                             |
| --------------------- | ----------------- | --------------------------------------------------------------------------- |
| OSPF                  | 224.0.0.5         | All OSPF routers, used for hellos and LSAs (neighbor discovery & adjacency) |
| OSPF                  | 224.0.0.6         | DR/BDR communication to optimize flooding on multi-access networks          |
| FHRP (HSRP v1)        | 224.0.0.2         | Hello messages for redundancy                                               |
| FHRP (HSRP v1)        | 0000.0c07.acXX    | Virtual MAC Address XX is HSRP group number                                 |
| FHRP (HSRP v2 / GLBP) | 224.0.0.102       | Hello messages for redundancy protocols                                     |
| FHRP (HSRP v2 / GLBP) | 0000.0c9f.fXXX    | Virtual MAC Address XXX is HSRP group number                                |
| FHRP (VRRP)           | 224.0.0.18        | Multicast communication for failover                                        |
|                       |                   |                                                                             |
| IPv6 Multicast        | FF00::/8          | Entire IPv6 multicast range                                                 |
| IPv6                  | FF02::1           | All nodes (hosts)                                                           |
| IPv6                  | FF02::2           | All routers                                                                 |
| IPv6                  | FF02::5           | All OSPF routers                                                            |
| IPv6                  | FF02::A           | All EIGRP routers                                                           |
| IPv6 Scope            | FF01::            | Interface-local scope                                                       |
| IPv6 Scope            | FF02::            | Link-local scope                                                            |
| IPv6 Scope            | FF05::            | Site-local scope                                                            |
| IPv6 Scope            | FF08::            | Organization-local scope                                                    |
| IPv6 Scope            | FF0E::            | Global scope                                                                |
| IPv6 (NDP)            | FF02::1:FFxx:xxxx | Solicited-node multicast (targeted address resolution)                      |
![[Pasted image 20260408223923.png]]

![[Pasted image 20260408184021.png]]

| Port     | Protocol Type | Service / Protocol | Description                                           |
| -------- | ------------- | ------------------ | ----------------------------------------------------- |
| 20, 21   | TCP           | FTP                | File transfer (20 = data, 21 = control)               |
| 22       | TCP           | SSH                | Secure CLI access                                     |
| 23       | TCP           | Telnet             | Insecure CLI access                                   |
| 25       | TCP           | SMTP               | Sending email                                         |
| 53       | TCP/UDP       | DNS                | Name resolution (UDP mostly, TCP for large responses) |
| 67, 68   | UDP           | DHCP               | Automatic IP assignment (67 = server, 68 = client)    |
| 69       | UDP           | TFTP               | Lightweight file transfer                             |
| 80       | TCP           | HTTP               | Unencrypted web traffic                               |
| 110      | TCP           | POP3               | Retrieving email                                      |
| 161, 162 | UDP           | SNMP               | Network monitoring and management                     |
| 443      | TCP           | HTTPS              | Secure web traffic                                    |
| 514      | UDP           | Syslog             | Logging messages                                      |
|          | UDP           | RADIUS             |                                                       |
|          | TCP           | TACACS+            |                                                       |

IPv6

| Type / Concept             | Prefix / Format    | Purpose                   | Key Notes                                                  |
| -------------------------- | ------------------ | ------------------------- | ---------------------------------------------------------- |
| Standard Prefix Length     | /64                | Network + Interface split | First 64 bits = network, last 64 = host (SLAAC sweet spot) |
| Example (Docs)             | 2001:DB8::/64      | Documentation range       | Used in labs, not routable                                 |
| Global Unicast             | 2000::/3           | Public IPv6 addresses     | Internet-routable (starts with 2 or 3)                     |
| Unique Local               | FC00::/7           | Private addressing        | Not internet routable (FD00::/8 commonly used)             |
| Link Local                 | FE80::/10          | Local link communication  | Auto-assigned, never routed                                |
| Multicast                  | FF00::/8           | One-to-many communication | Replaces broadcast                                         |
| Multicast Scope            | FF01::             | Interface-local           | Limited to interface                                       |
| Multicast Scope            | FF02::             | Link-local                | Most important scope                                       |
| Multicast Scope            | FF05::             | Site-local                | Within site                                                |
| Multicast Scope            | FF08::             | Organization-local        | Org-wide                                                   |
| Multicast Scope            | FF0E::             | Global                    | Global multicast                                           |
| Anycast                    | (No fixed prefix)  | Nearest device routing    | Uses unicast addresses                                     |
| Solicited-Node Multicast   | FF02::1:FFxx:xxxx  | NDP (ARP replacement)     | Last 24 bits of unicast appended                           |
| Full Solicited-Node Format | FF02::1:FF00:0/104 | Defined prefix            | Used for address resolution                                |
