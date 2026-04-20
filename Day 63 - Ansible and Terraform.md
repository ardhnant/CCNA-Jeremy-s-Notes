## Configuration drift

- Configuration drift is when individual changes made over time cause a device’s configuration to deviate from the standard/correct configurations as defined by the company.  
    - Although each device will have unique parts of its configuration (IP addresses, host name, etc), most of a device’s configuration is usually defined in standard templates designed by the network architects/engineers of the company.  
    - As individual engineers make changes to devices (for example to troubleshoot and fix network issues, test configurations, etc), the configuration of a device can drift away from the standard.  
    - Records of these individual changes and their reasons aren’t kept.  
    - This can lead to future issues.
- Even without automation tools, it is best to have standard configuration management practices.  
    - When a change is made, save the config as a text file and place it in a shared folder.  
    - A standard naming system like hostname_yyyymmdd might be used.  
    - There are flaws to this system, as an engineer might forget to place the new config in the folder after making changes. Which one should be considered the ‘correct’ config?  
    - Even if configurations are properly saved like this, it doesn’t guarantee that the configurations actually match the standard.

## Configuration Provisioning 

- Configuration provisioning refers to how configuration changes are applied to devices.  
    - This includes configuring new devices, too.
- Traditionally, configuration provisioning is done by connecting to devices one-by-one via SSH.  
    - This is not practical in large networks.
- Configuration management tools like Ansible, Puppet, and Chef allow us to make changes to devices on a mass scale with a fraction of the time/effort.
- Two essential components: templates and variables

![[Pasted image 20260408031012.png]]

## Configuration Management Tools

- Configuration management tools are network automation tools that facilitate the centralized control of large numbers of network devices.
- The options you need to be aware of for the CCNA are Ansible, Puppet, and Chef.
- These tools were originally developed after the rise of VMs, to enable server system admins to automate the process of creating, configuring, and removing VMs.  
    - However, they are also widely used to manage network devices.
- These tools can be used to perform tasks such as:  
    - Generate configurations for new devices on a large scale.  
    - Perform configuration changes on devices (all devices in your network, or a certain subset of devices).  
    - Check device configurations for compliance with defined standards.  
    - Compare configurations between devices, and between different versions of configurations on the same device.


## Ansible

![[Pasted image 20260408031201.png]]

- Ansible is a configuration management tool owned by Red Hat.
- Ansible itself is written in Python.
- Ansible is agentless.  
    - It doesn’t require any special software to run on the managed devices.
- Ansible uses SSH to connect to devices, make configuration changes, extract information, etc.
- Ansible uses a push model. The Ansible server (Control node) uses SSH to connect to managed devices and push configuration changes to them.  
    - Puppet and Chef use a pull model.
- After installing Ansible itself, you must create several text files:  
    - Playbooks: These are files are ‘blueprints of automation tasks’. They outline the logic and actions of the tasks that Ansible should do. Written in YAML.  
    - Inventory: These files list the devices that will be managed by Ansible, as well as characteristics of each device such as their device role (access switch, core switch, WAN router, firewall, etc). Written in INI, YAML, or other formats.  
    - Templates: These files represent a device’s configuration file, but specific values for variables are not provided. Written in Jinja2 format.  
    - Variables: These files list variables and their values. These values are substituted into the templates to create complete configuration files. Written in YAML.


![[Pasted image 20260408031239.png]]

## Infrastructure as Code

- Infrastructure as Code (IaC) is the practice of provisioning and managing infrastructure (servers, networks, cloud resources) using machine-readable configuration files (code) instead of manual configuration (e.g., CLI/GUI).
- Ansible, Puppet, and Chef are examples of IaC configuration management tools.  
    - Ansible manages device configurations using several files.
- Terraform is an IaC-based provisioning tool that automates the creation of infrastructure resources.
- IaC automates infrastructure deployment and management, ensuring consistency, scalability, and repeatability.

![[Pasted image 20260408031610.png]]

## Provisioning vs Management 

- Configuration management (e.g., Ansible, Puppet, Chef)  
    - Manages existing infrastructure by installing software, configuring settings, and maintaining system state.  
    - Ensures consistency by applying and enforcing configurations across multiple devices.
- Infrastructure provisioning (e.g., Terraform)  
    - Creates, modifies, and deletes infrastructure resources such as servers and network infrastructure.  
    - Focuses on initial setup rather than ongoing configuration management.
- Configuration management tools work on already existing systems, whereas provisioning tools build infrastructure from scratch.
- Terraform (provisioning) and Ansible (configuration management) can work together:  
    - Terraform provisions infrastructure (VMs, networks, storage, etc.), and Ansible provides ongoing configuration and management.

![[Pasted image 20260408031720.png]]


## Mutable vs immutable infrastructure 

- Configuration management tools typically use a mutable infrastructure approach.  
    - Infrastructure can be modified after deployment (e.g., applying updates, patches, or configuration changes).  
    - Changes are made in place, meaning existing resources are updated rather than replaced.

![[Pasted image 20260408031922.png]]

- Provisioning tools employ an immutable infrastructure approach.  
    - Infrastructure cannot be changed after deployment.  
    - “Changes” involve replacing the previous resource with a new version.  
    - No configuration drift, since each deployment starts from a fresh, predefined state.

![[Pasted image 20260408031941.png]]

## Procedural vs declarative

- Procedural approach (a.k.a. Imperative)  
    - Follows explicit steps in a specific order to achieve the desired outcome.  
    - The user must define each action to configure the infrastructure.  
    - Provides greater control compared to a declarative approach.
- Declarative approach  
    - Defines the desired end state.  
    - The tool (e.g., Terraform) figures out the steps needed to achieve the goal.  
    - Easier to maintain and ensures consistency across deployments.
- A procedural approach focuses on how to make changes (explicit steps).  
    - “Configure router’s hostname with hostname R1”  
    - “Configure the IP address of G0/1 with ip address 192.168.1.1 255.255.255.0”  
    - “Enable the interface with no shutdown”  
    - etc.
- A declarative approach focuses on what the final state should be.  
    - “Create a router named R1 with IP address 192.168.1.1/24 on its G0/1 interface, ensuring the interface is enabled”.

![[Pasted image 20260408032103.png]]


## Terraform

- Terraform is an open-source IaC tool developed by HashiCorp (acquired by IBM in 2025).
- It is primarily a provisioning tool, focused on deploying infrastructure resources on various cloud and on-prem platforms.  
    - These platforms are called providers and include AWS, Azure, GCP, Kubernetes, and many more (1000+).  
    - This includes integrations with Cisco platforms like Catalyst Center, ACI, and IOS XE.
- Like Ansible, Terraform uses a push model and is agentless; it doesn’t require a software agent on infrastructure it provisions or manages.

![[Pasted image 20260408032202.png]]

- The basic Terraform workflow consist of three main steps:  
    - Write: Define the desired state of your infrastructure resources in configuration files.  
    - Plan: Verify the changes that will be executed before applying them.  
    - Apply: Execute the plan to provision and manage the infrastructure resources.
- While Terraform Core is written in Go, configuration files are written in HashiCorp Configuration Language (HCL).  
    - HCL is an example of a domain-specific language (DSL), a type of language specialized for a particular purpose.  
    - Because DSLs are specialized, they allow users to perform complex tasks with much less effort than a general language (like Python or Go) would require.

# **Quiz**

![[Pasted image 20260408031322.png]]

![[Pasted image 20260408031349.png]]

![[Pasted image 20260408031403.png]]

![[Pasted image 20260408032323.png]]

![[Pasted image 20260408032335.png]]

![[Pasted image 20260408032405.png]]

