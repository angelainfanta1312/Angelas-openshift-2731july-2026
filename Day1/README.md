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

## Lab - Stopping containers
```
docker ps
docker stop ubuntu1 ubuntu2
docker start ubuntu1 ubuntu2
docker ps
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/df7fff96-9ea9-4827-a5de-05523f6a63e0" />

## Lab - Inspecting a container
```
docker container inspect ubuntu3-jegan
docker inspect ubuntu3-jegan
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/92c4f1a4-1850-412f-ab97-4d34baa985a5" />

## Lab - Finding IP Address of a running container
```
docker ps
docker inspect ubuntu3-jegan | grep IPA
docker inspect -f {{.NetworkSettings.Networks.brdige.IPAddress}} ubuntu3-jegan
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/f9346282-7519-4160-bfb8-c16e2e01ea5f" />


## Lab - Deleting running containers
Deleting a container gracefully
```
docker ps
docker stop ubuntu1
docker rm ubuntu1
```

Deleting a container forcibly
```
docker ps
docker rm -f ubuntu2
docker ps
```

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/7c1f2c37-4f5f-48f8-b040-3aa4e8f0fefd" />

## Lab - Deleting multiple containers that has a specific pattern in their names
```
docker ps

# List all container IDs that has nitesh in the name
docker ps -q -f "name=nitesh"

# List all container IDs that has sriram in the name
docker ps -q -f "name=sriram"

# List all container IDs that has jegan in the name
docker ps -q -f "name=jegan"

# Delete all containers that has jegan in the container name
docker rm -f $(docker ps -q -f "name=jegan")
```

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/6ffbe2d3-689c-4548-8b4e-7ef61381a142" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/596ff720-2a09-44e3-959b-2297866982f5" />


## Lab - Creating a Custom Docker Image

Note
<pre>
- In order to create a custom docker image, we must create a Dockerfile with instructions in it to customize the image
- Dockerfile is the standard filename, docker by default attempts to locate a file with name Dockerfile, and it is case-sensitive
- In case, you have name the file with a different name, then we must tell Docker which file has the custom image build instructions
</pre>

By default, ubuntu:26.04 will not have vim editor, it will not support ifconfig command to find IP, it will not support ping command
```
docker run -dit --name ubuntu1-jegan --hostname ubuntu1-jegan ubuntu:26.04 /bin/bash
docker exec -it ubuntu1-jegan /bin/bash

vim
ifconfig
ping 8.8.8.8
exit
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/785e7ced-48fd-4660-8f6c-5634982b59ee" />

Now, let's create a Dockerfile to install the missing tools ( vim editor, ifconfig and ping utilities )

```
# Create an empty folder CustomDockerImage in your home directory
cd ~
mkdir -p CustomDockerImage
cd CustomDockerImage
```

Create the Dockerfile with the below content, this will add just one image layer on top of the layers available in ubuntu:26.04
<pre>
FROM ubuntu:26.04

RUN apt update && apt install -y vim net-tools iputils-ping # In one image layer we are installing all the tools
</pre>

Create the Dockerfile with the below content, the below image will add 3 additional layers on top of the layers available in ubuntu:26.04
<pre>
FROM ubuntu:26.04

RUN apt update && apt install -y vim # We are adding 1 additional layer per RUN command
RUN apt update && apt install -y net-tools  # We are adding 1 additional layer per RUN command
RUN apt update && apt install -y iputils-ping # We are adding 1 additional layer per RUN command
</pre>


You can create any one of the Dockerfile shown above, I'll go with second one
```
cd ~/CustomDockerImage
cat Dockerfile

docker build -t myubuntu:1.0 .
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/1b086283-5387-4c52-8dc4-f33de82b96e1" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/516f4561-e01d-432d-a2a5-7c025ecde54c" />

Inspecting image layers
```
docker image inspect ubuntu:26.04
docker image inspect myubuntu:1.0
```
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/7fc46c38-fad0-402e-a9db-9d8d257f5f25" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/071af632-b050-49cd-bf09-a0005d936340" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/3bd6e91f-4d90-4caf-85bf-9d76a7d49069" />
<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/f06ba0c9-33f9-4fec-8ad2-1b824d462d2d" />
