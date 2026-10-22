# Chapter 3 — AI Operations

**Official exam weight: 22%**  
**Mock-exam allocation: 11 of 50 questions**

## Learning Objectives

By the end of this chapter, you should be able to:

1. Describe AI data center management and monitoring essentials.
2. Explain cluster orchestration and job scheduling.
3. Select and interpret key GPU health and performance measures.
4. Compare virtualization options for accelerated infrastructure.

---

## 3.1 Operate the Whole Service, Not Only the GPU

AI operations spans multiple layers:

```text
Business/application outcome
  Quality, correctness, user experience, cost

Model service
  Requests, latency, throughput, errors, queue depth, batch size

Job and orchestration layer
  Queue wait, placement, failures, retries, utilization, fairness

Accelerated system
  GPU/CPU/memory, NVLink/PCIe, NICs, local disks, drivers

Cluster infrastructure
  Compute fabric, storage, power, cooling, management services

Facility
  Utility power, UPS/PDU, temperature, humidity, leaks, physical security
```

A green GPU dashboard cannot prove an AI service is healthy. Correlate infrastructure telemetry with scheduler, application, and model-service metrics.

### The Four Signals Pattern

Across services, start with:

- **Latency:** how long work takes.
- **Traffic:** how much work arrives.
- **Errors:** failed or incorrect work.
- **Saturation:** how close a constrained resource is to its limit.

For batch clusters, add queue wait, completion time, throughput, and scheduling fairness.

---

## 3.2 Management Planes and Access

### In-Band Management

Uses the production/service network and the host operating system. It is convenient for normal administration but may be unavailable when the host OS or production network fails.

### Out-of-Band Management

Uses a dedicated management path, commonly through a **Baseboard Management Controller (BMC)**. It can provide functions such as:

- Power on/off/reset and remote console.
- Hardware inventory and sensor readings.
- Firmware management and hardware event logs.
- Recovery access when the host OS is unavailable.

Protect OOB networks strongly: isolate them, restrict access, use strong authentication, log activity, patch firmware, and avoid exposure to untrusted networks.

### Redfish and SNMP

- **Redfish:** standards-based RESTful management interface for modern servers and infrastructure.
- **SNMP:** widely used protocol for device monitoring and alerts; secure versions and controlled access are important.

### NVIDIA Base Command Manager

Base Command Manager (BCM) supports cluster installation, provisioning, administration, inventory, monitoring, software/firmware management, networking, and workload-manager integration. Its role is broader than GPU telemetry alone.

### Configuration and Change Control

Maintain a known-good configuration for:

- Firmware, BIOS, BMC, and switch software.
- Operating-system image and kernel.
- Driver, CUDA, libraries, container toolkit, and frameworks.
- Scheduler/orchestrator and plugins/operators.
- Network, storage, security, and monitoring policies.

Roll out changes in stages, validate compatibility, preserve rollback paths, and record what changed. “Latest” is not automatically “compatible.”

---

## 3.3 NVIDIA GPU Management and Monitoring

### Tools and Roles

| Tool | Best-known role |
|---|---|
| `nvidia-smi` | Quick local inventory, status, process, and configuration inspection |
| NVML | Programmatic GPU management/monitoring library used by tools |
| DCGM | Fleet-oriented data center GPU telemetry, health, diagnostics, policy, accounting, and NVSwitch monitoring |
| DCGM Exporter | Exposes selected DCGM telemetry in Prometheus format |
| Prometheus | Scrapes and stores time-series metrics |
| Grafana | Visualizes and dashboards metrics |
| BMC / Redfish | Hardware/platform management independent of host OS |

`nvidia-smi` is valuable for triage, but manually running it on hundreds of nodes is not a monitoring architecture. DCGM and an observability pipeline provide centralized, historical, alertable data.

### Core GPU Measures

#### Utilization

- **GPU/SM utilization:** proportion of the sample interval during which kernels were active.
- **Tensor activity:** whether AI-oriented matrix units are active, when exposed by the metric set.
- **Memory bandwidth activity:** pressure on GPU memory transfers.
- **Encoder/decoder utilization:** relevant to video pipelines.

