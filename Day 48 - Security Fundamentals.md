## Why security?

- What is the purpose/goal of security in an enterprise?
- The principles of the CIA Triad form the foundation of security:

- Confidentiality
    - Only authorized users should be able to access data.
    - Some information/data is public and can be accessed by anyone, some is secret and should only be accessed by specific people.

- Integrity
    - Data should not be tampered with (modified) by unauthorized users.
    - Data should be correct and authentic.

- Availability
    - The network/systems should be operational and accessible to authorized users.

- Attackers can threaten the confidentiality, integrity, and availability of an enterprise’s systems and information.

## Vulnerability, Exploit, Threat, Mitigation

- A vulnerability is any potential weakness that can compromise the CIA of a system/info.
    - A potential weakness isn’t a problem on its own.

- An exploit is something that can potentially be used to exploit the vulnerability.
    - Something that can potentially be used as an exploit isn’t a problem on it’s own.

- A threat is the potential of a vulnerability to be exploited.
    - A hacker exploiting a vulnerability in your system is a threat.

- A mitigation technique is something that can protect against threats.
    - Should be implemented everywhere a vulnerability can be exploited: client devices, servers, switches, routers, firewalls, etc.


> No system is perfectly secure!

## Common Attacks

1. DoS(denial of service) attacks
2. Spoofing attacks
3. Reflection/amplification attacks
4. Reconnaissance attacks
5. Malware
6. Social engineering attacks
7. Password-related attacks

### Denial-of-service attacks

- DoS attacks threaten the availability of a system.

- One common DoS attack is the TCP SYN flood.
    - TCP three-way handshake: SYN | SYN-ACK | ACK
    - The attacker sends countless TCP SYN messages to the target.
    - The target sends a SYN-ACK message in response to each SYN it receives.
    - The attacker never replies with the final ACK of the TCP three-way handshake.
    - The incomplete connections fill up the target’s TCP connection table.
    - The attacker continues sending SYN messages.
    - The target is no longer able to make legitimate TCP connections.

![[Pasted image 20260327131824.png]]

- In DDoS(Distributed Denial Of Service) attack, the attacker infects many target computers with malware and uses them all to initiate a denial-of-service attack, for example a TCP SYN flood attack.
- This group of infected computers is called a botnet.

![[Pasted image 20260327132023.png]]

### Spoofing attacks

- To spoof an address is to use a fake source address (IP or MAC address).
- Numerous attacks involve spoofing, it's not a single kind of attack.
- An example is a DHCP exhaustion attack.
- An attacker uses spoofed MAC addresses to flood DHCP Discover messages.
- The target server's DHCP pool becomes full, resulting in a denial-of-service to other devices.

![[Pasted image 20260327132250.png]]

### Reflection/Amplification attacks

- In a reflection attacks, the attacker sends traffic to a reflector, and spoofs the source address of its packets using the target's IP address.
- The reflector (ie. a DNS server) sends the reply to the target's IP address.
- If the amount of traffic sent to the target is large enough, this can result in a denial-of-service.
- A reflection attack becomes an amplification attack when the amount of traffic sent by the attacker is small, but it triggers a large amount of traffic to be sent from the reflector to the target.

![[Pasted image 20260327133210.png]]


### Man in the middle attacks

- In a man-in-the-middle-attack, the attacker places himself between the source and destination to eavesdrop on communication, or to modify traffic before it reaches the destination.
- A common example is ARP spoofing, also known as ARP poisoning.
- A host sends an ARP request, asking for the MAC address of another device.
- The target of the request sends an ARP reply, informing the requester of its MAC address.
- The attacker waits and sends another ARP reply after the legitimate replier.
- If the attacker's ARP reply arrives last, it will overwrite the legitimate ARP entry in PC1's ARP table.

![[Pasted image 20260327133643.png]]

- In PC1's ARP table, the entry for 10.0.0.1 will have the attacker's MAC address. 
- When PC1 tries to send traffic to SRV1, it will be forwarded to the attacker instead.
- The attacker can inspect the messages, and then forward them on to SRV1.
- The attacker can also modify the messages before forwarding them to SRV1.
- The compromises the Confidentiality and Integrity of Communications between PC1 and SRV1.

![[Pasted image 20260327133944.png]]

### Reconnaissance attacks 

- Reconnaissance attacks aren't attacks themselves, but they are used to gather information about a target which can be used for a future attack.
- This is often publicly available information.
- ie. nslookup to learn the IP address of a site:
- ![[Pasted image 20260327134223.png]]

- Or a `WHOIS` query to learn email addresses, phone numbers, physical addresses, etc.

### Malware

- Malware (malicious software) refers to a variety of harmful programs that can infect a computer.

- Viruses infect other software (a ‘host program’). The virus spreads as the software is shared by users. Typically they corrupt or modify files on the target computer.

- Worms do not require a host program. They are standalone malware and they are able to spread on their own, without user interaction. The spread of worms can congest the network, but the ‘payload’ of a worm can cause additional harm to target devices.

- Trojan Horses are harmful software that is disguised as legitimate software. They are spread through user interaction such as opening email attachments, or downloading a file from the Internet.

