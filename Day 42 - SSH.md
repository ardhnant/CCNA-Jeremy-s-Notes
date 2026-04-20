
## Console Port Security - login

- By default, no password is needed to access the CLI of a Cisco IOS device via the console port.
- You can configure a password on the console line.
	- A user will have to enter a password to access the CLI via the console port.

![[Pasted image 20260321212614.png]]

### `login local`

- Alternatively, you can configure the console line to require users to login using one of the configured usernames on the device.

![[Pasted image 20260321212733.png]]

## Layer 2 Switch - Management IP 

- Layer 2 switches don't perform packet routing and don't build a routing table. They aren't IP routing aware.
- However, you can assign an IP address to an SVI to allow remote connections to the CLI of the switch (using Telnet or SSH).

![[Pasted image 20260321212923.png]]

![[Pasted image 20260321212941.png]]


## Telnet

- Telnet (Teletype Network) is a protocol used to remotely access the CLI of a remote host.
- Telnet was developed in 1969.
- Telnet has been largely replaced by SSH, which is more secure.
- Telnet sends data in plain txt. No encryption!

![[Pasted image 20260321213146.png]]

### Telnet Configuration 

![[Pasted image 20260321213223.png]]

![[Pasted image 20260321213240.png]]

![[Pasted image 20260321213259.png]]


## SSH (Secure Shell)

- SSH (Secure Shell) was developed in 1995 to replace less secure protocols like Telnet.
- SSHv2, a major revision of SSHv1, was released in 2006.
- If a device supports both version 1 and version 2, it is said to run 'version 1.99'.
- Provides security features such as data encryption and authentication.

![[Pasted image 20260321214114.png]]

> The SSH server (the device being connected to) listens to SSH traffic on TCP port 22.


## SSH Configuration: Check SSH Support

![[Pasted image 20260321214311.png]]

- IOS images that support SSH will have 'K9' in their name.
- Cisco exports NPE (No Payload Encryption) IOS images to countries that have restriction on encryption technologies.
- NPE IOS images do not support cryptographic features such as SSH.

### SSH Configuration: RSA Keys

- To enable and use SSH, you must generate and RSA public and private key pair.
- The keys are used for data encryption/decryption, authentication, etc.

- The FQDN of the device is used to name the RSA keys.
FQDN = Fully Qualified Domain Name (host name + domain name)

![[Pasted image 20260321214908.png]]

- Generate the RSA keys. `crypto key generate rsa modulus <length>` is an alternative command.

> length must be 768 bits are greater for SSHv2


## SSH Configuration: VTY Lines

![[Pasted image 20260321214957.png]]


## SSH Configuration 

1. Configure host name
2. Configure DNS domain name
3. Generate RSA key pair
4. Configure enable PW, username/PW
5. Enable SSHv2 (only)
6. Configure VTY lines

Connect: `ssh -l <username> <ip address>` OR `ssh username@ip address`

## Command Summary

![[Pasted image 20260321215417.png]]

## Quiz

![[Pasted image 20260321215625.png]]

![[Pasted image 20260321215638.png]]

![[Pasted image 20260321215654.png]]

![[Pasted image 20260321215710.png]]

![[Pasted image 20260321215725.png]]

![[Pasted image 20260321215741.png]]

