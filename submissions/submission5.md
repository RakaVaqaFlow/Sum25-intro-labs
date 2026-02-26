# Virtualization Lab

## Task 1: VM Deployment

**VirtualBox** was used to deploy the virtual machine.

**VirtualBox version:** 7.0.26 r168464

**Steps:**
1. Install VirtualBox from the [official website](https://www.virtualbox.org/) (But I had vbox already).
2. Create a new VM with Ubuntu24 and attach image
3. Configure: memory, CPU cores, network.

![screenshot](img/lab5-1.png)
![screenshot](img/lab5-2.png)
![screenshot](img/lab5-3.png)


## Task 2: System Information Tools

### 1. Processor, RAM, and Network Information

**Processor:** `lscpu`

![screenshot](img/lab5-4.png)

**RAM:** `free -h`

![screenshot](img/lab5-5.png)

**Network:** `ip a s`

![screenshot](img/lab5-6.png)

### 2. Operating System Specifications

**OS:** `cat /etc/os-release && uname -a`

![screenshot](img/lab5-7.png)
