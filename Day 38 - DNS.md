- DNS is used to resolve human-readable names (google.com) to IP addresses.
- Machines such as PCs don't use names, they use addresses (ie. IPv4/IPv6).
- Names are much easier for us to use and remember than IP addresses.
- When you type 'youtube.com' into a web browser, your device will ask a DNS server for the IP address of youtube.com.
- The DNS server(s) your device uses can be manually configured or learned via DHCP.

## `ipconfig /all`

![[Pasted image 20260319144440.png]]

![[Pasted image 20260319144658.png]]

![[Pasted image 20260319144803.png]]

- In this case, R1 isn't acting as a DNS server or client. It is simply forwarding packets. No DNS configuration is required on R1.

>DNS 'A' record = Used to map names to IPv4 addresses.
>DNS 'AAAA' record = Used to map names to IPv6 addresses.

- Standard DNS queries/responses typically use UDP. TCP is used for DNS messages greater than 512 bytes. In either case, port 53 is used.
- Devices will same the DNS server's responses to a local DNS cache. This means they don't have to query the server every single time they want to access a particular destination.

![[Pasted image 20260319145507.png]]

![[Pasted image 20260319145726.png]]

![[Pasted image 20260319145806.png]]

![[Pasted image 20260319145828.png]]


## DNS in Cisco IOS

- For hosts in a network to use DNS, you don't need to configure DNS on the routers. They will simply forward DNS messages like any other packets.
- However, a Cisco router can be configured as a DNS server, although it's rare. g
	- If an internal DNS server is used, usually it's a Windows or Linux server.
- A Cisco router can also be configured as DNS client.

![[Pasted image 20260319150127.png]]

![[Pasted image 20260319150343.png]]


- IP address of youtube.com was learned via DNS, that's why it is temp, while other entries were manually configured.

![[Pasted image 20260319150525.png]]


![[Pasted image 20260319150540.png]]



a










