# Performance Analysis: Type-1 (Proxmox VE) vs Type-2 (VMware Workstation)

## 1. Experimental Configuration
The performance evaluation was conducted using identical virtual machine resource allocation across both hypervisors to ensure an accurate and fair comparison:

- **Guest Operating System:** Ubuntu 22.04 or later (64-bit)
- **Virtual CPU Allocation:** 2 vCPU (1 socket, 2 cores)
- **Memory Allocation:** 2 GB (2048 MB)
- **Virtual Disk Allocation:** 20 GB
- **Benchmarking Suite:** Sysbench CPU Benchmark
- **Execution Parameter:** `--cpu-max-prime=20000`
- **Execution Command:**
  ```bash
  sysbench cpu --cpu-max-prime=20000 run
  ```

---

## 2. Proxmox VE (Type-1) Results

| Metric | Proxmox VE |
| :--- | :--- |
| **Total Execution Time (s)** |  |
| **Total Events** |  |
| **Events per Second (Throughput)** |  |
| **Minimum Latency (ms)** |  |
| **Average Latency (ms)** |  |
| **Maximum Latency (ms)** |  |

---

## 3. VMware Workstation (Type-2) Results

| Metric | VMware Workstation |
| :--- | :--- |
| **Total Execution Time (s)** |  |
| **Total Events** |  |
| **Events per Second (Throughput)** |  |
| **Minimum Latency (ms)** |  |
| **Average Latency (ms)** |  |
| **Maximum Latency (ms)** |  |

---

## 4. Hypervisor Performance Comparison

| Performance Metric | Type-1: Proxmox VE | Type-2: VMware Workstation | Difference / Variation |
| :--- | :--- | :--- | :--- |
| **Total Execution Time (s)** |  |  |  |
| **Total Events** |  |  |  |
| **Events per Second** |  |  |  |
| **Minimum Latency (ms)** |  |  |  |
| **Average Latency (ms)** |  |  |  |
| **Maximum Latency (ms)** |  |  |  |

---

## 5. Observations
- The recorded benchmark metrics should be compared by assessing the total execution time, throughput (events per second), and average latency obtained on both virtual machines.
- Performance characteristics should be analyzed in relation to the architectural differences between Type-1 (bare-metal hypervisor running directly on server hardware) and Type-2 (hosted hypervisor executing on top of a host operating system layer).
- Any observed differences in event rates and latency reflect the virtualization overhead introduced by the respective hypervisor architecture under the tested CPU compute workload.

---

## 6. Conclusion
Virtual machines with identical configurations (2 vCPU, 2 GB RAM, 20 GB disk) were successfully provisioned and evaluated on both Proxmox VE (Type-1) and VMware Workstation (Type-2) hypervisors. CPU performance was benchmarked using the Sysbench suite with prime number calculations up to 20,000. The comparative analysis of execution times, throughput, and latency values demonstrates the performance impact of bare-metal versus hosted hypervisor architectures under equivalent computational workloads.
