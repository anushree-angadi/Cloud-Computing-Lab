# CC Experiment 01 — Hypervisor Performance Comparison

## Performance Evaluation of Type-1 and Type-2 Hypervisors

This experiment evaluates the CPU performance of two virtualization approaches: **Proxmox VE**, a Type-1 hypervisor, and **VMware Workstation**, a Type-2 hypervisor. Ubuntu virtual machines with similar hardware resources were created on both platforms and tested using the **Sysbench CPU benchmark**.

### Experimental Result

The Proxmox VE virtual machine achieved **1689.43 events/sec**, while the VMware Workstation virtual machine achieved **1058.76 events/sec**. The experiment shows a measurable difference in CPU throughput and latency between the two virtualization environments.

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

# 1. Aim

The main objectives of this experiment are:

- To create and configure a virtual machine using Proxmox VE.
- To create and configure a virtual machine using VMware Workstation.
- To provide comparable resources to both virtual machines.
- To execute the same CPU benchmark on both Ubuntu VMs.
- To record the benchmark measurements.
- To compare CPU throughput and latency between the two hypervisors.

---

# 2. Hypervisor Details

| Feature | Proxmox VE | VMware Workstation |
|---|---|---|
| Hypervisor Category | Type-1 | Type-2 |
| Virtualization Method | Bare-metal | Hosted |
| Main Technology | KVM | VMware virtualization |
| Host Environment | Directly on physical hardware | Runs above a host operating system |

A Type-1 hypervisor operates directly on the physical machine, while a Type-2 hypervisor operates through an existing host operating system.

---

# 3. Common VM Configuration

Similar resources were assigned to both virtual machines to make the comparison more consistent.

| Resource | Proxmox VE VM | VMware Workstation VM |
|---|---|---|
| Operating System | Ubuntu 24.04.3 LTS | Ubuntu 64-bit |
| CPU | 2 vCPU | 2 vCPU |
| Memory | 2048 MiB | 2048 MB |
| Storage | 20 GB | 20 GB |
| Network | VirtIO / vmbr0 | NAT |
| Benchmark | Sysbench CPU 1.0.20 | Sysbench CPU 1.0.20 |
| Prime Number Limit | 20000 | 20000 |

---

# 4. Type-1 Hypervisor – Proxmox VE

## 4.1 VM Configuration

The virtual machine was created in Proxmox VE with the following configuration:

- **VM Name:** `CC-Exp1-Type1`
- **CPU:** 2 cores
- **CPU Type:** `x86-64-v2-AES`
- **Memory:** 2048 MB
- **Disk:** 20 GB
- **Network:** VirtIO with `vmbr0`
- **Virtualization:** KVM

### Proxmox Dashboard

The Proxmox dashboard shows the available virtual machines and the Proxmox server environment.

![Proxmox Dashboard](screenshots/type1-proxmox/01-proxmox-dashboard.png)

**Figure 1: Proxmox VE dashboard.**

---

## 4.2 VM Creation and Configuration

The VM configuration was checked before completing the VM creation. The configuration includes 2 CPU cores, 2048 MB memory, 20 GB storage, and a VirtIO network interface.

![Proxmox VM Configuration](screenshots/type1-proxmox/02-proxmox-vm-configuration.png)

**Figure 2: Proxmox VM configuration before creation.**

---

## 4.3 Running Virtual Machine

After creation, the `CC-Exp1-Type1` virtual machine was started successfully.

![Proxmox VM Running](screenshots/type1-proxmox/03-proxmox-vm-running.png)

**Figure 3: CC-Exp1-Type1 VM running in Proxmox VE.**

---

## 4.4 Commands Used

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
