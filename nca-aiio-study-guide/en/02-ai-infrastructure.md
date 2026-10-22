# Chapter 2 — AI Infrastructure

**Official exam weight: 40%**  
**Mock-exam allocation: 20 of 50 questions**

## Learning Objectives

By the end of this chapter, you should be able to:

1. Identify hardware requirements for different training workloads.
2. Explain how and why GPU infrastructure scales.
3. Describe data center power and cooling concepts.
4. Compare on-premises, cloud, and hybrid infrastructure.
5. Identify the components of an accelerated cluster.
6. Recognize facility requirements and dependencies.
7. Determine networking requirements for AI workloads.
8. Explain data center network protocols and concepts.
9. Match high-speed network options to use cases.
10. Explain the purpose and benefits of a DPU.

---

## 2.1 Start With the Workload

Infrastructure design starts with requirements, not a preferred product. Collect at least:

- Model architecture and parameter count.
- Training, fine-tuning, inference, simulation, or data analytics purpose.
- Numerical precision and optimization method.
- Model weights, activations, gradients, optimizer state, and temporary workspace needs.
- Dataset size, file size distribution, preprocessing, and access pattern.
- Single-node versus distributed execution.
- Expected users, concurrency, latency, throughput, and availability targets.
- Data governance, locality, sovereignty, and security constraints.
- Growth, burst, deadline, budget, power, cooling, rack, and support constraints.

### GPU Memory Is a Hard Constraint

A GPU can be computationally fast and still be unsuitable if the workload does not fit. Training memory typically includes more than model weights:

```text
Approximate training footprint =
  weights + gradients + optimizer state + activations + communication/workspace buffers
```

The exact footprint depends on precision, optimizer, batch size, sequence length, architecture, parallelism, checkpointing, and implementation. Do not use a simple “parameter count × bytes” calculation as the entire training requirement.

Common responses to a memory shortfall include:

- Reduce batch size or sequence length when acceptable.
- Use mixed/lower precision where quality and stability allow.
- Apply activation checkpointing.
- Shard parameters, gradients, and optimizer state.
- Use tensor, pipeline, or other model-parallel methods.
- Select GPUs with more memory or use a system with a stronger GPU fabric.

### Balance the Node

| Resource | Why it matters | Underprovisioning symptom |
|---|---|---|
| GPUs | Parallel compute and accelerator memory | Long compute phases, inability to fit model |
| CPU | Input preparation, orchestration, serial work | GPUs wait while CPU is saturated |
| System RAM | Dataset cache, preprocessing, application state | Paging, reloads, OOM failures |
| Local storage | Cache, scratch, checkpoints | Input stalls or slow checkpoint writes |
| PCIe / host I/O | Host-device and device I/O | Transfer bottleneck despite idle compute |
| GPU fabric | Intra-system GPU communication | Poor multi-GPU scaling |
| Network fabric | Inter-node collectives and storage access | High communication time, stragglers |

---

## 2.2 Scale-Up and Scale-Out

### Scale-Up

Scale-up increases capability inside one system or tightly coupled domain—for example, adding GPUs to a server or connecting more GPUs through NVLink/NVSwitch.

Benefits:

- Very high-bandwidth, low-latency GPU communication.
- Simpler job topology than a multi-node deployment.
- Useful for models requiring close cooperation among GPUs.

Limits:

- Finite GPUs, memory, power, cooling, and expansion per system.
- Larger fault domain if many resources are tightly integrated.

### Scale-Out

Scale-out adds servers/nodes connected by a high-speed network.

Benefits:

- Greater total compute and memory capacity.
- Incremental growth and support for many concurrent jobs.
- Large distributed training and high-throughput inference.

Challenges:

- Network communication and synchronization overhead.
- Topology-aware scheduling and software complexity.
- Storage throughput must grow with compute.
- More devices, cables, failures, and operational burden.

### Scaling Efficiency

