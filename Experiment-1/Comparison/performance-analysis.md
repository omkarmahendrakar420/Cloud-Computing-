# Performance Analysis: Type-1 (Proxmox VE) vs Type-2 (VMware Workstation)

## 1. Experimental Configuration
The comparative performance evaluation was conducted using uniform virtual machine resource allocations across both hypervisor platforms to isolate the overhead introduced by the hypervisor architecture:

- **Guest Operating System:** Ubuntu 24.04 LTS (64-bit)
- **Virtual CPU Allocation:** 2 vCPU (1 socket, 2 cores)
- **Memory Allocation:** 2 GB (2048 MiB)
- **Virtual Disk Allocation:** 20 GB
- **Benchmarking Suite:** Sysbench CPU Benchmark
- **Workload Parameter:** `--cpu-max-prime=20000`
- **Execution Command:**
  ```bash
  sysbench cpu --cpu-max-prime=20000 run
  ```

---

## 2. Proxmox VE (Type-1 Bare-Metal) Results

| Metric | Measured Value |
| :--- | :--- |
| **Hypervisor Architecture** | Type-1 (Bare-Metal / KVM Kernel) |
| **Total Execution Time** | **10.0005 s** |
| **Total Events Processed** | **17,494** |
| **Events per Second (Throughput)** | **1,749.16** |
| **Minimum Latency** | **0.57 ms** |
| **Average Latency** | **0.57 ms** |
| **Maximum Latency** | **2.43 ms** |
| **95th Percentile Latency** | **0.58 ms** |

---

## 3. VMware Workstation (Type-2 Hosted) Results

| Metric | Measured Value |
| :--- | :--- |
| **Hypervisor Architecture** | Type-2 (Hosted on Windows 11) |
| **Total Execution Time** | **10.0006 s** |
| **Total Events Processed** | **7,077** |
| **Events per Second (Throughput)** | **707.43** |
| **Minimum Latency** | **1.17 ms** |
| **Average Latency** | **1.41 ms** |
| **Maximum Latency** | **3.90 ms** |
| **95th Percentile Latency** | **1.37 ms** |

---

## 4. Hypervisor Performance Comparison

| Performance Metric | Type-1: Proxmox VE | Type-2: VMware Workstation | Performance Variance / Advantage |
| :--- | :---: | :---: | :---: |
| **Total Execution Time** | 10.0005 s | 10.0006 s | Fixed ~10s duration |
| **Total Events Processed** | **17,494** | **7,077** | **+147.2% More Events** on Proxmox VE |
| **Throughput (Events/sec)** | **1,749.16** | **707.43** | **2.47× Higher Throughput** on Proxmox VE |
| **Average Latency** | **0.57 ms** | **1.41 ms** | **59.6% Reduction in Latency** on Proxmox VE |
| **Minimum Latency** | **0.57 ms** | **1.17 ms** | **51.3% Lower Min Latency** on Proxmox VE |
| **Maximum Latency** | **2.43 ms** | **3.90 ms** | **37.7% Lower Max Latency** on Proxmox VE |
| **95th Percentile Latency** | **0.58 ms** | **1.37 ms** | **57.7% Lower Tail Latency** on Proxmox VE |

---

## 5. Technical Observations

1. **Throughput Advantage:**
   - Proxmox VE completed **17,494 events** at **1,749.16 events/sec**, whereas VMware Workstation completed **7,077 events** at **707.43 events/sec**.
   - Proxmox VE demonstrated a **+147.2% throughput increase** over VMware Workstation.

2. **Latency Analysis:**
   - The average execution latency on Proxmox VE was **0.57 ms** compared to **1.41 ms** on VMware Workstation (a **59.6% reduction** in response delay).
   - Tail latency (95th percentile) remained strictly low on Proxmox VE at **0.58 ms**, compared to **1.37 ms** on VMware Workstation, proving much tighter latency bounds.

3. **Virtualization Layer Overhead:**
   - In Type-1 bare-metal virtualization (Proxmox VE), guest CPU instructions execute directly through hardware virtualization extensions (Intel VT-x / AMD-V) managed by the KVM kernel module without intermediate user-space OS interference.
   - In Type-2 hosted virtualization (VMware Workstation), virtual machine execution requires continuous translation across the Windows host operating system scheduler and kernel, introducing substantial context-switching latency and CPU scheduling contention.

---

## 6. Conclusion
The experimental results conclusively validate that the **Type-1 bare-metal hypervisor (Proxmox VE)** delivers superior CPU compute throughput and significantly lower execution latency compared to the **Type-2 hosted hypervisor (VMware Workstation)** under identical hardware resource constraints (2 vCPU, 2 GB RAM, 20 GB Disk). For production enterprise cloud environments and compute-intensive workloads, Type-1 bare-metal virtualization provides near-native execution efficiency with minimal architectural virtualization overhead.
