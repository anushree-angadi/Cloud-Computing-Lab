# Hypervisor Performance Analysis

## 1. Experiment Objective

The objective of this experiment is to compare the CPU performance of a
Type 1 hypervisor (Proxmox) and a Type 2 hypervisor (VMware) using Sysbench.

## 2. Benchmark Configuration

Both tests were performed using:

- Benchmark: Sysbench CPU
- Threads: 1
- Prime number limit: 20000
- Test duration: approximately 10 seconds

## 3. Results

| Metric | Type 1 – Proxmox | Type 2 – VMware |
|---|---:|---:|
| CPU speed (events/sec) | 1689.43 | 1058.76 |
| Total events | 16903 | 10589 |
| Average latency (ms) | 0.59 | 0.94 |
| Minimum latency (ms) | 0.57 | 0.72 |
| Maximum latency (ms) | 1.09 | 5.32 |
| 95th percentile (ms) | 0.68 | 1.61 |

## 4. Performance Analysis

The Type 1 Proxmox configuration achieved 1689.43 events per second,
while the Type 2 VMware configuration achieved 1058.76 events per second.

The Type 1 result produced approximately 59.6% more events per second
than the Type 2 result.

The average latency was 0.59 ms for Type 1 and 0.94 ms for Type 2.
The Type 1 configuration therefore had approximately 37.2% lower
average latency.

The maximum latency was 1.09 ms for Type 1 and 5.32 ms for Type 2.

## 5. Observations

- Type 1 recorded higher CPU throughput in this Sysbench test.
- Type 1 recorded lower average latency.
- Type 1 recorded lower maximum latency.
- The 95th-percentile latency was also lower for Type 1.
- Both tests used one thread and a prime-number limit of 20000.

## 6. Conclusion

Based on the Sysbench CPU benchmark, the Type 1 Proxmox configuration
showed higher CPU throughput and lower latency than the Type 2 VMware
configuration under the tested conditions.

The results are specific to the hardware, virtual-machine
configuration, and benchmark settings used in this experiment.
