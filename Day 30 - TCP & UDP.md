
## Function of Layer 4 (Transport Layer)

- Provides transparent transfer of data between end hosts.
- Provides (or doesn't provide) various services to applications:
	- reliable data transfer
	- error recovery
	- data sequencing 
	- flow control
- Provides Layer 4 addressing (port numbers).
	- Identify the Application Layer protocol
	- Provides session multiplexing.

- The following ranges have been designated by IANA (Internet Assigned Number Authority)
	- Well-known port numbers: 0-1023
	- Registered port numbers: 1024-49151
	- Ephemeral/private/dynamic port numbers: 49152-65535

![[Pasted image 20260316145616.png]]

![[Pasted image 20260316145710.png]]

> A session is an exchange of data between two or more communication devices.

## TCP (Transmission Control Protocol)

- TCP is connection-oriented.
	- Before actually sending data to the destination host, the two hosts communicate to establish a connection. Once the connection is established, the data exchange begins.
- TCP provides reliable communication.
	- The destination host must acknowledge that it received TCP segment. 
	- If a segment isn't acknowledged, it is sent again.

- TCP provides sequencing.
	- Sequence numbers in the TCP header allow destination hosts to put segments in the correct order even if they arrive out of order.
- TCP provides flow control.
	- The destination host can tell the source host to increase/decrease the rate that data is sent.

### TCP Header

![[Pasted image 20260316150426.png]]

![[Pasted image 20260316150442.png]]

### Source port

- 16 bits = 65536(2^16) available port numbers

### Sequence number / Acknowledgement (if ACK set)

- These two fields provide sequencing and reliable communication.

### Window Size

- The window size is used for flow control.

#### Three-Way Handshake

![[Pasted image 20260316150812.png]]

#### Four-Way Handshake

![[Pasted image 20260316150847.png]]

#### TCP: Sequencing / Acknowledgment 

![[Pasted image 20260316150946.png]]

>Host set a random initial sequence number.

> Forward acknowledgement is used to indicate the sequence number of the next segment the host expects to receive.

#### TCP Retransmission 

![[Pasted image 20260316151128.png]]


### TCP Flow Control: Window Size 

- Acknowledging every single segment, no matter what size, is inefficient.
- The TCP header's Window Size field allows more data to be sent before an acknowledgment is required.
- A 'sliding window' can be used to dynamically adjust how large the window size is.

![[Pasted image 20260316151512.png]]

> In all of these examples, I used very simple sequence numbers. In real situation, the sequence numbers get much larger and do not increase by 1 with each message. For the CCNA, just understand the concepts and don't worry about the exact numbers. 


## UDP (User Datagram Protocol)

- UDP is not connection-oriented.
	- The sending host does not establish a connetin with the destination host before sending data. The data is simply sent.
- UDP does not provide reliable communication.
	- When UDP is used, acknowledgments are not sent for received segments. If a segment is lost, UDP has no mechanism to re-transmit it. Segment are sent 'best-effort'.
- UDP does not provide sequencing.
	- There is no sequence numbers field in the UDP header. If segments arrive out of order, UDP has no mechanism to put them back in order.
- UDP does not provide flow control.
	- UDP has no mechanism like TCP's window size to control the flow of data.

![[Pasted image 20260316152117.png]]

### Comparing TCP & UDP

- TCP provides more features than UDP, but at the cost of additional overhead.
- For application that require reliable communications (for example downloading a file), TCP is preferred.
- For application like real-time voice and video, UDP is preferred.
- There are some applications that use UDP, but provide reliability etc within the application itself.
- Some applications use both TCP & UDP, depending on the situation.

![[Pasted image 20260316152437.png]]

## Port Numbers

**TCP**
- FTP data - 20
- FTP control - 21
- SSH - 22
- Telnet - 23
- SMTP - 25
- HTTP - 80
- POP3 - 110
- HTTPS - 443

**UDP**
- DHCP server - 67
- DHCP client - 68
- TFTP - 69
- SNMP agent - 161
- SNMP manager - 162
- Syslog - 514

**TCP & UDP**
- DNS - 53

# **Quiz**

![[Pasted image 20260316152918.png]]

![[Pasted image 20260316152930.png]]

![[Pasted image 20260316152943.png]]

![[Pasted image 20260316152957.png]]

![[Pasted image 20260316153015.png]]

![[Pasted image 20260316153036.png]]





















