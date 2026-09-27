# CC Experiment 01 — Hypervisor Performance Comparison

## Performance Evaluation of Type-1 and Type-2 Hypervisors

This experiment evaluates the CPU performance of two different virtualization approaches: **Proxmox VE**, a Type-1 hypervisor, and **VMware Workstation**, a Type-2 hypervisor. Ubuntu virtual machines with similar hardware resources were created on both platforms and tested using the **Sysbench CPU benchmark**.

### Experimental Result

The Proxmox VE virtual machine achieved **1689.43 events/sec**, while the VMware Workstation virtual machine achieved **1058.76 events/sec**. The results show a measurable difference in CPU throughput and latency between the two virtualization environments.

---

## Table of Contents

1. [Aim](#1-aim)
2. [Hypervisor Details](#2-hypervisor-details)
3. [Common VM Configuration](#3-common-vm-configuration)
4. [Type-1 Hypervisor – Proxmox VE](#4-type-1-hypervisor--proxmox-ve)
5. [Type-2 Hypervisor – VMware Workstation](#5-type-2-hypervisor--vmware-workstation)
6. [Result Comparison](#6-result-comparison)
7. [Performance Discussion](#7-performance-discussion)
8. [Conclusion](#8-conclusion)
9. [Project Structure](#9-project-structure)

---

## 1. Aim

The main objectives of this experiment are:

* To create and configure a VM using **Proxmox VE**.
* To create and configure a VM using **VMware Workstation**.
* To provide comparable CPU, memory, and storage resources to both VMs.
* To execute the same CPU benchmark in both Ubuntu VMs.
* To collect the benchmark measurements.
* To compare the CPU throughput and latency obtained from both hypervisors.

---

## 2. Hypervisor Details

| Feature               | Proxmox VE                    | VMware Workstation                 |
| --------------------- | ----------------------------- | ---------------------------------- |
| Hypervisor Category   | Type-1                        | Type-2                             |
| Virtualization Method | Bare-metal                    | Hosted                             |
| Main Technology       | KVM                           | VMware virtualization              |
| Host Environment      | Directly on physical hardware | Runs above a host operating system |

A Type-1 hypervisor operates directly on the physical machine, whereas a Type-2 hypervisor works through an existing host operating system.

---

## 3. Common VM Configuration

To make the comparison more consistent, similar resources were assigned to both virtual machines.

| Resource           | Proxmox VE VM       | VMware Workstation VM |
| ------------------ | ------------------- | --------------------- |
| Operating System   | Ubuntu 22.04.5 LTS  | Ubuntu 64-bit         |
| CPU                | 2 vCPU              | 2 vCPU                |
| Memory             | 2048 MiB            | 2048 MB               |
| Storage            | 20 GB               | 20 GB                 |
| Network            | VirtIO / vmbr0      | NAT                   |
| Benchmark          | Sysbench CPU 1.0.20 | Sysbench CPU 1.0.20   |
| Prime Number Limit | 20000               | 20000                 |

The benchmark was executed for approximately **10 seconds** using a single thread.

---

# 4. Type-1 Hypervisor – Proxmox VE

## 4.1 VM Setup

The virtual machine created in Proxmox VE was configured with the following resources:

* **VM Name:** `CC-Exp1-Type1`
* **CPU:** 2 vCPU
* **CPU Type:** `x86-64-v2-AES`
* **RAM:** 2048 MiB
* **Storage:** 20 GB
* **Network:** VirtIO with `vmbr0`
* **Guest OS:** Ubuntu 22.04.5 LTS
* **Virtualization:** KVM

## 4.2 Commands Executed

```bash
hostnamectl
lscpu
free -h
df -h
top

sudo apt update
sudo apt install sysbench -y
sysbench --version
sysbench cpu --cpu-max-prime=20000 run
```

## 4.3 Benchmark Output

The Sysbench CPU test produced the following important measurements:

| Metric            | Proxmox VE |
| ----------------- | ---------: |
| Number of Threads |          1 |
| Prime Limit       |      20000 |
| Execution Time    |  10.0006 s |
| Total Events      |      16903 |
| Events/sec        |    1689.43 |
| Minimum Latency   |    0.57 ms |
| Average Latency   |    0.59 ms |
| Maximum Latency   |    1.09 ms |
| 95th Percentile   |    0.68 ms |

## 4.4 Observation

The Proxmox VM completed **16,903 events** during the benchmark period. The measured throughput was **1689.43 events/sec**, with an average latency of **0.59 ms**.

## 4.5 Screenshots

1. Proxmox VE dashboard
2. VM configuration page
3. Running VM
4. Ubuntu console
5. System information
6. Sysbench benchmark output
7. Resource monitoring

---

# 5. Type-2 Hypervisor – VMware Workstation

## 5.1 VM Setup

The VMware Workstation VM was configured using the following settings:

* **CPU:** 2 vCPU
* **Processors:** 1 processor with 2 cores
* **Memory:** 2048 MB
* **Storage:** 20 GB
* **Guest OS:** Ubuntu 64-bit
* **Network:** NAT
* **Host CPU:** 12th Gen Intel Core i5-12450H

## 5.2 Commands Executed

```bash
hostnamectl
lscpu
free -h
df -h
top

sudo apt update
sudo apt install sysbench -y
sysbench --version
sysbench cpu --cpu-max-prime=20000 run
```

The correct Sysbench parameter is:

```bash
--cpu-max-prime=20000
```

## 5.3 Benchmark Output

The important VMware Workstation measurements were:

| Metric            | VMware Workstation |
| ----------------- | -----------------: |
| Number of Threads |                  1 |
| Prime Limit       |              20000 |
| Execution Time    |          10.0002 s |
| Total Events      |              10589 |
| Events/sec        |            1058.76 |
| Minimum Latency   |            0.72 ms |
| Average Latency   |            0.94 ms |
| Maximum Latency   |            5.32 ms |
| 95th Percentile   |            1.61 ms |

## 5.4 Observation

The VMware virtual machine processed **10,589 events** during the test. Its measured CPU throughput was **1058.76 events/sec**, and the average latency was **0.94 ms**.

## 5.5 Screenshots

1. VMware VM hardware configuration
2. Ubuntu VM running
3. System configuration information
4. Sysbench benchmark result

---

# 6. Result Comparison

The benchmark results obtained from both virtual machines are summarized below.

| Parameter       |  Proxmox VE | VMware Workstation |
| --------------- | ----------: | -----------------: |
| Hypervisor Type |      Type-1 |             Type-2 |
| Execution Time  |   10.0006 s |          10.0002 s |
| Total Events    |      16,903 |             10,589 |
| Events/sec      | **1689.43** |        **1058.76** |
| Minimum Latency | **0.57 ms** |            0.72 ms |
| Average Latency | **0.59 ms** |            0.94 ms |
| 95th Percentile | **0.68 ms** |            1.61 ms |
| Maximum Latency | **1.09 ms** |            5.32 ms |

### Percentage Difference

* CPU throughput difference: approximately **59.6%**
* Average latency difference: approximately **37.2%**
* 95th percentile latency difference: approximately **57.8%**
* Maximum latency difference: approximately **79.5%**

### Performance Chart

The comparison chart can be included here to visually represent the difference in CPU throughput and latency.

---

# 7. Performance Discussion

The benchmark results show that the two virtualization platforms produced different CPU performance measurements even though similar VM resources were assigned.

### CPU Throughput

Proxmox VE recorded **1689.43 events/sec**, whereas VMware Workstation recorded **1058.76 events/sec**. Therefore, more Sysbench CPU events were completed during the same test period on the Proxmox VM.

### Latency

The average latency measured on Proxmox was **0.59 ms**, compared with **0.94 ms** on VMware Workstation. The maximum latency also showed a noticeable difference:

* Proxmox VE: **1.09 ms**
* VMware Workstation: **5.32 ms**

### Possible Reason

The difference can be related to the virtualization architecture. Proxmox VE uses a Type-1 approach with KVM, while VMware Workstation operates as a hosted virtualization platform. The additional host operating system layer in a Type-2 setup can introduce additional resource management overhead.

However, these measurements represent this particular test environment. Factors such as the physical processor, host workload, VM configuration, and system background processes can also affect benchmark results.

---

# 8. Conclusion

This experiment compared CPU performance between a **Type-1 hypervisor, Proxmox VE**, and a **Type-2 hypervisor, VMware Workstation** using Ubuntu virtual machines and the Sysbench CPU benchmark.

Both VMs were given comparable resources and tested using a prime limit of **20000**.

The measured results were:

* **Proxmox VE:** 1689.43 events/sec
* **VMware Workstation:** 1058.76 events/sec

The experiment demonstrates that virtualization architecture can influence CPU throughput and latency. In this particular test, the Proxmox VM produced higher CPU throughput and lower measured latency than the VMware Workstation VM.

---

# 9. Project Structure

```text
CC-Experiment-01-Hypervisor-Analysis/
│
├── README.md
│
├── screenshots/
│   │
│   ├── type1-proxmox/
│   │   ├── 01-proxmox-dashboard.png
│   │   ├── 02-proxmox-vm-configuration.png
│   │   ├── 03-proxmox-vm-running.jpeg
│   │   ├── 04-proxmox-ubuntu-console.jpeg
│   │   ├── 05-proxmox-system-configuration.jpeg
│   │   ├── 06-proxmox-sysbench-result.jpeg
│   │   └── 07-proxmox-resource-monitoring.jpeg
│   │
│   ├── type2-vmware/
│   │   ├── 01-vmware-vm-configuration.jpeg
│   │   ├── 02-vmware-vm-running.jpeg
│   │   ├── 03-vmware-system-configuration.jpeg
│   │   └── 04-vmware-sysbench-result.jpeg
│   │
│   └── comparison/
│       └── 01-hypervisor-performance-comparison.png
│
└── results/
    └── performance-analysis.md
```

## VM Shutdown

For Ubuntu, the VM can be shut down using:

```bash
sudo poweroff
```

For VMware Workstation, the guest can also be shut down through:

```text
VM → Power → Shut Down Guest
```