High utilization is not automatically efficient; a slow or inappropriate kernel may keep the GPU busy. Low utilization is not automatically bad; a latency service may deliberately maintain headroom.

#### Memory

- Used versus total GPU memory.
- Memory-bandwidth utilization.
- Allocation failures or out-of-memory events.
- ECC and memory error indicators.

Allocated memory is not the same as active computation. A model can occupy most memory while serving no requests.

#### Thermals, Power, and Clocks

- GPU temperature and thermal limits.
- Power draw and enforced power limit.
- Graphics/SM and memory clocks.
- Throttling reasons.

If temperature or power limits cause clock throttling, performance can drop even though utilization remains high.

#### Reliability and Interconnect

- Correctable and uncorrectable ECC errors.
- Xid/system error events and health state.
- Retired/remapped memory pages where supported.
- PCIe replay/errors and link characteristics.
- NVLink/NVSwitch link state, throughput, and errors.

An uncorrectable error or repeated fault trend may require draining the node and hardware investigation. One metric alone is rarely enough; correlate time, job, node, topology, and event logs.

### Golden Signals for Common Workloads

| Workload | Key outcome metrics | Supporting infrastructure metrics |
|---|---|---|
| Distributed training | Time/step, samples/sec, scaling efficiency, loss | GPU activity, collective time, network, storage, throttling |
| Online inference | p50/p95/p99 latency, requests/sec, error rate, cost/request | Queue depth, batch size, GPU memory/activity, power, network |
| Batch inference | Items/sec, completion time, cost/item | GPU activity, I/O throughput, scheduler wait |
| Shared cluster | Useful utilization, queue wait, job success, fairness | Fragmentation, idle reserved GPUs, preemption, failed nodes |

### Monitoring Design

Good monitoring includes:

- A baseline for healthy operation.
- Time synchronization across systems.
- Labels for node, GPU, job, tenant, model, and topology.
- Appropriate sampling and retention.
- Alerts based on actionable symptoms and sustained conditions.
- Runbooks with owner, impact, validation, remediation, and escalation.
- Capacity and trend reports, not only incident alerts.

Avoid alerting on a single instantaneous spike without context. Use thresholds, duration, rate of change, and correlated signals.

---

## 3.4 Troubleshooting Method

Use a repeatable evidence-driven process:

1. **Define the symptom:** slow training, high tail latency, failed jobs, OOM, low utilization, or hardware error.
2. **Establish scope:** one GPU, one node, one rack, one job, one model, or the full cluster.
3. **Check recent changes:** software, firmware, topology, data, workload, power, or environment.
4. **Follow the dependency path:** application → scheduler → GPU/CPU → interconnect → storage/network → facility.
5. **Compare:** healthy peer, previous baseline, or known-good job.
6. **Test one hypothesis safely:** avoid changing several variables at once.
7. **Validate recovery and capture learning:** update the runbook and preventive alert.

### Symptom Map

| Symptom | Evidence to inspect first |
|---|---|
| Low GPU utilization | CPU/preprocessing, data loader, storage, queueing, batch size, synchronization |
| High utilization but low application throughput | Kernel efficiency, precision, memory bandwidth, clocks/throttling, communication |
| One worker slows the job | Per-GPU/per-node metrics, link errors, thermal/power limits, storage path, placement |
| OOM | Model/batch/sequence size, fragmentation, competing processes, memory leak |
| Tail latency spikes | Queue depth, dynamic batching delay, contention, cold starts, throttling, network |
| Repeated job failure on one node | DCGM diagnostics, Xid/ECC/link events, system logs; drain suspect node |

---

## 3.5 Containers, Orchestration, and Scheduling

### Containers

Containers package an application and user-space dependencies while sharing the host kernel. They improve portability and reproducibility but are not hardware virtual machines. A containerized CUDA application still needs a compatible host GPU driver and a runtime path that exposes GPUs to the container.

The NVIDIA Container Toolkit enables supported container runtimes to use NVIDIA GPUs.

### Kubernetes

Kubernetes is commonly used for containerized, service-oriented, and cloud-native workloads. Relevant concepts include:

