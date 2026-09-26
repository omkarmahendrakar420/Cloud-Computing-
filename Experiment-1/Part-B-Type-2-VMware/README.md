# Part B: Performance Analysis Using Type-2 Hypervisor – VMware Workstation

## 1. Launching VMware Workstation
VMware Workstation is a Type-2 (hosted) hypervisor running as an application on top of the host operating system (Windows 11).

![Launching VMware Workstation](./screenshots/01-vmware-launch.png)
*Step 1: VMware Workstation Pro home screen.*

---

## 2. Virtual Machine Creation Wizard

### Step 2.1: Selecting Configuration Type
Select **Typical (recommended)** to configure the virtual machine in standard steps.

![Selecting Typical Configuration](./screenshots/02-vmware-wizard-typical.png)
*Step 2.1: Selecting Typical VM creation wizard.*

### Step 2.2: Selecting Installation Media (ISO)
Browse and select the Ubuntu 64-bit ISO image:

![Selecting Ubuntu ISO](./screenshots/03-vmware-select-iso.png)
*Step 2.2: Selecting Ubuntu 24.04 installer ISO image.*

### Step 2.3: Easy Install Information
Enter Linux personalization details (Full name, User name, Password):

![Easy Install Information](./screenshots/04-vmware-easy-install-info.png)
*Step 2.3: Configuring user credentials.*

### Step 2.4: Naming the Virtual Machine
Set the VM name as `Ubuntu 64-bit` and specify the disk storage path:

![Naming the Virtual Machine](./screenshots/05-vmware-name-vm.png)
*Step 2.4: Specifying the VM name and storage location.*

### Step 2.5: Specifying Virtual Disk Capacity
Allocate a **20.0 GB** maximum virtual disk size:

![Specifying Disk Capacity](./screenshots/06-vmware-specify-disk.png)
*Step 2.5: Configuring 20 GB virtual disk capacity.*

### Step 2.6: Hardware Configuration Summary
Review the configured parameters (20 GB Disk, 2 CPU Cores, NAT Adapter):

![Hardware Configuration Summary](./screenshots/07-vmware-vm-configuration.png)
*Step 2.6: Ready to create VM configuration summary.*

---

## 3. Powering On and Installing Ubuntu

### Step 3.1: Powering On the Virtual Machine
Power on the virtual machine from the VMware library:

![Powering On VM](./screenshots/08-vmware-booting.png)
*Step 3.1: VM initial boot screen in VMware Workstation.*

### Step 3.2: Ubuntu Installer Setup
Follow the Ubuntu OS installation wizard:

| Step | Description | Screenshot |
| :---: | :--- | :---: |
| **Welcome** | Language & Welcome selection | ![Welcome](./screenshots/09-vmware-ubuntu-welcome.png) |
| **Accessibility** | Accessibility preferences | ![Accessibility](./screenshots/10-vmware-ubuntu-accessibility.png) |
| **Keyboard** | Keyboard layout configuration | ![Keyboard](./screenshots/11-vmware-ubuntu-keyboard.png) |
| **Install Type** | Interactive installation option | ![Install Type](./screenshots/12-vmware-ubuntu-install-type.png) |
| **Applications** | Default application selection | ![Apps](./screenshots/13-vmware-ubuntu-apps-selection.png) |
| **Drivers** | Proprietary software & driver configuration | ![Drivers](./screenshots/14-vmware-ubuntu-software-drivers.png) |
| **Disk Setup** | Virtual disk partitioning (`sda`) | ![Disk Setup](./screenshots/15-vmware-ubuntu-disk-partitioning.png) |
| **User Account** | User account and hostname setup | ![User Account](./screenshots/16-vmware-ubuntu-user-account.png) |
| **Timezone** | Geographical timezone selection | ![Timezone](./screenshots/17-vmware-ubuntu-timezone.png) |
| **Review** | Reviewing configuration choices | ![Review](./screenshots/18-vmware-ubuntu-ready-to-install.png) |
| **Installing** | Base system package installation | ![Installing](./screenshots/19-vmware-ubuntu-installing-system.png) |
| **Copying** | Copying files to virtual disk | ![Copying](./screenshots/20-vmware-ubuntu-copying-files.png) |
| **Complete** | Installation completion confirmation | ![Complete](./screenshots/21-vmware-ubuntu-install-complete.png) |
| **Summary** | Partition and configuration review | ![Summary](./screenshots/22-vmware-ubuntu-review-choices.png) |
| **Slides** | Ubuntu features overview slide | ![Slides](./screenshots/23-vmware-ubuntu-install-slides.png) |

---

## 4. System Configuration & Resource Verification
After installation, open the Ubuntu terminal to verify allocated hardware resources:

### 4.1 System Hostname & OS Details
```bash
hostnamectl
```
![Hostnamectl Output](./screenshots/24-vmware-ubuntu-running-hostnamectl.png)
*Step 4.1: `hostnamectl` showing Ubuntu 24.04 LTS and VMware Virtual Platform.*

### 4.2 CPU Architecture & Core Allocation
```bash
lscpu
```
![lscpu Output](./screenshots/25-vmware-lscpu.png)
*Step 4.2: `lscpu` verifying 2 vCPU on AMD Ryzen 5 5600H processor.*

### 4.3 Memory (RAM) Allocation
```bash
free -h
```
![free -h Output](./screenshots/26-vmware-free-memory.png)
*Step 4.3: `free -h` verifying system memory allocation and swap space.*

### 4.4 Virtual Disk Space
```bash
df -h
```
![df -h Output](./screenshots/27-vmware-disk-df.png)
*Step 4.4: `df -h` inspecting virtual disk partitions and filesystem usage.*

### 4.5 Live System Process Monitoring
```bash
top
```
![top Output](./screenshots/28-vmware-top-monitoring.png)
*Step 4.5: `top` displaying real-time CPU task execution and load average.*

---

## 5. Installing Sysbench & Running CPU Benchmark

### 5.1 Updating Package Index
```bash
sudo apt update
```
![Package Update](./screenshots/29-vmware-apt-update.png)
*Step 5.1: Updating Ubuntu package repository indexes.*

### 5.2 Installing Sysbench
```bash
sudo apt install sysbench -y
```
![Install Sysbench](./screenshots/30-vmware-apt-install-sysbench.png)
*Step 5.2: Installing Sysbench benchmarking package.*

### 5.3 Verifying Sysbench Version
```bash
sysbench --version
```
![Verify Version](./screenshots/31-vmware-sysbench-version.png)
*Step 5.3: Sysbench version confirmation (`sysbench 1.0.20`).*

### 5.4 Executing CPU Performance Benchmark
Execute the CPU benchmark test with prime limit 20,000:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

![Sysbench Benchmark Result](./screenshots/32-vmware-sysbench-result.png)
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

![VMware Settings](./screenshots/33-vmware-virtual-machine-settings.png)
*Step 6: VMware Workstation Virtual Machine Settings showing Memory, Processors (2), Hard Disk, and NAT adapter.*

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
