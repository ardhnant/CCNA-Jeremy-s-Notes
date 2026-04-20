
| Purpose                | Command                            | What It Does                     | When You Use It                   |
| ---------------------- | ---------------------------------- | -------------------------------- | --------------------------------- |
| View IP settings       | `ipconfig`                         | Shows basic IP, mask, gateway    | First step in troubleshooting     |
| Detailed config        | `ipconfig /all`                    | Shows MAC, DHCP, DNS, lease info | Checking DHCP / gateway / DNS     |
| Renew DHCP             | `ipconfig /renew`                  | Requests new IP from DHCP server | IP wrong or expired               |
| Release DHCP           | `ipconfig /release`                | Drops current DHCP IP            | Resetting IP                      |
| Flush DNS              | `ipconfig /flushdns`               | Clears DNS cache                 | DNS behaving weird                |
| Test connectivity      | `ping <IP>`                        | Tests reachability               | Verify host/gateway/internet      |
| Test local stack       | `ping 127.0.0.1`                   | Tests TCP/IP stack only          | Checking if OS networking works   |
| Trace path             | `tracert <IP>`                     | Shows router hops to destination | Routing path analysis             |
| Advanced trace         | `pathping <IP>`                    | Shows latency + packet loss      | Detecting where drops occur       |
| DNS query              | `nslookup domain.com`              | Queries DNS server               | Verifying name resolution         |
| View ARP table         | `arp -a`                           | Shows IP-to-MAC mappings         | Checking gateway MAC, VLAN issues |
| Show routing table     | `route print`                      | Displays local routing table     | Checking default route            |
| Add static route       | `route add`                        | Adds manual route                | Lab scenarios                     |
| Delete static route    | `route delete`                     | Removes manual route             | Fix incorrect route               |
| Show connections       | `netstat -an`                      | Shows open ports/sessions        | Checking services                 |
| Show routes (alt)      | `netstat -r`                       | Displays routing table           | Same as route print               |
| Show MAC address       | `getmac`                           | Displays NIC MAC address         | Matching ARP entries              |
| Interface details      | `netsh interface ipv4 show config` | Detailed interface config        | Advanced inspection               |
| Port test (PowerShell) | `Test-NetConnection IP -Port X`    | Tests specific TCP port          | Firewall / service checks         |
| Telnet test            | `telnet IP port`                   | Tests port reachability          | Basic port connectivity           |
