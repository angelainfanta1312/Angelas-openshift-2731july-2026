# Day 1

## Info - Bootloader
<pre>
- we can dual/multi-boot 3~4 OS on the same laptop/desktop
- eg:
  LILO, GRUB, BootCamp
- GRUB2 is the bootloader that is supported by pretty much all the latest linux distros
- only one OS can be active at any point of time
</pre>

## Info - Hypervisor Overview
<pre>
- is nothing but virtualization technology
- virtualization = technolgy that requires hardware + software support
- with virtualization, we can run multiple OS side by side on the same laptop/desktop/workstation/server
- i.e more than one OS can run actively in the same machine
- Multi-Core Processor
  - Server Grade Processors support many cores in a single Processor Chip
  - 256 Physical Cores
  - With Intel Hyperthreading or AMD SMT, each Physical core supports minimum 2 Logical(virtual) cores
  - 1 Processor supports upto 512 Logic Cores(CPU)
- Server grade motherboards, they support multiple Sockets
- Let's say there is a Server board with 8 Processor Sockets, assume each Processor supports 512 Logical Cores
- Total logical/virtual cores = 512 * 8 = 4096 Logical Cores
- Assume you have laptop with 4 Cores (Quad Core Processor) with 16GB RAM and 500 GB HDD/SDD
  - What is the maximum number of Virtual Machines that can be created in this type of laptop?
    - 8 Virtual Machines can safely RUN
- each Virtual machine must be allocated with dedicated H/W resources
  - CPU
  - RAM
  - Storage 
  - hence, this type of virtualization is called heavy-weight virtualization
- the max number of VMs supported by a machine is mainly decided by CPU Cores, RAM and Storage
- Intel Processor
  - the Virtualization feature set is called VT-X
- AMD Processor
  - the virtualization feature set is called AMD-V
- there are 2 types of hypervisors
  1. Type 1 - a.k.a Bare-metal Hypervisors
     - e.g: VMWare vSphere/vCenter, Linux KVM & Microsoft Hyper-v
     - these are used in Servers & Workstations
     - there is not Host OS requirement
     - the Hypervisor itself acts as a minimal OS
  2. Type 2 - a.k.a Hosted Hypervisors
     - e.g: VMWare Fusion/Workstation, Oracle virtualbox, Parallels, etc.,
     - these are used in Desktops & Laptops
     - the laptop/desktop already comes with a Host OS
     - the virtualization software is installed on top of Host OS
     - it won't offer near native H/W performance
- each VM, represents one fully functional Operating System
</pre>

## Info - Hypervisor High Level Architecture
![type1](HypervisorHighLevelArchitecture.png)

## Info - Containerization Overview
<pre>
- this is application virtualization technology
- each container represents one application process in the OS
- containers are not equivalent to OS
- containers will never be able to replace an OS or virtual machine
- in reality, both containerization and virtualization or used in combination
- Container => Your Application + dependent libraries are bundled into an image, running instance of image is container
- Similarities between Hypervisor and Containerization
  - just like VMs acquire an IP address, each containers get its own IP address(most likely a Private IP address)
  - just like VMs get its own file system (folders & files), containers also has its own filesystem
  - just like VMs get a software defined network stack (7 OSI layers), containers also get a software defined network stack
  - just like OS gets its own Port range, container also get its own port range 0-65535
  - just like VMs get a virtual Network card (NIC), containers also get a virtual network card
- In what ways VMs and Containers are different?
  - Inside VM, an Operating System runs
  - Inside Container, one application will be running
  - hence, comparing Virtual Machine with a Container is technically wrong
</pre>

## Info - Docker High Level Architecture
<pre>
  
</pre>
## Info - Docker Alternatives
<pre>
  
</pre>
