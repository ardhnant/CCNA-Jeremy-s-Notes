## Port Security 

- Port security is a security feature of Cisco switches.
- It allows you to control which source MAC address(es) are allowed to enter the switchport.
- If an unauthorized source MAC address enters the port, an action will be taken.
	- The default action is to place the interface is an 'err-disabled' state.

![[Pasted image 20260327112706.png]]

![[Pasted image 20260327112727.png]]

- When you enable port security on an interface with the default settings, one MAC address is allowed.  
    - You can configure the allowed MAC address manually.  
    - If you don’t configure it manually, the switch will allow the first source MAC address that enters the interface.
- You can change the maximum number of MAC addresses allowed.
- A combination of manually configured MAC addresses and dynamically learned addresses is possible.

![[Pasted image 20260327112930.png]]

## Why port security?

- Port security allows network admins to control which devices are allowed to access the network.

- However, MAC address spoofing is a simple task.  
    - It’s easy to configure a device to send frames with a different source MAC address.
	
- Rather than manually specifying the MAC addresses allowed on each port, port security’s ability to limit the number of MAC addresses allowed on an interface is more useful.

- Think of the DHCP starvation attack carried out in the Day 48 Lab video.  
	- the attacker spoofed thousands of fake MAC addresses  
    - the DHCP server assigned IP addresses to these fake MAC addresses, exhausting the DHCP pool  
    - the switch’s MAC address table can also become full due to such an attack

- Limiting the number of MAC addresses on an interface can protect against those attacks.

## Enabling Port Security

![[Pasted image 20260327113241.png]]

![[Pasted image 20260327113307.png]]

### `show port-security interface`

![[Pasted image 20260327113435.png]]

![[Pasted image 20260327113449.png]]

![[Pasted image 20260327113514.png]]

then we add a alien device with the interface

![[Pasted image 20260327113548.png]]

![[Pasted image 20260327113612.png]]


### Re-enabling an interface (manually)

![[Pasted image 20260327113646.png]]


### Re-enabling an interface (ErrDisable Recovery)

![[Pasted image 20260327113742.png]]

> Every 5 minutes (by default, all err-disabled interfaces will be re-enabled it err-disable recovery has been enabled for the cause of the interface's disablement.)


![[Pasted image 20260327113936.png]]

ErrDisable Recovery is useless if you don't remove the device that caused the interface to enter the err-disabled state!

## Violation Modes

- There are three different violation modes that determine what the switch will do if an unauthorized frame enters an interface configured with port security.
- Shutdown
    - Effectively shuts down the port by placing it in an err-disabled state.
    - Generates a Syslog and/or SNMP message when the interface is disabled.
    - The violation counter is set to 1 when the interface is disabled.

- Restrict
    - The switch discards traffic from unauthorized MAC addresses.
    - The interface is NOT disabled.
    - Generates a Syslog and/or SNMP message each time an unauthorized MAC is detected.
    - The violation counter is incremented by 1 for each unauthorized frame.

- Protect
    - The switch discards traffic from unauthorized MAC addresses.
    - The interface is NOT disabled.
    - It does NOT generate Syslog/SNMP messages for unauthorized traffic.
    - It does NOT increment the violation counter.

### Violation mode: Restrict 

![[Pasted image 20260327114344.png]]

![[Pasted image 20260327114356.png]]

### Violation mode: Protect

![[Pasted image 20260327115316.png]]

## Secure MAC address aging

![[Pasted image 20260327115353.png]]

- By default secure MAC addresses will not ‘age out’ (Aging Time : 0 mins)
    - Can be configured with `switchport port-security aging time` minutes

- The default aging type is Absolute
    - Absolute: After the secure MAC address is learned, the aging timer starts and the MAC is removed after the timer expires, even if the switch continues receiving frames from that source MAC address.
    - Inactivity: After the secure MAC address is learned, the aging timer starts but is reset every time a frame from that source MAC address is received on the interface.
    - Aging type is configured with `switchport port-security aging type {absolute | inactivity}`

- Secure Static MAC aging (addresses configured with switchport port-security mac-address x.x.x) is disabled by default.
    - Can be enabled with `switchport port-security aging static`


![[Pasted image 20260327115649.png|697]]

## Sticky Secure MAC Addresses

- ‘Sticky’ secure MAC address learning can be enabled with the following command:  
    `SW1(config-if)# switchport port-security mac-address sticky`

- When enabled, dynamically-learned secure MAC addresses will be added to the running config like this:  
    `switchport port-security mac-address sticky mac-address`

- The ‘sticky’ secure MAC addresses will never age out.
    - You need to save the running-config to the startup-config to make them truly permanent (or else they will not be kept if the switch restarts)

- When you issue the `switchport port-security mac-address sticky` command, all current dynamically-learned secure MAC addresses will be converted to sticky secure MAC addresses.

- If you issue the `no switchport port-security mac-address sticky` command, all current sticky secure MAC addresses will be converted to regular dynamically-learned secure MAC addresses.

![[Pasted image 20260327115900.png]]

![[Pasted image 20260327115910.png]]


## MAC Address Table

- Secure MAC addresses will be added to the MAC address table like any other MAC address.
	- Sticky and Static secure MAC addresses will have a type of STATIC
	- Dynamically-learned secure MAC addresses will have a type of DYNAMIC
	- You can view all secure MAC address with `show mac address-table secure`
![[Pasted image 20260327120155.png]]


## Command Review 

![[Pasted image 20260327120233.png]]

# **Quiz**

![[Pasted image 20260327120301.png]]

![[Pasted image 20260327120313.png]]

![[Pasted image 20260327120333.png]]

![[Pasted image 20260327120352.png]]

![[Pasted image 20260327120412.png]]

![[Pasted image 20260327120433.png]]





















