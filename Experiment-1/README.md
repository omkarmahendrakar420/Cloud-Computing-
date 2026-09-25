# Performance Analysis of Type-1 and Type-2 Hypervisors

## 1. Title
**Performance Analysis of Type-1 and Type-2 Hypervisors: Proxmox VE (Type-1) vs VMware Workstation (Type-2)**

---

## 2. Objective
The objective of this experiment is to create identically configured virtual machines on a Type-1 hypervisor (Proxmox VE) and a Type-2 hypervisor (VMware Workstation), and then measure and compare their CPU performance using the Sysbench benchmark suite under identical workload conditions.

---

## 3. Hypervisors Used

| Part | Hypervisor | Hypervisor Type | Architecture / Deployment |
| :--- | :--- | :--- | :--- |
| **Part A** | Proxmox VE | Type-1 (Bare-metal) | Runs directly on host hardware |
| **Part B** | VMware Workstation | Type-2 (Hosted) | Runs on top of a host operating system |

---

## 4. Common Virtual Machine Configuration

To ensure a fair and consistent performance comparison, both virtual machines are provisioned with identical hardware specifications:

| Parameter | Configuration |
| :--- | :--- |
| **Operating System** | Ubuntu 22.04 or later (64-bit) |
| **Processor Allocation** | 2 vCPU (1 Socket, 2 Cores) |
| **Memory Allocation** | 2 GB (2048 MB / MiB) |
| **Virtual Disk Allocation** | 20 GB |
| **Benchmark Suite** | Sysbench (CPU Benchmark) |

---

## 5. Benchmark Command

The CPU benchmark is executed inside the guest Ubuntu virtual machine on both hypervisors using:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

The benchmark measures processor efficiency by computing prime numbers up to 20,000. The key metrics recorded for analysis include:

- **Total execution time** (s)
- **Total events**
- **Events per second** (throughput)
- **Minimum latency** (ms)
- **Average latency** (ms)
- **Maximum latency** (ms)

---

## 6. Experiment Structure

The experiment is organized into the following modules:

- **[Part-A-Type-1-Proxmox](./Part-A-Type-1-Proxmox/README.md):** Step-by-step procedure for VM creation, Ubuntu installation, system verification, and CPU benchmarking on the Proxmox VE (Type-1) bare-metal hypervisor.
- **[Part-B-Type-2-VMware](./Part-B-Type-2-VMware/README.md):** Step-by-step procedure for VM creation, Ubuntu installation, system verification, and CPU benchmarking on the VMware Workstation (Type-2) hosted hypervisor.
- **[Comparison](./Comparison/README.md):** Consolidated performance comparison tables, graphical/tabular evaluation, and experimental analysis comparing Type-1 vs Type-2 hypervisors.