If one GPU completes a workload in 100 minutes, perfect scaling would let four GPUs finish in 25 minutes. Real scaling is lower because of communication, synchronization, imbalance, serial work, and input bottlenecks.

```text
Speedup(n) = single-resource time / n-resource time

Scaling efficiency = Speedup(n) / n
```

Adding GPUs when efficiency has already collapsed wastes capacity. Measure compute time, collective communication, I/O, and imbalance first.

### Common Parallelism Patterns

- **Data parallelism:** each GPU holds a model replica and processes different data; gradients must be synchronized.
- **Tensor parallelism:** individual tensor operations are split across GPUs; requires frequent high-speed communication.
- **Pipeline parallelism:** model stages run on different GPUs; bubble time and stage balance matter.
- **Expert parallelism:** mixture-of-experts components are distributed; routing traffic can stress the network.

Communication-intensive methods make topology, latency, and bandwidth especially important.

---

## 2.3 Accelerated Cluster Components

```text
Users / CI pipelines
        |
Login, API, and management nodes
        |
Scheduler / orchestrator / cluster manager
        |
-------------------------------------------------
| GPU compute nodes | High-speed compute fabric |
| Shared storage    | In-band service network   |
| BMCs and switches | Out-of-band management    |
-------------------------------------------------
        |
Power, cooling, racks, cabling, security, monitoring
```

### Major Components

1. **GPU compute nodes** — GPUs, CPUs, RAM, local storage, NICs/SuperNICs, and GPU interconnect.
2. **Compute fabric** — carries distributed workload communication and collectives.
3. **Storage fabric and systems** — feed training data and absorb checkpoints/results.
4. **In-band network** — provisioning, user access, services, and general data movement.
5. **Out-of-band (OOB) network** — BMC, switch, and device management independent of the host OS.
6. **Management/login nodes** — authentication, cluster services, orchestration, monitoring, and user entry points.
7. **Orchestration and scheduling** — commonly Kubernetes for containerized services and Slurm for queued HPC/AI jobs.
8. **Software platform** — OS, drivers, CUDA, libraries, container runtime, framework, and workload software.
9. **Facility infrastructure** — power distribution, UPS/generator strategy, cooling, racks, floor loading, fire protection, and physical security.

### Why Separate Network Planes?

Separation improves predictability, security, and recovery:

- Heavy storage traffic should not disrupt latency-sensitive training collectives.
- OOB access should remain available when an OS or production network fails.
- Management and user traffic can be governed separately.

Physical separation is not always required; logical isolation may use VLANs, VRFs, or VXLAN/EVPN. The design must still protect bandwidth and failure domains.

### Resilience Principles

- Remove single points of failure where availability requires it.
- Use redundant power supplies, PDUs, network paths, and management services appropriately.
- Preserve topology awareness so a scheduler can choose healthy, well-connected resources.
- Checkpoint long jobs to durable storage.
- Maintain spare capacity and documented replacement procedures.
- Validate failover; redundancy that has never been tested is only an assumption.

---

## 2.4 Storage for AI

AI storage is not just a capacity problem.

| Requirement | Example pressure |
|---|---|
| Capacity | Large datasets, model artifacts, checkpoints |
| Sequential throughput | Streaming large training samples to many nodes |
| IOPS | Many small files or random accesses |
| Metadata performance | Directory scans and millions of small objects/files |
| Write throughput | Frequent distributed checkpoints |
| Durability | Protect training progress and governed assets |
| Parallel access | Many workers reading simultaneously |

### Data Tiers

- **Object/archive storage:** durable, economical capacity and source-of-truth datasets.
- **Shared high-performance storage:** parallel access for active workloads and checkpoints.
- **Local NVMe/cache:** lowest-latency staging and scratch data, but may be ephemeral.

### GPU-Direct I/O Concepts

- **GPUDirect RDMA** allows supported network adapters to exchange data with GPU memory more directly, reducing CPU involvement and extra copies.
- **GPUDirect Storage (GDS)** provides a more direct data path between storage and GPU memory in supported configurations.

