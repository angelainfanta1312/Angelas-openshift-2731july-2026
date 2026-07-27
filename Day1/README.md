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
- is a Linux technology
- Containerization is made possible by some of the linux kernel features
  1. Namespace
     - this helps us isolate one container from the other
  2. Control Groups a.k.a CGroups
     - this helps us apply resource quota restrications on the container level
     - we can restrict how much maximum RAM one container can utilize at the max
- all containers running on the same Host/Guest OS shares the underlying Host/Guest OS Hardwares
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
- Container Softwares
  - Docker, Podman, etc.,
</pre>

## Info - Container Engine
<pre>
- they are high-level user-friendly softwares
- it helps us manage container images and containers without knowing any linux kernel level knowledge
- examples
  - Docker, Podman, etc
- Docker Container Engine under the hood depends on Containerd, which inturn depends on runC Container Runtime
  to manage, Container Images and Containers
</pre>

## Info - Container Runtime
<pre>
- they are low-level not so user-friently utilities that manages container images and containers
- generally, end-users like us, will never use the Container Runtime directly
- examples
  - runC, cRun, CRI-O, rkt(pronounced as Rocket), etc
  
</pre>

## Info - Docker High Level Architecture
![docker](DockerHighLevelArchitecture.png)

## Info - Docker Image
<pre>
- is a blueprint of a Docker container
- For example, to install a Windows 11 Operating System, we download window-os11.iso file from Microsoft website
- Using the windows-os11.iso, we can install Windows 11 on mutiple machines
- Same way, using a single docker image, we can create any number of containers
- For example, using nginx web server image, we can create multiple nginx webserver containers
- Usually one Docker Image, will support one application
</pre>  

## Info - Docker Container
<pre>
- One Container supports one application
- is a running instance of a Container Image
- It holds your application and its dependencies ( libraries and/or web/app server )
- Containers gets an unique name and IP address
</pre>  

## Info - Docker Alternatives
<pre>
- Podman
- Containerd
- LXC
</pre>

## Info - Docker Registry
<pre>
- is a collection of one or more Docker Images
- there are 3 types
  1. Local Docker Registry
     - this is a folder, typically in Linux /var/lib/docker
  2. Private Docker Registry
     - this can host your Custom Docker Images
     - this can also host public images 
     - this can be setup using Sonatype Nexus or JFrog Artifactory
  3. Remote Docker Registry
     - a.k.a Docker Hub
     - it is a website maintained by Docker Inc organization along with Community support
</pre>

## Info - Installting Docker Community Edition in Ubuntu
```
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo usermod -aG docker $USER
newgrp docker
docker --version
docker images
```

## Lab - Check your docker version
```
docker --version
docker info
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/01d7a159-6cb5-4b99-b79c-b9de3a226eba" />

## Lab - Listing docker images from your local docker registry
```
docker images
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/bcd9bb91-0016-4515-a718-3b7345c5e467" />

Troubleshooting Permission denied error, when it prompts for password type palmeto@123
```
newgrp docker
docker images
```

## Lab - Download docker image from Docker Hub Remote Registry to your Local Docker Registry
```
docker pull hello-world:latest
docker images
```

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/71a487c2-d22a-416e-9ab8-80da942f8807" />

## Lab - Delete a docker image from your local registry
```
docker images | grep hello
docker rmi hello-world:latest
docker images | grep hello
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/04fce26e-051d-4d00-87c6-0ac5d3efc7ea" />

## Lab - Create a container in the foreground(interactive) mode

The command below will create a new container and starts(runs) it
```
docker run -it --name ubuntu1 --hostname ubuntu1 ubuntu:latest /bin/bash
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b51cb239-82be-4607-ab20-45a06eb78b06" />

Note
<pre>
docker - docker client
run - creates and start the container
it - stands for interactive terminal
name - is the name of the docker container which must be unique ( lowercases preferred )
hostname - hostname of the container
ubuntu:latest - is the image name along with its tag/version(latest)
/bin/bash - tells we would like to start bash terminal within the container once it starts running
</pre>

When we exit the terminal from a interactively created container, it exits(stops) the container.

## Lab - Create container in the background(daemon) mode
```
docker run -dit --name ubuntu2 --hostname ubuntu2 ubuntu:latest /bin/bash
```

Listing the running containers
```
docker ps
```

List all containers ( including containers that are not running )
```
docker ps -a
```

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/b0bb426e-6509-4e66-b62f-aaeb9ce2c995" />

## Lab - Getting inside a container shell
```
docker exec -it ubuntu2 /bin/bash
ls -l
hostname
hostname -i
exit
```

Listing and see the container ubuntu2 still continues to run
```
docker ps
```

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/508836f5-1708-4515-9905-244d01e3235f" />

## Lab - Rename a container
```
docker ps
docker rename ubuntu ubuntu1
docker ps
```

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/7e6801cf-57e3-45ae-8c89-53ff542bb44e" />


## Lab - Starting an exited container
```
docker ps -a
docker start ubuntu1 ubuntu2
docker ps
```

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/e7c27623-f477-46e0-b62b-e29df05c504d" />
