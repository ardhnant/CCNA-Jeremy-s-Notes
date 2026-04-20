## Virtual Machines

Apps running on a server without virtualization:

![[Pasted image 20260402200744.png]]

![[Pasted image 20260402200815.png]]

![[Pasted image 20260402200830.png]]

- Virtual Machines (VMs) allow multiple OS's to run on a single physical server.
- A Hypervisor is used to manage and allocate hardware resources to each VM.  
    - Type 1 Hypervisors (aka Native or Bare-metal) run directly on top of hardware.  
    - Type 2 Hypervisors (aka Hosted) run on top of a Host OS (ie. Windows).
   
- Type 1 Hypervisors are widely used in data center environments.
   
- Type 2 Hypervisors are commonly used on personal devices.  
    - ie. running a virtual network lab on your PC using Cisco Modeling Labs (CML).
   
- The OS in each VM can be the same or different (Windows, Linux, macOS, etc).
- Bins/Libs are the software libraries/services needed by the Apps running in each VM.
- A VM allows its app/apps to run in an isolated environment, separate from the apps in other VMs.

- VMs are easy to create, delete, move, etc.  
    - A VM can be easily saved and moved between different physical servers.

![[Pasted image 20260402201036.png]]


## Containers 

![[Pasted image 20260402201117.png]]

- Containers are software packages that contain an App and all dependencies (Bins/Libs in the diagram) for the contained App to run.  
    - Multiple Apps can be run in a single container, but this is not how containers are usually used.
   
- Containers run on a Container Engine (ie. Docker Engine).  
    - The container engine is run on a host OS (usually Linux).
   
- Containers are lightweight (small in size) and include only the dependencies required to run the specific App.
    
- A Container Orchestrator is a software platform for automating the deployment, management, scaling etc. of containers.  
    - Kubernetes (originally designed by Google) is the most popular container orchestrator.  
    - Docker Swarm is Docker’s container orchestration tool.
   
- In small numbers manual operation is possible, but large-scale systems (ie. with Microservices) can require thousands of containers.

> Microservice Architecture is an approach to software architecture that divides a larger solution into smaller parts (microservices).  
   Those microservices all run in containers that can be orchestrated by Kubernetes (or another platform).