- **Pod:** smallest deployable unit.
- **Node:** worker machine that runs pods.
- **Requests/limits:** resource declarations used for placement and control.
- **Device plugin / DRA driver:** advertises and allocates specialized devices such as GPUs.
- **Labels, taints, tolerations, affinity:** influence eligible nodes and placement.
- **DaemonSet:** runs a pod on selected nodes, useful for node-level agents such as exporters.
- **Operator:** automates lifecycle of complex software using controllers and custom resources.

### NVIDIA GPU Operator

The GPU Operator automates installation and lifecycle management of the NVIDIA software components needed to use GPUs in Kubernetes, including drivers, container toolkit, device plugin, feature discovery, MIG management, and DCGM-based monitoring as configured.

It does not replace Kubernetes itself or guarantee that applications are efficiently written.

### Slurm

Slurm is a workload manager widely used for HPC and large distributed training. Key concepts include:

- Jobs and job steps.
- Partitions/queues.
- Resource requests and allocations.
- Priorities, reservations, dependencies, and limits.
- Backfill scheduling.
- Accounting and fair-share policies.

Slurm is a strong fit for queued, long-running, tightly coupled batch jobs. Kubernetes is a strong fit for containerized applications and services. Organizations can use either or both depending on workload and operating model.

### Scheduling Goals

A scheduler balances competing objectives:

- High useful utilization.
- Fairness and quotas among teams.
- Priority and deadlines.
- Short queue time.
- Data and topology locality.
- Isolation and policy compliance.
- Reliability and maintenance needs.

### Important Scheduling Concepts

- **Gang scheduling:** start all required resources for a distributed job together; avoids partial allocation that cannot make progress.
- **Bin packing:** place workloads densely to leave larger contiguous resource blocks free.
- **Fragmentation:** enough total GPU capacity exists, but not in the required shape/type/topology.
- **Backfill:** run smaller jobs in gaps without delaying a reserved higher-priority job.
- **Preemption:** pause/terminate lower-priority work so more important work can run; checkpointing reduces lost work.
- **Topology-aware placement:** co-locate communicating tasks on favorable GPU/network paths.
- **Quota/fair share:** control resource access over time across users or teams.

### Why “Allocated” Is Not “Useful”

A scheduler may report every GPU allocated while jobs are input-bound, sleeping, failed, or reserving unused memory. Measure useful workload throughput and device activity, not allocation alone.

---

## 3.6 Virtualizing Accelerated Infrastructure

### Containers Versus Virtual Machines

| Property | Container | Virtual machine |
|---|---|---|
| Kernel | Shares host kernel | Has a guest OS/kernel |
| Isolation | Process/namespace/cgroup based | Hypervisor and hardware-assisted boundary |
| Startup/overhead | Usually lower | Usually higher |
| GPU access | Container toolkit/device allocation | Passthrough or vGPU mechanisms |

Containers and VMs can be combined; for example, containers can run inside GPU-enabled VMs.

### GPU Passthrough

An entire physical GPU is assigned directly to one VM.

Advantages:

- Near-native, exclusive access.
- Simple performance model and strong workload ownership.

Tradeoffs:

- The GPU cannot be shared among multiple VMs while passed through.
- Reduced consolidation and potentially less flexible migration/management.
- Platform IOMMU, firmware, and hypervisor requirements apply.
- NVLink-connected GPUs may need assignment constraints.

Best fit: a VM needs exclusive performance and an entire GPU.

### NVIDIA vGPU

vGPU software exposes supported physical GPU resources to multiple VMs through defined profiles and management software.

Advantages:

- Consolidation and VM-level isolation.
- Flexible profiles for graphics or compute use cases where supported.
- Enterprise virtualization features and management integration.

Tradeoffs:

- Licensing and compatibility matrices matter.
- Sharing mode/profile affects performance predictability.
- Hypervisor, host driver/manager, and guest driver versions must align.

Best fit: multiple VM tenants or applications need managed fractional GPU access.

### Multi-Instance GPU (MIG)

