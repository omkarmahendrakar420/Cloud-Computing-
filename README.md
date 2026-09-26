# Cloud Computing Laboratory

[![Course](https://img.shields.io/badge/Course-Cloud%20Computing%20Laboratory-blue.svg)](#)
[![Hypervisors](https://img.shields.io/badge/Hypervisors-Proxmox%20VE%20%7C%20VMware%20Workstation-orange.svg)](#)
[![Benchmark](https://img.shields.io/badge/Benchmark-Sysbench%20CPU%2020k%20Primes-green.svg)](#)
[![Status](https://img.shields.io/badge/Status-Completed-brightgreen.svg)](#)

---

## 📚 Experiments Index
- **[Experiment 1: Performance Analysis of Type-1 and Type-2 Hypervisors](./Experiment-1/README.md)**

---

# Experiment 1: Performance Analysis of Type-1 and Type-2 Hypervisors

## Table of Contents
1. [Experiment Overview & Objective](#1-experiment-overview--objective)
2. [Hypervisor Architecture & Classification](#2-hypervisor-architecture--classification)
3. [Standardized Virtual Machine Specifications](#3-standardized-virtual-machine-specifications)
4. [Part A: Performance Analysis Using Type-1 Hypervisor (Proxmox VE)](#part-a-performance-analysis-using-type-1-hypervisor--proxmox-ve)
5. [Part B: Performance Analysis Using Type-2 Hypervisor (VMware Workstation)](#part-b-performance-analysis-using-type-2-hypervisor--vmware-workstation)
6. [Hypervisor Performance Comparison](#hypervisor-performance-comparison)
7. [Technical Observations & Discussion](#technical-observations--discussion)
8. [Conclusion](#conclusion)

---

## 1. Experiment Overview & Objective

### Objective
The primary objective of this experiment is to deploy identically configured virtual machines on a **Type-1 Bare-Metal Hypervisor (Proxmox VE)** and a **Type-2 Hosted Hypervisor (VMware Workstation)**, and evaluate their computational CPU performance using the `sysbench` benchmark suite under identical workload conditions.

### Benchmark Command
```bash
sysbench cpu --cpu-max-prime=20000 run
```

---

## 2. Hypervisor Architecture & Classification

| Hypervisor | Architecture Type | Host Platform | Execution Model |
| :--- | :--- | :--- | :--- |
| **Proxmox VE** | **Type-1 (Bare-Metal)** | Dedicated Server Hardware | Direct execution on CPU VT-x/AMD-V via KVM kernel module |
| **VMware Workstation** | **Type-2 (Hosted)** | Windows 11 Host OS | Executed through host OS kernel scheduler & VMM translation |

### Architectural Flowchart

```mermaid
graph TD
    subgraph Type1["Type-1 Bare-Metal Architecture (Proxmox VE)"]
        H1["Physical Server Hardware (CPU, RAM, Disk)"] --> P1["Proxmox VE Hypervisor (KVM Kernel)"]
        P1 --> VM1["Ubuntu 24.04 VM (b1-t1)"]
        VM1 --> S1["Sysbench CPU Benchmark: 1,749.16 Events/sec"]
    end

    subgraph Type2["Type-2 Hosted Architecture (VMware Workstation)"]
        H2["Physical Host Hardware (AMD Ryzen 5 5600H)"] --> OS2["Host OS (Windows 11)"]
        OS2 --> VMW["VMware Workstation Pro"]
        VMW --> VM2["Ubuntu 24.04 VM"]
        VM2 --> S2["Sysbench CPU Benchmark: 707.43 Events/sec"]
    end
```

---

## 3. Standardized Virtual Machine Specifications

To ensure direct comparability, both virtual machines were provisioned with uniform resource parameters:

| Resource Parameter | Proxmox VE (Type-1) | VMware Workstation (Type-2) |
| :--- | :--- | :--- |
| **Virtual Machine Name** | `TYPE1` / `b1-t1` | `Ubuntu 64-bit` |
| **Guest Operating System** | Ubuntu 24.04 LTS (64-bit) | Ubuntu 24.04 LTS (64-bit) |
| **Virtual CPU (vCPU)** | 2 vCPU (1 socket, 2 cores) | 2 vCPU (1 processor, 2 cores) |
| **Memory Allocation** | 2048 MiB (2 GB RAM) | 2048 MB (2 GB RAM) |
| **Virtual Disk Allocation** | 20 GB Virtual Disk | 20 GB Virtual Disk |
| **Network Mode** | VirtIO Bridge (`vmbr0`) | NAT (`vmnet8`) |
| **Benchmark Tool** | `sysbench` 1.0.20 | `sysbench` 1.0.20 |

---

# PART A: Performance Analysis Using Type-1 Hypervisor – Proxmox VE

## 1. Accessing the Proxmox VE Web Interface
Proxmox VE is deployed directly on physical bare-metal server hardware and accessed remotely through its web-based management interface over HTTPS.

- **URL:** `https://10.11.0.252:8006`
- **Port:** `8006`

![Step 1: Accessing Proxmox Web Interface](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/01-proxmox-access-url.png)
*Step 1: Navigating to the Proxmox VE Web Interface in Google Chrome.*

---

## 2. Logging in to Proxmox VE
Authenticate into the Proxmox server node using assigned credentials:
- **Username:** `root`
- **Realm:** `Linux PAM standard authentication`

![Step 2: Proxmox VE Login Page](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/02-proxmox-login.png)
*Step 2: Proxmox VE authentication dialog.*

---

## 3. Understanding the Proxmox VE Interface & Dashboard
The Proxmox dashboard displays the centralized Datacenter hierarchy, server node (`admin1-HP-Pro-Tower-280-G9-E-PCI-Desktop-PC`), storage pools, networks, and active virtual machines.

![Step 3: Proxmox VE Dashboard](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/03-proxmox-dashboard.png)
*Step 3: Proxmox VE Datacenter inventory overview.*

---

## 4. Creating the Virtual Machine in Proxmox VE

### Step 4.1: General Configuration
Configure the node, unique virtual machine ID, and VM name:
- **Node:** `admin1-HP-Pro-Tower-280-G9-E-PCI-Desktop-PC`
- **VM ID:** `101` / `117`
- **Name:** `TYPE1` / `b1-t1`

![Step 4.1: Create VM - General](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/04-proxmox-create-vm-general.png)
*Step 4.1: Configuring VM General settings.*

### Step 4.2: Operating System (OS) Configuration
- **Installation Media:** CD/DVD Disc image file (iso)
- **Storage:** `local`
- **ISO Image:** `ubuntu-22.04.5-desktop-amd64.iso` / `ubuntu-24.04`
- **Guest OS Type:** Linux (`6.x - 2.6 Kernel`)

![Step 4.2: Create VM - OS](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/05-proxmox-create-vm-os.png)
*Step 4.2: Selecting the Ubuntu ISO image.*

### Step 4.3: System Configuration
- **Graphics Card:** Default
- **Machine:** Default (`i440fx`)
- **Firmware / BIOS:** Default (`SeaBIOS`)
- **SCSI Controller:** `VirtIO SCSI single`

![Step 4.3: Create VM - System](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/06-proxmox-create-vm-system.png)
*Step 4.3: Configuring System hardware platform.*

### Step 4.4: Virtual Disk Configuration
- **Bus/Device:** `SCSI` (Device `0`)
- **Storage:** `local-lvm`
- **Disk Size:** `20 GiB`
- **Cache:** Default (No cache)
- **IO Thread:** Enabled

![Step 4.4: Create VM - Disks](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/07-proxmox-create-vm-disks.png)
*Step 4.4: Allocating 20 GB SCSI virtual disk.*

### Step 4.5: CPU Processor Configuration
- **Sockets:** `1`
- **Cores:** `2` (Total vCPU = `2`)
- **Type:** `x86-64-v2-AES`

![Step 4.5: Create VM - CPU](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/08-proxmox-create-vm-cpu.png)
*Step 4.5: Allocating 2 virtual CPU cores.*

### Step 4.6: Memory Resource Configuration
- **Memory:** `2048 MiB` (2 GB RAM)

![Step 4.6: Create VM - Memory](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/09-proxmox-create-vm-memory.png)
*Step 4.6: Allocating 2048 MiB (2 GB) RAM.*

### Step 4.7: Network Interface Configuration
- **Bridge:** `vmbr0`
- **Model:** `VirtIO (paravirtualized)`
- **Firewall:** Enabled

![Step 4.7: Create VM - Network](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/10-proxmox-create-vm-network.png)
*Step 4.7: Configuring VirtIO network bridge.*

### Step 4.8: Confirming Virtual Machine Configuration
Review all configured hardware parameters before final provisioning:

![Step 4.8: Create VM - Confirm](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/11-proxmox-create-vm-confirm.png)
*Step 4.8: Final VM configuration confirmation summary.*

---

## 5. Starting and Verifying the Virtual Machine
Power on the virtual machine and verify that its status changes to **Running**:

![Step 5: Proxmox VM Running](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/12-proxmox-vm-running.png)
*Step 5: Virtual Machine running actively in Proxmox Datacenter.*

---

## 6. Accessing Ubuntu Console via noVNC & System Verification
Access the guest OS console via noVNC to verify system hostname and operating system information:

```bash
hostnamectl
```

![Step 6: Ubuntu Console inside Proxmox](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/13-proxmox-ubuntu-console.png)
*Step 6: Output of `hostnamectl` in Ubuntu guest console inside Proxmox.*

---

## 7. Verifying CPU and Memory Hardware Allocation
Inspect CPU and memory allocation inside the Ubuntu terminal:

```bash
# Verify CPU architecture and 2 vCPU core allocation
lscpu

# Verify RAM memory allocation (2.0 GB)
free -h
```

![Step 7: System Configuration Verification](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/14-proxmox-system-configuration.png)
*Step 7: Verifying allocated 2 vCPU and 2.0 GiB memory via `lscpu` and `free -h`.*

---

## 8. Installing Sysbench & Running CPU Benchmark
Update repository index, install Sysbench, and execute the CPU benchmark:

```bash
sudo apt update
sudo apt install sysbench -y
sysbench --version

# Run Sysbench CPU benchmark
sysbench cpu --cpu-max-prime=20000 run
```

![Step 8: Sysbench Benchmark Result](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/15-proxmox-sysbench-result.png)
*Step 8: Benchmark output of `sysbench cpu --cpu-max-prime=20000 run` on Proxmox VE.*

### Measured Sysbench Performance Data (Proxmox VE):
- **Total Execution Time:** `10.0005 s`
- **Total Number of Events:** `17,494`
- **Events per Second (Throughput):** `1,749.16`
- **Minimum Latency:** `0.57 ms`
- **Average Latency:** `0.57 ms`
- **Maximum Latency:** `2.43 ms`
- **95th Percentile Latency:** `0.58 ms`

---

## 9. Resource Monitoring in Proxmox VE

### 9.1 Host Node Resource Summary
![Step 9.1: Host Node Resource Summary](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/16-proxmox-resource-monitoring-node.png)
*Step 9.1: Proxmox bare-metal host node resource overview.*

### 9.2 Virtual Machine Resource Summary
![Step 9.2: VM Resource Summary](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/17-proxmox-resource-monitoring-vm.png)
*Step 9.2: Proxmox VM summary dashboard showing 0.75% CPU load, 1.76 GB RAM usage, and 20 GB storage.*

### 9.3 CPU Utilization Graph
![Step 9.3: CPU Utilization Graph](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/18-proxmox-resource-monitoring-cpu.png)
*Step 9.3: Real-time CPU utilization spike corresponding to the active Sysbench workload execution.*

### 9.4 Memory Usage Graph
![Step 9.4: Memory Usage Graph](./Experiment-1/Part-A-Type-1-Proxmox/screenshots/19-proxmox-resource-monitoring-ram.png)
*Step 9.4: Stable memory utilization curve throughout the benchmarking lifecycle.*

---

## 10. Observation Table – Type-1 Hypervisor (Proxmox VE)

| Parameter | Experimental Value |
| :--- | :--- |
| **Hypervisor** | Proxmox VE (KVM) |
| **Hypervisor Architecture** | Type-1 (Bare-Metal) |
| **Guest Operating System** | Ubuntu 24.04 LTS |
| **CPU Allocation** | 2 vCPU (x86-64-v2-AES) |
| **Memory Allocation** | 2 GB (2048 MiB) |
| **Disk Allocation** | 20 GB |
| **Total Execution Time** | **10.0005 s** |
| **Total Events Processed** | **17,494** |
| **Events per Second (Throughput)** | **1,749.16** |
| **Minimum Latency** | **0.57 ms** |
| **Average Latency** | **0.57 ms** |
| **Maximum Latency** | **2.43 ms** |
| **95th Percentile Latency** | **0.58 ms** |

---

# PART B: Performance Analysis Using Type-2 Hypervisor – VMware Workstation

## 1. Launching VMware Workstation
VMware Workstation is a Type-2 (hosted) hypervisor running on top of Windows 11.

![Step 1: Launching VMware Workstation](./Experiment-1/Part-B-Type-2-VMware/screenshots/01-vmware-launch.png)
*Step 1: VMware Workstation Pro home screen.*

---

## 2. Virtual Machine Creation Wizard

### Step 2.1: Selecting Configuration Type
Select **Typical (recommended)** configuration:

![Step 2.1: Selecting Typical Configuration](./Experiment-1/Part-B-Type-2-VMware/screenshots/02-vmware-wizard-typical.png)
*Step 2.1: Selecting Typical VM creation wizard.*

### Step 2.2: Selecting Installation Media (ISO)
Browse and select the Ubuntu 64-bit ISO image:

![Step 2.2: Selecting Ubuntu ISO](./Experiment-1/Part-B-Type-2-VMware/screenshots/03-vmware-select-iso.png)
*Step 2.2: Selecting Ubuntu installer ISO image.*

### Step 2.3: Easy Install Information
Configure user credentials and full name:

![Step 2.3: Easy Install Information](./Experiment-1/Part-B-Type-2-VMware/screenshots/04-vmware-easy-install-info.png)
*Step 2.3: Configuring user credentials.*

### Step 2.4: Naming the Virtual Machine
Set VM name as `Ubuntu 64-bit` and specify directory path:

![Step 2.4: Naming the Virtual Machine](./Experiment-1/Part-B-Type-2-VMware/screenshots/05-vmware-name-vm.png)
*Step 2.4: Specifying the VM name and storage location.*

### Step 2.5: Specifying Virtual Disk Capacity
Set disk size to **20.0 GB**:

![Step 2.5: Specifying Disk Capacity](./Experiment-1/Part-B-Type-2-VMware/screenshots/06-vmware-specify-disk.png)
*Step 2.5: Configuring 20 GB virtual disk capacity.*

### Step 2.6: Hardware Configuration Summary
Review the configured parameters (20 GB Disk, 2 CPU Cores, NAT Adapter):

![Step 2.6: Hardware Configuration Summary](./Experiment-1/Part-B-Type-2-VMware/screenshots/07-vmware-vm-configuration.png)
*Step 2.6: Ready to create VM configuration summary.*

---

## 3. Powering On and Installing Ubuntu

### Step 3.1: Powering On the Virtual Machine
![Step 3.1: Powering On VM](./Experiment-1/Part-B-Type-2-VMware/screenshots/08-vmware-booting.png)
*Step 3.1: Initial boot screen in VMware Workstation.*

### Step 3.2: Complete Ubuntu Installation Workflow

| Step | Installation Action | Screenshot Evidence |
| :---: | :--- | :---: |
| **Welcome** | Language selection | ![Welcome](./Experiment-1/Part-B-Type-2-VMware/screenshots/09-vmware-ubuntu-welcome.png) |
| **Accessibility** | Accessibility settings | ![Accessibility](./Experiment-1/Part-B-Type-2-VMware/screenshots/10-vmware-ubuntu-accessibility.png) |
| **Keyboard** | Keyboard layout setup | ![Keyboard](./Experiment-1/Part-B-Type-2-VMware/screenshots/11-vmware-ubuntu-keyboard.png) |
| **Install Type** | Interactive installation option | ![Install Type](./Experiment-1/Part-B-Type-2-VMware/screenshots/12-vmware-ubuntu-install-type.png) |
| **Applications** | Default app bundle selection | ![Apps](./Experiment-1/Part-B-Type-2-VMware/screenshots/13-vmware-ubuntu-apps-selection.png) |
| **Drivers** | Proprietary codecs & drivers | ![Drivers](./Experiment-1/Part-B-Type-2-VMware/screenshots/14-vmware-ubuntu-software-drivers.png) |
| **Disk Setup** | Virtual disk partitioning (`sda`) | ![Disk Setup](./Experiment-1/Part-B-Type-2-VMware/screenshots/15-vmware-ubuntu-disk-partitioning.png) |
| **User Account** | Setting username & password | ![User Account](./Experiment-1/Part-B-Type-2-VMware/screenshots/16-vmware-ubuntu-user-account.png) |
| **Timezone** | Timezone configuration | ![Timezone](./Experiment-1/Part-B-Type-2-VMware/screenshots/17-vmware-ubuntu-timezone.png) |
| **Review** | Reviewing disk & OS choices | ![Review](./Experiment-1/Part-B-Type-2-VMware/screenshots/18-vmware-ubuntu-ready-to-install.png) |
| **Installing** | Base system package setup | ![Installing](./Experiment-1/Part-B-Type-2-VMware/screenshots/19-vmware-ubuntu-installing-system.png) |
| **Copying** | Copying files to virtual storage | ![Copying](./Experiment-1/Part-B-Type-2-VMware/screenshots/20-vmware-ubuntu-copying-files.png) |
| **Complete** | Installation complete prompt | ![Complete](./Experiment-1/Part-B-Type-2-VMware/screenshots/21-vmware-ubuntu-install-complete.png) |
| **Summary** | Partition and configuration review | ![Summary](./Experiment-1/Part-B-Type-2-VMware/screenshots/22-vmware-ubuntu-review-choices.png) |
| **Slides** | Ubuntu features overview slide | ![Slides](./Experiment-1/Part-B-Type-2-VMware/screenshots/23-vmware-ubuntu-install-slides.png) |

---

## 4. System Configuration & Resource Verification

### 4.1 System Hostname & OS Details
```bash
hostnamectl
```
![Step 4.1: Hostnamectl Output](./Experiment-1/Part-B-Type-2-VMware/screenshots/24-vmware-ubuntu-running-hostnamectl.png)
*Step 4.1: `hostnamectl` showing Ubuntu 24.04 LTS on VMware Virtual Platform.*

### 4.2 CPU Architecture & Core Allocation
```bash
lscpu
```
![Step 4.2: lscpu Output](./Experiment-1/Part-B-Type-2-VMware/screenshots/25-vmware-lscpu.png)
*Step 4.2: `lscpu` verifying 2 vCPU on AMD Ryzen 5 5600H processor.*

### 4.3 Memory (RAM) Allocation
```bash
free -h
```
![Step 4.3: free -h Output](./Experiment-1/Part-B-Type-2-VMware/screenshots/26-vmware-free-memory.png)
*Step 4.3: `free -h` verifying system memory allocation.*

### 4.4 Virtual Disk Space
```bash
df -h
```
![Step 4.4: df -h Output](./Experiment-1/Part-B-Type-2-VMware/screenshots/27-vmware-disk-df.png)
*Step 4.4: `df -h` inspecting virtual disk partitions.*

### 4.5 Live System Process Monitoring
```bash
top
```
![Step 4.5: top Output](./Experiment-1/Part-B-Type-2-VMware/screenshots/28-vmware-top-monitoring.png)
*Step 4.5: `top` displaying live CPU task execution.*

---

## 5. Installing Sysbench & Running CPU Benchmark

### 5.1 Updating Package Index
```bash
sudo apt update
```
![Step 5.1: Package Update](./Experiment-1/Part-B-Type-2-VMware/screenshots/29-vmware-apt-update.png)
*Step 5.1: Updating package repository indexes.*

### 5.2 Installing Sysbench
```bash
sudo apt install sysbench -y
```
![Step 5.2: Install Sysbench](./Experiment-1/Part-B-Type-2-VMware/screenshots/30-vmware-apt-install-sysbench.png)
*Step 5.2: Installing Sysbench benchmarking package.*

### 5.3 Verifying Sysbench Version
```bash
sysbench --version
```
![Step 5.3: Verify Version](./Experiment-1/Part-B-Type-2-VMware/screenshots/31-vmware-sysbench-version.png)
*Step 5.3: Sysbench version confirmation (`sysbench 1.0.20`).*

### 5.4 Executing CPU Performance Benchmark
```bash
sysbench cpu --cpu-max-prime=20000 run
```
![Step 5.4: Sysbench Benchmark Result](./Experiment-1/Part-B-Type-2-VMware/screenshots/32-vmware-sysbench-result.png)
*Step 5.4: Terminal output of `sysbench cpu --cpu-max-prime=20000 run` on VMware Workstation.*

### Measured Sysbench Performance Data (VMware Workstation):
- **Total Execution Time:** `10.0006 s` (or `10.0012 s` in terminal run)
- **Total Number of Events:** `7,077` (or `8,012` in terminal run)
- **Events per Second (Throughput):** `707.43` (or `800.97` in terminal run)
- **Minimum Latency:** `1.17 ms`
- **Average Latency:** `1.41 ms` (or `1.25 ms` in terminal run)
- **Maximum Latency:** `3.90 ms`
- **95th Percentile Latency:** `1.37 ms`

---

## 6. VMware Virtual Machine Settings Inspection
Access **VM -> Settings** to verify configured virtual hardware devices:

![Step 6: VMware Settings](./Experiment-1/Part-B-Type-2-VMware/screenshots/33-vmware-virtual-machine-settings.png)
*Step 6: VMware Workstation Virtual Machine Settings dialog.*

---

## 7. Observation Table – Type-2 Hypervisor (VMware Workstation)

| Parameter | Experimental Value |
| :--- | :--- |
| **Hypervisor** | VMware Workstation Pro |
| **Hypervisor Architecture** | Type-2 (Hosted) |
| **Host Operating System** | Windows 11 (64-bit) |
| **Guest Operating System** | Ubuntu 24.04 LTS |
| **CPU Allocation** | 2 vCPU (AMD Ryzen 5 5600H) |
| **Memory Allocation** | 2 GB / 4 GB |
| **Disk Allocation** | 20 GB Virtual Disk |
| **Total Execution Time** | **10.0006 s** |
| **Total Events Processed** | **7,077** |
| **Events per Second (Throughput)** | **707.43** |
| **Minimum Latency** | **1.17 ms** |
| **Average Latency** | **1.41 ms** |
| **Maximum Latency** | **3.90 ms** |
| **95th Percentile Latency** | **1.37 ms** |

---

# Hypervisor Performance Comparison

## Side-by-Side Performance Comparison Table

| Performance Metric | Type-1: Proxmox VE | Type-2: VMware Workstation | Performance Variance / Advantage |
| :--- | :---: | :---: | :---: |
| **Hypervisor Architecture** | **Type-1 (Bare-Metal)** | **Type-2 (Hosted)** | Direct hardware vs OS layer |
| **Guest Operating System** | **Ubuntu 24.04 LTS** | **Ubuntu 24.04 LTS** | Standardized |
| **Allocated vCPU** | **2 vCPU** | **2 vCPU** | Standardized |
| **Allocated RAM** | **2 GB** | **2 GB** | Standardized |
| **Allocated Disk** | **20 GB** | **20 GB** | Standardized |
| **Total Execution Time** | **10.0005 s** | **10.0006 s** | Fixed ~10s duration |
| **Total Events Processed** | **17,494** | **7,077** | **+147.2% More Events** (Proxmox VE) |
| **Throughput (Events/sec)** | **1,749.16** | **707.43** | **2.47× Higher Throughput** (Proxmox VE) |
| **Average Latency** | **0.57 ms** | **1.41 ms** | **59.6% Lower Latency** (Proxmox VE) |
| **Minimum Latency** | **0.57 ms** | **1.17 ms** | **51.3% Lower Min Latency** (Proxmox VE) |
| **Maximum Latency** | **2.43 ms** | **3.90 ms** | **37.7% Lower Max Latency** (Proxmox VE) |
| **95th Percentile Latency** | **0.58 ms** | **1.37 ms** | **57.7% Lower Tail Latency** (Proxmox VE) |

---

## Official Laboratory Comparison Evidence

![Official Hypervisor Performance Comparison](./Experiment-1/Comparison/screenshots/01-hypervisor-performance-comparison.png)
*Figure: Empirical comparison table captured from the completed benchmark analysis.*

---

## Technical Observations & Discussion

1. **Computational Throughput:**
   - Proxmox VE achieved **1,749.16 events per second**, processing **17,494 events** in 10 seconds.
   - VMware Workstation achieved **707.43 events per second**, processing **7,077 events** in 10 seconds.
   - Proxmox VE delivered a **+147.2% increase in computational throughput (2.47× faster)** under identical 2 vCPU allocation.

2. **Latency & Response Bounds:**
   - Proxmox VE recorded an average latency of **0.57 ms** with a 95th percentile latency of **0.58 ms**.
   - VMware Workstation recorded an average latency of **1.41 ms** with a 95th percentile latency of **1.37 ms**.
   - Proxmox VE achieved a **59.6% reduction in average latency** and tighter latency consistency.

3. **Virtualization Architecture Analysis:**
   - In **Type-1 bare-metal virtualization (Proxmox VE)**, CPU instructions from the guest VM execute directly through hardware virtualization extensions (Intel VT-x / AMD-V) managed by the KVM kernel module without intermediate user-space OS scheduling.
   - In **Type-2 hosted virtualization (VMware Workstation)**, VM CPU instructions must pass through the VMware VMM engine, translate into Windows system calls, and compete for time slices with Windows desktop processes, causing context-switching overhead and increased latency.

---

## Conclusion
The experimental results demonstrate that the **Type-1 bare-metal hypervisor (Proxmox VE)** significantly outperforms the **Type-2 hosted hypervisor (VMware Workstation)** in CPU throughput and execution latency under identical virtual machine resource configurations (2 vCPU, 2 GB RAM, 20 GB Disk). For production enterprise cloud environments and compute-intensive virtualization workloads, Type-1 bare-metal hypervisors provide near-native hardware execution efficiency with minimal architectural virtualization overhead.
