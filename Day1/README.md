# Day 1

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
     e.g: VMWare vSphere/vCenter, Linux KVM & Microsoft Hyper-v
  2. Type 2 - a.k.a Hosted Hypervisors
     e.g: VMWare Fusion/Workstation, Oracle virtualbox, Parallels, etc.,
</pre>

## Info - Hypervisor High Level Architecture
<pre>  
</pre>
## Info - Containerization Overview
<pre>
  
</pre>
## Info - Docker High Level Architecture
<pre>
  
</pre>
## Info - Docker Alternatives
<pre>
  
</pre>