MIG spatially partitions a supported GPU into isolated GPU instances with dedicated portions of compute and memory resources.

Advantages:

- Hardware-enforced resource separation.
- Predictable resource slices for smaller workloads.
- Improved utilization when one workload does not need a full GPU.

Tradeoffs:

- Only supported GPUs and instance profiles.
- Each instance has a fixed resource shape; fragmentation can occur.
- Some workloads need a full GPU or different sharing approach.
- Reconfiguration and orchestration need operational planning.

Best fit: multiple smaller workloads need predictable isolated slices.

### Time Slicing

Multiple workloads share GPU execution time. It can improve concurrency but does not create the same dedicated memory/compute partitions as MIG. Performance becomes less predictable under contention.

### Selection Summary

| Requirement | Typical choice |
|---|---|
| One VM needs the full physical GPU | Passthrough |
| Several VMs need managed GPU profiles | vGPU |
| Several workloads need isolated hardware slices | MIG |
| Lightweight container concurrency with flexible sharing | Time slicing or scheduler sharing, if isolation needs permit |
| Bare-metal job needs all GPU performance | Direct physical allocation |

### Compatibility Is a System Property

Validate the complete matrix: GPU, server firmware, IOMMU/BIOS, hypervisor, vGPU manager, guest driver, host driver, CUDA, container runtime, orchestrator, MIG profile, and licensing. A component being supported individually does not prove the combination is supported.

---

## 3.7 Operational Scenarios

### Scenario A: Dashboard shows 100% allocated, but users report poor throughput

Compare DCGM activity and application throughput with scheduler allocations. Find idle reservations, blocked data loaders, jobs waiting at synchronization barriers, and fragmented resources.

### Scenario B: One GPU reports rising uncorrectable errors

Correlate with DCGM health, system logs, job failures, and interconnect data. Drain the node if appropriate, preserve evidence, run approved diagnostics, and follow hardware support procedures. Do not repeatedly return a suspect device to production without validation.

### Scenario C: Small inference services waste entire GPUs

If hardware and workloads support it, use MIG for predictable isolated slices, or an appropriate sharing/serving method when strict partitions are unnecessary. Measure latency and interference before production rollout.

### Scenario D: Distributed jobs hold some GPUs while waiting for the rest

Use gang scheduling so the job starts only when the complete resource set is available. This prevents partial allocations from idling and blocking other work.

---

## 3.8 Exam-Focused Checklist

You should be able to answer these without notes:

- Why is out-of-band management valuable during host failure?
- What are the roles of `nvidia-smi`, NVML, DCGM, DCGM Exporter, Prometheus, and Grafana?
- Which metrics reveal activity, memory pressure, throttling, and hardware faults?
- Why must infrastructure metrics be correlated with application and job metrics?
- What does the GPU Operator automate?
- When would you choose Kubernetes, Slurm, or both?
- What are gang scheduling, backfill, bin packing, fragmentation, preemption, and topology-aware placement?
- How do containers differ from VMs?
- When should you use passthrough, vGPU, MIG, or time slicing?
- Why must the whole compatibility matrix be validated?

## Primary References

- [Official NCA-AIIO study guide](https://dam-cdn.nvd.orangelogic.com/AssetLink/x874j05hy3m3r2sor84kpvp70750m468.pdf)
- [NVIDIA DCGM overview](https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/index.html)
- [DCGM Exporter](https://docs.nvidia.com/datacenter/dcgm/latest/gpu-telemetry/dcgm-exporter.html)
- [NVIDIA GPU Operator](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/index.html)
- [NVIDIA AI Enterprise infrastructure software](https://docs.nvidia.com/ai-enterprise/planning-resource/ai-enterprise-infrastructure-software/latest/software-suite.html)
- [NVIDIA vGPU features](https://docs.nvidia.com/knowledge-base/latest/vgpu-features.html)
- [NVIDIA GPU passthrough](https://docs.nvidia.com/vgpu/latest/grid-vgpu-user-guide/using-gpu-pass-through.html)
- [NVIDIA Base Command Manager](https://docs.nvidia.com/base-command-manager/product-manuals/admin/introduction)

