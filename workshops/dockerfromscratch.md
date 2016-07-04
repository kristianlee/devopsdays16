#Docker from Scratch

I naively thought that this workshop would be about 'using' Docker from scratch, it actually turned out to be something a little different. 

It aimed to rebuild Docker from scratch, using the same methods the Docker devs had. There were a series of interactive workshops, whereby you'd find a way of implementing each stage of Docker's containerisation. 

The slides for the presentation are [here](https://github.com/Fewbytes/rubber-docker/tree/master/slides), and the main repo for the exercises is [here](https://github.com/Fewbytes/rubber-docker/tree/master/levels)

I'll attempt to go through the main points of the presentation / workstation here:

###How do containers work in terms of the Linux kernel?

- Namespaces (give a group of processes a view of the system that is distinct and different from others running on the system), 6 namespaces: 'network', 'pid', 'user'
- Cgroups (new, and going through a rewrite now: cgroups v1 will be in the workshop, v2 in the works) **Resource control mechanism** i.e.: "I want this group of processes to use 20% of CPU, 40% of RAM. They're not only about resource control now, they can also act as a device whitelist controller
- CoW filesystem - copy on write filesystem: use same base layer of files, only have the deltas written somewhere. Overlayfs (other one devicemapper)

###There's no such thing as containers in the Linux Kernel (!)
Containers are not a kernel primitive (i.e. there's no such thing in the kernel). 
It's a combination of various kernel mechanisms.

- Upside = varying levels of containment, different  types of containers (some that share certain resources, but don't others). 
- Downside = containers in Linux will never be as robust as on other systems. Secure, performance guaranteed (no Ddos etc). Docker is unlikely to ever get there. 

###Docker Mechanisms

**Dockercli**- talks over http API with docker daemon. 
**Docker daemon** - spawns containers (processes), talks to libcontainer/lxc and in turn that talks to the kernel. 

###Linux Mechanisms
**Fork** - takes a process, duplicates the process, and creates it as a child process of the original. When you do so, the memory pages is shared
**Exec** - takes a binary image (say grep) loads it, replaces the memory of the process, with the memory of the binary image. 
**Chroot** = every process in linux can have its own root filesystem node (actually a link to somewhere in the tree). Not very secure, easy to break out of. Was designed for builds, not security. 

2 problems with chroot - easy to breakout of, and you have to unpack the image each time. 

SO: 
**Pivot_root**
Changes the root filesystem for the entire process tree in the same mount namespace. Solves main isue with chroot
Takes 2 arguments, new root, and where to mount the old root. 

###Namespaces API:
-clone - same as unshare, but it forks off a new process
-unshare - creates a new namespace. Receives a flag which indicates which namespace you want to transition to, and creates a new namespace of that type (can do multiple at once) but in existing process.  
-setns - transitions an existing namespace to an existing process. 

Mount = shared or private 
We would normally want private for containers. 
Default bind mount option is shared (different from man documentation). 
For privatte
Mount --make-private t

###CoW storage
Start with a common read branch, writes go to a different branch.
Several flavours: layered filesystems, filesystem snapshots (btrfs), block level snapshots (LVM/DM)

Overlayfs can fill up the host's FS from the container. AUFS cannot. Overlayfs is the default because it's faster. 
By default Docker uses devicemapper with 20gb. 


###Cgroups
Cgroup - controls a group of processes, any forks etc will still be contained.

Nice to limit cpu
Ulimit to limit memory - problem is it only contains 1 process, children etc are unlimited. 
 
Blkio, controls I/O

Shares != a limit, only throttles back when other thing need the CPU. 

Memory controller
Limit in bytes = physical memory
Kmem = kernel memory
Sw = swap memory