- The above malware types can exploit various vulnerabilities to threaten any of the CIA of the target device.

 _there are many types of malware!_

### Social Engineering attacks

- Social engineering attacks target the most vulnerable part of any system – people!

- They involve psychological manipulation to make the target reveal confidential information or perform some action.
   
- Phishing typically involves fraudulent emails that appear to come from a legitimate business (Amazon, bank, credit card company, etc) and contain links to a fraudulent website that seems legitimate. Users are told to login to the fraudulent website, providing their login credentials to the attacker.
    - spear phishing is a more targeted form of phishing, ie. aimed at employees of a certain company.
    - whaling is phishing targeted at high-profile individuals, ie. a company president.


- Vishing (voice phishing) is phishing performed over the phone.
    - ‘Hi, this is Jeremy from the IT department. Due to company policy we need to reset your password, could you tell me the password you’re currently using and I’ll reset it for you?’

- Smishing (SMS phishing) is phishing using SMS text messages.
   
- Watering hole attacks compromise sites that the target victim frequently visits. If a malicious link is placed on a website the target trusts, they might not hesitate to click it.
   
- Tailgating attacks involve entering restricted, secured areas by simply walking in behind an authorized person as they enter. Often, the target will hold the door open for the attacker to be polite, assuming the attacker is also authorized to enter.

![[Pasted image 20260327134624.png]]

### Passwords-related attacks

- Most systems use a username/password combination to authenticate users.
   
- The username is often simple/easy to guess (for example the user’s email address), and the strength and secrecy of the password is relied on to provide the necessary security.
   
- Attackers can learn a user’s passwords via multiple methods:
    - Guessing
    - Dictionary attack: A program runs through a ‘dictionary’ or list of common words/passwords to find the target’s password.
    - Brute force attack: A program tries every possible combination of letters, numbers, and special characters to find the target’s password.
   
- Strong passwords should contain:
    - at LEAST 8 characters (preferably more)
    - a mixture of UPPERCASE and lowercase letters
    - a mixture of letters and numbers
    - one or more special characters (# @ ! ? etc.)
    - should be changed regularly

![[Pasted image 20260327134825.png]]


## Multi-factor authentication (MFA)

- Multi-factor authentication involves providing more than just a username/password to prove your identity.
   
- It usually involves providing two of the following (=two-factor authentication):
   
- Something you know
    - a username/password combination, a PIN, etc.
   
- Something you have
    - pressing a notification that appears on your phone, a badge that is scanned, etc.
   
- Something you are
    - biometrics such as a face scan, palm scan, fingerprint scan, retina scan, etc.
  
- Requiring multiple factors of authentication greatly increases the security. Even if an attacker learns the target’s password (something you know), they won’t be able to login to the target’s account.

## Digital Certification 

- Digital certificates are another form of authentication used to prove the identity of the holder of the certificate.
- They are used for websites to verify that the website being accessed is legitimate.
- Entities that want a certificate to prove their identity send a CSR (Certificate Signing Request) to a CA (Certificate Authority), which will generate and sign the certificate.

![[Pasted image 20260327135204.png]]


## Controlling and monitoring users with AAA

- AAA (triple-A) stands for Authentication, Authorization, and Accounting.
- It is a framework for controlling and monitor users of a computer system (ie. a network).
- Authentication is the process of verifying a user’s identity.
    - logging in = authentication
   
- Authorization is the process of granting the user the appropriate access and permissions.
    - granting the user access to some files/services, restricting access to other files/services = authorization
   
- Accounting is the process of recording the user’s activities on the system.
    - logging when a user makes a change to a file = accounting
   
- Enterprises typically use a AAA server to provide AAA services.
    - ISE (Identity Services Engine) is Cisco’s AAA server.
   
- AAA servers usually support the following two AAA protocols:
    - RADIUS: an open standard protocol. Uses UDP ports 1812 and 1813.
    - TACACS+: A Cisco proprietary protocol. Uses TCP port 49.
   
- For the CCNA, know the differences between Authentication, Authorization, and Accounting.


## Security Program Elements

- User awareness programs are designed to make employees aware of potential security threats and risks.
    - For example, a company might send out false phishing emails to make employees click a link and sign in with their login credentials.
    - Although the emails are harmless, employees who fall for the false emails will be informed that it is part of a user awareness program and they should be more careful about phishing emails.
   
- User training programs are more formal than user awareness programs.
    - For example, dedicated training sessions which educate users on the corporate security policies, how to create strong passwords, and how to avoid potential threats.
   
- Physical access control protects equipment and data from potential attackers by only allowing authorized users into protected areas such as network closets or data center floors.
    - Multifactor locks can protect access to restricted areas.
        - ie. a door that requires users to swipe a badge and scan their fingerprint to enter.
        - Permissions of the badge can easily be changed, for example permissions can be removed when an employee leaves the company.


# **Quiz**

![[Pasted image 20260327135457.png]]

![[Pasted image 20260327135509.png]]

![[Pasted image 20260327135520.png]]

![[Pasted image 20260327135531.png]]

![[Pasted image 20260327135543.png]]

![[Pasted image 20260327135555.png]]








