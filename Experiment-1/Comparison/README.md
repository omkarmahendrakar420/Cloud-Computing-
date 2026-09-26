# Hypervisor Performance Comparison

## Overview
This section presents the comprehensive empirical CPU performance comparison between a **Type-1 bare-metal hypervisor (Proxmox VE)** and a **Type-2 hosted hypervisor (VMware Workstation)**.

Both environments were provisioned with identical virtual hardware limits (Ubuntu 24.04, 2 vCPU, 2 GB RAM, 20 GB Disk) and evaluated using the Sysbench CPU workload (`--cpu-max-prime=20000`).

---

## Performance Comparison Table

| Performance Metric | Type-1: Proxmox VE | Type-2: VMware Workstation | Performance Difference / Advantage |
| :--- | :---: | :---: | :---: |
| **Hypervisor Architecture** | **Type-1 (Bare-Metal)** | **Type-2 (Hosted)** | Direct hardware access vs OS layer |
| **Guest Operating System** | **Ubuntu 24.04 LTS** | **Ubuntu 24.04 LTS** | Standardized |
| **Allocated vCPU** | **2 vCPU** | **2 vCPU** | Standardized |
| **Allocated RAM** | **2 GB** | **2 GB** | Standardized |
| **Allocated Disk** | **20 GB** | **20 GB** | Standardized |
| **Total Execution Time** | **10.0005 s** | **10.0006 s** | Fixed 10s benchmark window |
| **Total Events Processed** | **17,494** | **7,077** | **+147.2% More Events** (Proxmox VE) |
| **Events per Second (Throughput)** | **1,749.16** | **707.43** | **2.47× Higher Throughput** (Proxmox VE) |
| **Average Latency** | **0.57 ms** | **1.41 ms** | **59.6% Lower Latency** (Proxmox VE) |

---

## Performance Comparison Evidence

### Official Lab Comparison Table Screenshot
![Hypervisor Performance Comparison](./screenshots/01-hypervisor-performance-comparison.png)
*Figure: Empirical comparison table captured from the completed benchmark analysis.*

---

## Architectural Insights

```
Type-1 (Proxmox VE):
[ Guest VM ] ---> [ Proxmox VE (KVM Kernel) ] ---> [ Physical CPU ]
(Direct hardware execution via Intel VT-x/AMD-V -> Minimal virtualization overhead)

Type-2 (VMware Workstation):
[ Guest VM ] ---> [ VMware VMM Engine ] ---> [ Windows Host OS ] ---> [ Physical CPU ]
(Double scheduling & host OS kernel context switching -> Higher overhead & latency)
```

For complete mathematical breakdown, analysis of latency distribution, and formal conclusions, refer to [**`performance-analysis.md`**](./performance-analysis.md).
