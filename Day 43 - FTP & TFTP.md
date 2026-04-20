
## FTP & TFTP 

- FTP (File Transfer Protocol) and TFTP (Trivial File Transfer Protocol) and industry standard protocols used to transfer files over a network.
- They both use a client-server model.
	- Clients can use FTP or TFTP to copy files from a server.
	- Clients can use FTP or TFTP to copy files to a server
- As a network engineer, the most common use for FTP/TFTP is in the process of upgrading the operating system of a network device.
- You can use FTP/TFTP to download the newer version of IOS from a server, and then reboot the device with the new IOS image.

![[Pasted image 20260324120214.png]]


## Trivial File Transfer Protocol

- TFTP was first standardized in 1981.
- Named 'Trivial' because it is simple and has only basic features compared to FTP.
	- Only allows a client to copy a file to or from a server.
- Was released after FTP, but is not a replacement for FTP. It is another tool to use when lightweight simplicity is more important than functionality.
- No authentication (username/PW), so server will respond to all TFTP requests.
- No encryption, so all data is sent in plain text.
- Best used in a controlled environment to transfer small files quickly.
- TFTP server listen on UDP Port 69.
- UDP is connectionless and doesn't reliability with retransmissions.
- However, TFTP has similar built-in features within the protocol itself.

### TFTP Reliability 

- Every TFTP data message is acknowledged.
	- If the client is transferring a file to the server, the server will send Ack messages.
	- If the server is transferring a file to the client, the client will send Ack messages.
- Timers are used, and if an expected message isn't received in time, the waiting device will re-send previous message.

>TFTP uses 'lock-step' communication. The client and server alternately send a message and then wait for a reply. (+retransmissions are sent as needed)

![[Pasted image 20260324121036.png]]

### TFTP 'Connections' 

- TFTP file transfers have three phases:
	1. **Connections:** TFTP client sends a request to the server, and the server responds back, initializing the connection.
	2. **Data Transfer:** The client and server exchange TFTP messages. One sends data and the other sends acknowledgments.
	3. **Connection Termination:** After the last data message has been sent, a final acknowledgment is sent to terminate the connection.

![[Pasted image 20260324121630.png]]

### TFTP TID

- When the client sends the first message to the server, the destination port is UDP 69 and the source is a random ephemeral port.
- This random port is called a 'Transfer Identifier' (TID) and identifies and data transfer.
- This server then also selects a random TID to use as the source port when it replies, not 69.
- When the client sends the next message, the destination port will be the server's TID, not 69.

![[Pasted image 20260324121934.png]]


## File Transfer Protocol

- FTP was first standardized in 1971.
- FTP uses TCP port 20 and 21.
- Username and passwords are used for authentication, however there is no encryption.
- For greater security, FTPS (FTP over SSL/TLS) can be used.
- SSH File Transfer Protocol (SFTP) can also be used for greater security.
- FTP is more complex than TFTP and allows not only file transfers, but clients can also navigate file directories, add and remove directories, list files, etc.
- The client sends FTP commands to the server to perform these functions.

### FTP Control Connections

- FTP uses two types of connections:
	- An FTP control connection (TCP 21) is established and used to send FTP commands and replies.
	- When files or data are to be transferred, separate FTP data (TCP 20) connections are established and terminated as needed.

![[Pasted image 20260324122551.png]]


### Active Mode FTP Data Connections

- The default method of establishing FTP data connections is active mode, in which the server initiates TCP connection

![[Pasted image 20260324122844.png]]


### Passive Mode FTP Data Connections

- In FTP passive mode, the client initiates the data connections. This often necessary when the client is behind a firewall, which could block the incoming connection from the server.

- Firewalls usually don't permit 'outside' devices to initiate connections. In this case, FTP passive mode is used and the client (behind the firewalls) initiates the TCP connection.

![[Pasted image 20260324123512.png]]

## IOS File Systems

- A file system is a way of controlling how data is stored and retrieved.
- You can view the file systems of a Cisco IOS device with `show file systems`

![[Pasted image 20260324123649.png]]

**disk** - storage devices such as flash memory.
**opaque** - used for internal functions
**nvram** - internal NVRAM. The startup-config file is stored here.
**network** - represents external file systems, for example external FTP/TFTP servers.

### Upgrading Cisco IOS

- You can view the current version of IOS with `show version`
![[Pasted image 20260324124012.png]]

- You can view the contents of flash with `show flash`
![[Pasted image 20260324124049.png]]

![[Pasted image 20260324124102.png]]


### Copying files from TFTP server to flash

![[Pasted image 20260324124125.png]]

### booting IOS with the file

![[Pasted image 20260324124143.png]]

![[Pasted image 20260324124202.png]]

![[Pasted image 20260324124219.png]]

## Command Review

![[Pasted image 20260324124343.png]]