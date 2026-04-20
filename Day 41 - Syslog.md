## Syslog

 - Syslog is an industry standard protocol for message logging.
 - On network devices, Syslog can be used to log events such as changes in interface status (up <>down), changes in OSPF neighbor status (up<>down), system restarts, etc.
 - The messages can be displayed in the CLI, saved in the device's RAM, or sent to an external Syslog server.

![[Pasted image 20260321170338.png]]

- Logs are essential when troubleshooting issues, examining the cause of incidents, etc.
- Syslog and SNMP are both used for monitoring and troubleshooting of devices. They are complementary, but their functionalities are different.

## Syslog Message Format

seq : time stamp : %facility - severity - MENEMONIC : description 

- A 'seq' number indicating the order/sequence of messages.
- A timestamp indicating the time the message was generated.
- 'facility' indicates which process on the device generated the message.
- 'severity' is a number that indicates the severity of logged event.
- 'menemonic' is a short code for the message, indicating what happened.
- 'description' is the detailed information about the even being reported.

> Timestamp and sequence are two fields may or may not be displayed depending on the device's configuration.


## Syslog severity Levels

![[Pasted image 20260321171002.png]]

- Because severities are very subjective, a relay or collector should not assume that all originators have the same definition of severity. - RFC 5424 (The Syslog Protocol)

![[Pasted image 20260321171209.png]]

## Syslog Logging Locations

- Console line: Syslog messages will be displayed in the CLI when connected to the device via the console port. By default, all messages (level 0 - level 7) are displayed.
- VTP lines - Syslog messages will be displayed in the CLI when connected to the device via Telnet/SSH. Disabled by default.
- Buffer - Syslog messaes will be saved to RAM. By default, all messages (level 0 - level 7) are displayed. 
	- You can view the messages with show logging.

- External server: You can configure the device to send Syslog messages to an external server.
(* Syslog server will listen for messages on UDP port 514. )

## Syslog Configuration 

![[Pasted image 20260321171705.png]]

**terminal monitor**
- Even if logging monitor level is enabled, by default Syslog messages will not be displayed when connected via Telnet or SSH.
- For the messages to be displayed, you must use the following command `terminal monitor`
- This command must be used every time you connect to the device via Telnet or SSH. 

 The clean distinction

- **`logging monitor`** → _Router-side setting_
- **`terminal monitor`** → _Your session-side switch_

They work together like a radio station and a listener 📻

 Think of it like this

- `logging monitor`  
     “Router is allowed to **broadcast logs to VTY lines**”
- `terminal monitor`  
     “My session is ready to **receive and display those logs**”


**logging synchronous** 
- By default, logging messages displayed in the CLI while you are in the middle of typing a command will result in something like this:

![[Pasted image 20260321172024.png]]

- To prevent this, you should use the logging synchronous on the appropriate line. 
![[Pasted image 20260321172114.png]]

- This will cause a new line to be printed if your typing is interrupted by a message.
![[Pasted image 20260321172154.png]]


## `service timestamps / service sequence-numbers`

![[Pasted image 20260321172241.png]]


## VTY 

Similar to console VTY is a virtual interface which can be connected to other interfaces via SSH and Telnet wirelessely. 

- It can have max 16 host connected at same time on a single router.

## Syslog vs SNMP 

- Syslog and SNMP are both used for monitoring and troubleshooting of devices. They are complementary, but their functionalities are different.

- Syslog is used for message logging.
	- Events that occur within the system are categorized based on facility/severity and logged. 
	- Used for system management, analysis, and troubleshooting.
	- Messages are sent from the devices to the server. The server can't actively pull information from the devices (like SNMP Get) or modify variables (like SNMP Set).

- SNMP is used to retrieve and organize information about the SNMP managed devices. 
	- IP addresses, current interface status, temperature, CPU usage, tec.
	- SNMP servers can use Get query the clients and Set to modify variables on the clients.