These technologies do not make slow storage fast. They reduce data-path overhead when the full hardware/software path supports them.

### Diagnosing a Storage Bottleneck

Indicators include low GPU utilization that rises after data caching, high I/O wait, saturated storage links, slow epoch startup, or long checkpoint times. Add storage throughput, improve data layout/sharding, cache data, or improve preprocessing before adding GPUs.

---

## 2.5 AI Networking Requirements

### Four Questions to Ask

1. **How much traffic?** Estimate per-node and aggregate bandwidth.
2. **How sensitive is it to delay?** Distributed collectives can be latency sensitive.
3. **Can it tolerate loss/retransmission?** RDMA designs require careful fabric behavior and configuration.
4. **What is the topology?** Oversubscription, hop count, rail alignment, and path diversity affect performance.

### Key Terms

| Term | Meaning |
|---|---|
| Bandwidth | Maximum data transfer rate of a link/path |
| Throughput | Actual useful data delivered per unit time |
| Latency | Time for data to travel or an operation to complete |
| Jitter | Variation in latency |
| Packet loss | Packets dropped before delivery; may trigger recovery/retransmission |
| Oversubscription | Downstream potential traffic exceeds available uplink capacity |
| Bisection bandwidth | Capacity across a network cut dividing endpoints into two groups |
| East-west traffic | Traffic between data center systems, common in distributed AI |
| North-south traffic | Traffic entering or leaving the data center/application boundary |
| RDMA | Direct memory access over a network with reduced CPU involvement |
| Collective | Group operation such as all-reduce, broadcast, or all-gather |

### High-Speed Network Options

#### InfiniBand

A purpose-built high-performance fabric with native RDMA, low latency, and strong support for HPC/AI communication. It is commonly used for compute and storage fabrics in large-scale AI systems.

#### Ethernet

A broad ecosystem used for enterprise, management, storage, and AI fabrics. High-performance AI Ethernet deployments use careful congestion control, topology, telemetry, and acceleration. **RoCE** carries RDMA over Ethernet.

#### NVLink and NVSwitch

These form a high-bandwidth GPU communication domain in supported systems. They complement rather than replace the scale-out network: NVLink/NVSwitch commonly handles tightly coupled GPU communication locally, while InfiniBand or high-speed Ethernet connects systems.

### Protocol and Feature Map

| Technology | What to remember |
|---|---|
| TCP/IP | Reliable general-purpose networking; CPU/software overhead can be higher than RDMA paths |
| UDP | Connectionless transport; applications provide any needed reliability |
| RDMA | Lower-copy, lower-CPU remote memory access model |
| RoCE | RDMA over an Ethernet network; requires a properly engineered fabric |
| InfiniBand verbs/RDMA | Native high-performance communication model on InfiniBand |
| GPUDirect RDMA | Directer NIC-to-GPU data path in supported systems |
| NCCL | GPU collective communication library using available topology/transports |

### Topology

- **Leaf-spine / fat-tree:** predictable multi-path connectivity and high bisection bandwidth when appropriately provisioned.
- **Rail-optimized design:** aligns corresponding NIC/GPU paths across nodes to improve predictable collective traffic patterns.
- **Non-blocking fabric:** designed so simultaneous traffic can proceed without oversubscription at the target design point.

### Congestion and Hot Spots

Average bandwidth can look healthy while a subset of links is overloaded. Watch per-port utilization, queueing, errors, dropped packets, ECN/PFC behavior where applicable, and application collective timing. Topology-aware placement can reduce cross-fabric traffic and stragglers.

---

## 2.6 On-Premises, Cloud, and Hybrid

### On-Premises

Advantages:

- Direct control over hardware, topology, security, maintenance, and data locality.
- Predictable performance and cost for steady high utilization.
- Easier alignment with certain sovereignty or isolated-environment requirements.
- Ability to customize networking and storage deeply.

Challenges:

