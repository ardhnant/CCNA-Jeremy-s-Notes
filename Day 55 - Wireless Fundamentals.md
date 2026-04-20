- Although we will briefly look at other types of wireless networks, in this section of the course we will be focusing on wireless LANs using Wi-Fi.
- The standards we use for wireless LANs are defined in IEEE 802.11.
- The Wi-Fi is a trademark of the Wi-Fi Alliance, not directly connected to the IEEE.
- The WiFi Alliance tests and certifies equipment for 802.11 standards compliance interoperability with other devices.
- However, WiFi has become the common term that people use to refer to 802.11 wireless LANs, and I will use both terms.

![[Pasted image 20260401233955.png]]


## Wireless Networks

 Wireless networks have some issues that we need to deal with.
1. All devices within range receive all frames, like devices connected to an Ethernet hub.
	- Privacy of data within the LAN is a greater concern.
	- CSMA/CA (Carrier Sense Multi Access with Collision Avoidance) is used to facilitate half-duplex communications.
	- CSMA/CD is used in wired networks to detect and recover from collisions.
	- CSMA/CA is used in wireless networks to avoid collisions.
	- When using CSMA/CA, a device will wait for other devices to stop transmitting before it transmits data itself.
![[Pasted image 20260401234554.png]]

2. Wireless communications are regulated by various international and national bodies.
3. Wireless signal coverage area must be considered.
	- Signal range 
	- Signal absorption, reflection, refraction, diffraction and scattering.

4. Other devices using the same channels can cause interference.
- For example, a wireless LAN in your neighbor's house/apartment.

![[Pasted image 20260401234128.png]]

![[Pasted image 20260401234146.png]]

![[Pasted image 20260401234157.png]]


 **Signal Absorption** 
- Absorption happens when a wireless signal passes throug a material and is converted into heat, weakening the original signal.
![[Pasted image 20260401235015.png]]


**Signal Refraction**
Reflection happens when a signal bounces off of a material, for example metal.
- This is why WiFi reception is usually poor in elevators. The signal bounces off the metal and very little penetrates into the elevator.
![[Pasted image 20260401235221.png]]


**Signal Refraction**
Refraction happens when a wave is bent wen entering a medium where the signal travels at a different speed.
- For example, glass and water can refract waves.

![[Pasted image 20260401235341.png]]


**Signal Deffraction**
Diffraction happens when a wave encounters an obstacle and travels around it.
- This can result in 'blind spots' behind the obstacle.

![[Pasted image 20260401235523.png]]


**Signal Scattering**
Scattering happens when a material causes a signal to scatter in all directions.
- Dust, smog, uneven surfaces, etc. can cause scattering.

![[Pasted image 20260401235639.png]]


## Radio Frequency 

- To send wireles signals, the sender applies an alternating current to an antenna.
	- This creates electromagnetic fields which propagate out as waves.
- Electromagnetic waves can be measured in multiple ways for example amplitude and frequency.
- Amplitude is the maximum strength of the electric and magnetic fields.

![[Pasted image 20260402000047.png]]

- Frequency measures the number of up/down cycles per a given unit of time.
- The most common measurement of frequency is hertz.
	- Hz (Hertz) = cycles per second
	- kHz (kilohertz) = 1,000 cycle per seconds
	- MHz (Megahertz) = 1,000,000 cycle per second
	- GHz (Gigahertz) = 1,000,000,000 cycle per second
	- THz (Terahertz) = 1,000,000,000,000 cycle per second

![[Pasted image 20260402090835.png]]

- Another important term is period, the amount of time of one cycle.
	- If the frequency is a 4Hz, the period is 0.25 seconds.

- The visible frequency rate is 400THz to 790THz.
- The radio frequency range is from 30Hz to 300GHz and is used from many purposes.

### Radio Frequency Bands

- WiFi uses two main bands (frequency ranges)

- 2.4 GHz band
	- The actual range is 2.400 GHz to 2.4835 GHz
- 5 GHz band 
	- The actual range is from 5.150 GHz to 5.825 GHz

- The 2.4 GHz band typically provides further reach in open space and better penetration of obstacles such as walls.
	- However, more devices tend to use the 2.4 GHz band so interference can be bigger problem compared to the 5 GHz band

- WiFi 6 (802.11ax) has expanded the spectrum range to include a band in 6 GHz range.

## Channels

- In a small wireless LAN with only a single AP (Access Point), you can use any channel.
- However, in larger WLANs with multiple APs, it's important that adjacent APs don't use overlapping channels. This helps avoid interference.
- In the 2.4 GHz band, it is recommended to use channels 1, 6, and 11.

![[Pasted image 20260402094518.png]]

- Outside of North America you could use other combinations, but for the CCNA exam remember 1, 6, and 11.
- The 5GHz band consists of non-overlapping channels, so it is much easier to avoid interference between adjacent APs. To be exact 24 non overlapping channels.