- Capital expense and procurement lead time.
- Capacity planning risk: stranded capacity or insufficient capacity.
- Facility power, cooling, space, and staffing responsibility.
- Hardware lifecycle, upgrades, security, and operations remain with the organization.

### Public Cloud

Advantages:

- Rapid access and elastic capacity.
- Useful for experiments, bursts, temporary projects, and geographic reach.
- Managed services can reduce some undifferentiated operations work.
- Operating expense model; no private facility build-out for the consumed capacity.

Challenges:

- Ongoing compute, storage, and network-egress cost.
- Instance availability, quotas, region, and topology constraints.
- Data movement time and cost.
- Performance may depend on service and tenancy choices.
- Governance, sovereignty, and provider responsibility boundaries must be understood.

### Hybrid

Hybrid designs place each stage where it best fits—for example, sensitive data preparation and steady training on-premises, with cloud burst capacity for experiments or inference. The tradeoff is greater identity, data, tooling, networking, observability, and cost-management complexity.

### Decision Matrix

| Requirement | Often favors |
|---|---|
| Bursty, uncertain, short-term demand | Cloud |
| Stable, high, long-term utilization | On-premises may be economical |
| Strict local control or disconnected use | On-premises |
| Immediate access without facility build | Cloud |
| Existing data is very large and already local | Train near the data |
| Different stages have different needs | Hybrid |

There is no universally superior location. Include total cost of ownership, utilization, staffing, data movement, risk, and opportunity cost—not only hourly compute price.

---

## 2.7 Power, Cooling, and Facility Planning

Accelerated systems concentrate significant electrical and thermal load. Facility planning must precede delivery.

### Power Concepts

- **Power (kW):** instantaneous rate of energy use.
- **Energy (kWh):** power consumed over time.
- **IT load:** power used by compute, storage, and networking equipment.
- **Facility load:** IT load plus cooling, power conversion, lighting, and other overhead.
- **PUE:** `total facility energy / IT equipment energy`. Lower is generally more efficient; the theoretical floor is 1.0.
- **Nameplate/rated power:** maximum or specified design value; do not assume measured average is safe for circuit design.
- **Power redundancy:** A/B feeds, redundant PSUs, UPS, generators, and PDU design according to required availability.

### Planning Calculation

For rough planning only:

```text
Rack IT load ≈ sum of maximum planned device loads
Facility input estimate ≈ IT load × planned PUE
```

Then apply electrical codes, circuit derating, redundancy policy, startup/transient behavior, and vendor/facility requirements. A design professional must perform final sizing.

### Cooling Concepts

Nearly all IT electrical power becomes heat. Cooling must remove that heat continuously.

- **Air cooling:** front-to-back airflow, hot/cold aisle discipline, containment, adequate airflow, and pressure management.
- **Liquid cooling:** direct-to-chip or other liquid approaches can support much higher density; they require facility water loops, coolant distribution units, leak detection, water quality, and service procedures.
- **Temperature and humidity:** operate within equipment and facility environmental limits; avoid condensation and electrostatic risk.
- **Altitude:** thinner air can reduce cooling capacity; equipment guidance may derate operation.
- **Contamination:** filtration and cleanliness protect equipment and heat-transfer surfaces.

### Facility Checklist

- Utility, UPS, generator, switchgear, PDU, and branch-circuit capacity.
- Cooling capacity at both facility and rack density.
- Rack dimensions, rail compatibility, cable management, and service clearance.
- Floor loading, loading dock, freight elevator, route, door, and turning clearances.
- Fiber/copper media, transceivers, cable lengths, bend radius, and labeling.
- Grounding/bonding and applicable codes.
- Fire detection/suppression and liquid leak detection where needed.
- Physical security and environmental monitoring.
- Staging, burn-in, spares, and maintenance access.

### Dependency Chain

```text
Power/cooling constraint
    → fewer systems per rack
    → more racks and floor space
    → longer/more cables and switches
    → topology, latency, cost, and deployment changes
```

Facilities and network architecture cannot be designed independently.