- Using channels 1, 6, and 11 you can place APs in a honeycomb pattern to provide complete coverage of an area without interference between channels.

![[Pasted image 20260402102817.png]]

### 802.11 Standards

![[Pasted image 20260402102913.png]]

## Service Sets

- 802.11 defines different kinds of service sets which are group of wireless network devices.

- There are three main types:
	- Independent 
	- Infrastructure
	- Mesh

- All devices in a service set share the same SSID (service set identifier).
- The SSID is a human-readable name which identifies the service set.
- The SSID does not have to be unique.

### Service Sets: IBSS

- An IBSS (Independent Basic Service Set) is a wireless network in which two or more wireless devices connect directly without using an AP (Access Point).
- Also called an ad hoc network.
- Can be used for file transfer (ie. AirDrop).
- Not scalable beyond a dew devices.

![[Pasted image 20260402105852.png]]

### Service Sets: BSS

- A BSS (Basic Service Set) is a kind of Infrastructure Service Set in which clients connect to each other via an AP(Access Point), but not directly connected to each other.
- A BSSID (Basic Service Set ID) is used to uniquely identify the AP.
	- Other APs can use the same SSID, but not the same BSSID.
	- The BSSID is the MAC address of the APs radio.

- Wireless device request to associate with the BSS.
- Wireless devices that have associated with the BSS are called 'clients' or 'stations'.
- The area around an AP where its signal is usable is called a BSA(Basic Service Area)
 ![[Pasted image 20260402110430.png]]

### Service Sets: ESS

- To create larger wireless LANs beyond the range of a single AP, we use an ESS (Extended Service Set).
- APs whith their own BSSs are connected by a wired network.
	- Each BSS uses the same SSID.
	- Each BSS has a unique BSSID.
	- Each BSS uses a different channel to avoid interference.

- Clients can pass between APs without having to reconnect, providing a seamless WiFi experience when moving between APs.
	- This is called roaming.
- The BSAs should overlab about 10-15%.

![[Pasted image 20260402110918.png]]


### Service Sets: MBSS

- An MBSS (Mesh Basic Service Set) can be used in situations where it's difficult to run an Ethernet connection to every AP.
- Mesh APs use two radios: one to provide a BSS to wireless clients, and one to form a 'blackhaul network' which is used to bridge traffic from AP to AP.
- At least one AP is connected to the wired network, and it is called the RAP (Root Access Point).
- The other APs are called MAPs (Mesh Access Points).
- A protocol is used to determine the best path through the mesh (similar to how dynamic routing protocols are used to determine the best path to a destination).

![[Pasted image 20260402112400.png]]


## Distribution System

- Most wireless networks aren't standalone networks.
	- Rather they are a way for wireless clients to connect to the wired network infrastructuer.
- In 802.11, the upstream wired network is called the DS(Distribution Systems).
- Each wireless BSS or ESS is mapped to a VLAN in the wired network.

![[Pasted image 20260402112618.png]]

- It’s possible for an AP to provide multiple wireless LANs, each with a unique SSID.
- Each WLAN is mapped to a separate VLAN and connected to the wired network via a trunk.
- Each WLAN uses a unique BSSID, usually by incrementing the last digit of the BSSID by one.

![[Pasted image 20260402114713.png]]


## Additional AP Operational Modes

- APs can operate in additional modes beyond the ones we’ve introduced so far.
- An AP in repeater mode can be used to extend the range of a BSS.

- The repeater will simply retransmit any signal it receives from the AP.  
    - A repeater with a single radio must operate on the same channel as the AP, but this can drastically reduce the overall throughput on the channel.  
    - A repeater with two radios can receive on one channel, and then retransmit on another channel.

![[Pasted image 20260402114822.png]]


- A workgroup bridge (WGB) operates as a wireless client of another AP, and can be used to connect wired devices to the wireless network.
- In the example below, PC1 does not have wireless capabilities, and also does not have access to a wired connection to SW1.
- PC1 has a wired connection to the WGB, which has a wireless connection to the AP.

> There are two kinds of WGBs:  
Universal WGB (uWGB) is an 802.11 standard that allows one device to be bridged to the wireless network.  
WGB is a Cisco-proprietary version of the 802.11 standard that allows multiple wired clients to be bridged to the wireless network.


![[Pasted image 20260402115026.png]]


- An outdoor bridge can be used to connect networks over long distances without a physical cable connecting them.
- The APs will use specialized antennas that focus most of the signal power in one direction, which allows the wireless connection to be made over longer distances than normally possible.
- The connection can be point-to-point as in the diagram below, or point-to-multipoint in which multiple sites connect to one central site.

![[Pasted image 20260402115122.png]]


# **Quiz**

![[Pasted image 20260402115147.png]]

![[Pasted image 20260402115214.png]]

![[Pasted image 20260402115338.png]]

![[Pasted image 20260402115349.png]]

![[Pasted image 20260402115406.png]]

![[Pasted image 20260402115459.png]]

![[Pasted image 20260402115514.png]]