---

## 2.8 Data Processing Units (DPUs)

An NVIDIA BlueField DPU combines high-speed networking with programmable processing and hardware accelerators. It runs infrastructure services separately from the host CPU.

### Main Benefits

- **Offload:** move networking, storage, security, and virtualization work away from the host CPU.
- **Acceleration:** execute supported infrastructure functions efficiently and near the data path.
- **Isolation:** place trusted infrastructure control in a separate domain from tenant/application software.
- **Free host resources:** preserve CPU cycles for application and AI work.
- **Consistent policy:** apply infrastructure services at the server edge.

### DOCA

NVIDIA DOCA is the software framework used to program and operate BlueField DPUs and supported network accelerators. Remember the pairing:

```text
BlueField = DPU hardware/platform
DOCA      = software framework/ecosystem
```

### DPU Versus GPU Versus CPU

| Processor | Primary design emphasis |
|---|---|
| CPU | General-purpose control, serial and diverse workloads |
| GPU | Highly parallel compute and tensor/matrix throughput |
| DPU | Data center infrastructure processing, movement, security, and isolation |

A DPU does not replace the GPU for neural-network training and does not replace the CPU for all general-purpose logic.

---

## 2.9 Bottleneck-First Scenarios

### Scenario A: Four nodes scale well; sixteen do not

Measure collective communication time, network hot spots, oversubscription, topology placement, stragglers, storage load, and synchronization. More nodes cannot overcome a saturated fabric.

### Scenario B: New racks exceed facility limits

Do not merely spread servers across random racks. Recalculate power, cooling, rack layout, cable distances, switch counts, topology, and failure domains as one design.

### Scenario C: Host CPUs are busy with networking and security services

A DPU can offload and isolate supported infrastructure functions, freeing CPU capacity. Confirm the application, hardware, and software path supports the required offload.

### Scenario D: Training repeatedly pauses at checkpoint time

Investigate aggregate write bandwidth, metadata pressure, checkpoint coordination, network/storage fabric contention, and storage architecture. Additional compute is unlikely to solve the pause.

---

## 2.10 Exam-Focused Checklist

You should be able to answer these without notes:

- Why is model weight size alone insufficient for training-memory sizing?
- What is the difference between scale-up and scale-out?
- What reduces distributed scaling efficiency?
- Why separate compute, storage, in-band, and out-of-band traffic?
- When do throughput, IOPS, metadata performance, and latency matter for storage?
- What is the difference between InfiniBand, Ethernet/RoCE, and NVLink?
- What do RDMA, GPUDirect RDMA, NCCL, and oversubscription mean?
- Which constraints favor on-premises, cloud, or hybrid?
- What does PUE measure?
- Why must power, cooling, rack density, and networking be co-designed?
- What functions can a DPU offload, accelerate, and isolate?

## Primary References

- [Official NCA-AIIO study guide](https://dam-cdn.nvd.orangelogic.com/AssetLink/x874j05hy3m3r2sor84kpvp70750m468.pdf)
- [NVIDIA DGX SuperPOD reference architecture](https://docs.nvidia.com/dgx-superpod/reference-architecture-scalable-infrastructure-b200/latest/)
- [DGX SuperPOD network fabrics](https://docs.nvidia.com/dgx-superpod/reference-architecture/scalable-infrastructure-b300/latest/network-fabrics.html)
- [Planning a DGX data center deployment](https://docs.nvidia.com/dgx-superpod/design-guides/dgx-superpod-data-center-design-h100/latest/planning.html)
- [NVIDIA BlueField and infrastructure offload](https://docs.nvidia.com/ai-enterprise/planning-resource/ai-enterprise-infrastructure-software/latest/software-suite.html)
- [NVIDIA AI training overview](https://www.nvidia.com/en-eu/glossary/ai-training/)
- [On-premises versus cloud for AI](https://blogs.nvidia.com/blog/whats-difference-between-ai-on-premises-in-the-cloud/)

