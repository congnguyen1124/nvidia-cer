# NCA-AIIO Mock Exam — Answer Key and Explanations

Do not use this file until the 60-minute mock exam is complete.

## Quick Answer Key

| Q | Answer | Q | Answer | Q | Answer | Q | Answer | Q | Answer |
|---:|:---:|---:|:---:|---:|:---:|---:|:---:|---:|:---:|
| 1 | B | 11 | B | 21 | A | 31 | B | 41 | B |
| 2 | C | 12 | C | 22 | B | 32 | B | 42 | D |
| 3 | C | 13 | A | 23 | C | 33 | B | 43 | B |
| 4 | C | 14 | B | 24 | B | 34 | C | 44 | A |
| 5 | B | 15 | A | 25 | B | 35 | A | 45 | A |
| 6 | B | 16 | B | 26 | A, D | 36 | A | 46 | B |
| 7 | C | 17 | B, E | 27 | B | 37 | B, D | 47 | C |
| 8 | B | 18 | A | 28 | C | 38 | B | 48 | D |
| 9 | C | 19 | A | 29 | B | 39 | A | 49 | A, D |
| 10 | B | 20 | A, D | 30 | C | 40 | A | 50 | D |

For each **Choose TWO** question, award one point only if both selections are correct and no extra option was selected.

## Score Interpretation

This practice threshold is deliberately demanding and is not an official NVIDIA passing score.

| Score | Interpretation | Recommended action |
|---:|---|---|
| 45–50 (90–100%) | Strong | Review misses and sit one more timed recall session. |
| 40–44 (80–88%) | Likely ready for final review | Close domain gaps, especially any domain below 75%. |
| 35–39 (70–78%) | Developing | Re-read mapped sections and retest after 48 hours. |
| Below 35 | Foundation needs work | Repeat the chapters using active recall before another full mock. |

### Domain Question Sets

- **Essential AI Knowledge (19):** 1, 4, 6, 8, 11, 13, 16, 19, 21, 23, 26, 28, 31, 34, 36, 39, 42, 46, 49.
- **AI Infrastructure (20):** 2, 5, 7, 10, 12, 15, 18, 20, 24, 27, 29, 32, 35, 37, 40, 43, 45, 47, 48, 50.
- **AI Operations (11):** 3, 9, 14, 17, 22, 25, 30, 33, 38, 41, 44.

Suggested domain readiness target: **75% or higher in each domain**.

---

## Detailed Explanations

### 1. B — DL is within ML, which is within AI

AI is the broadest field. ML is an approach in which systems learn from data, and DL is an ML family based on multi-layer neural networks. AI can also include non-learning approaches such as rule-based systems.  
**Objective:** 1.3 — AI, ML, and DL concepts.

### 2. C — Training needs more than model weights

Training memory may include gradients, optimizer state, saved activations, temporary workspace, communication buffers, and the weights themselves. The amount varies with precision, optimizer, batch/sequence size, architecture, and parallelism.  
**Objective:** 2.1 — Hardware requirements for training use cases.

### 3. C — Allocation is not useful utilization

Allocated GPUs may be waiting for input, synchronization, CPU preprocessing, or another resource. Correlate scheduler records with GPU activity and application throughput before concluding the cluster is efficient.  
**Objective:** 3.1 and 3.3 — Data center monitoring and GPU measures.

### 4. C — TensorRT optimizes inference

TensorRT is an inference optimizer/compiler and runtime designed to improve latency and throughput on NVIDIA GPUs. DCGM monitors GPUs, BCM manages clusters, and GPUDirect Storage improves a supported storage data path.  
**Objective:** 1.1 and 1.6 — NVIDIA software stack and solution purpose.

### 5. B — Adding networked servers is scale-out

Scale-out adds nodes and aggregate resources across a fabric. Scale-up adds resources inside a system or tightly coupled domain.  
**Objective:** 2.2 — Scale GPU infrastructure.

### 6. B — Online inference targets service latency and throughput

Interactive inference is judged by response latency—especially tail latency—while sustaining the necessary request rate and availability. Time to train and optimizer state belong to training concerns.  
**Objective:** 1.2 — Training versus inference architecture.

### 7. C — Measure the scaling bottleneck first

Poor scaling commonly comes from collective communication, oversubscription, topology, storage, synchronization, or stragglers. Adding more nodes before identifying the limiting resource can reduce efficiency further.  
**Objective:** 2.2 and 2.7 — Scaling and network requirements.

### 8. B — NGC distributes optimized assets

The NGC Catalog provides GPU-optimized containers, models, Helm charts, and related resources. NVML is a management library, Redfish is a hardware-management interface, and NVSwitch is an interconnect component.  
**Objective:** 1.1 — NVIDIA software stack.

### 9. C — DCGM Exporter bridges telemetry to Prometheus

DCGM Exporter converts selected DCGM fields into the Prometheus exposition format. Prometheus then scrapes and stores those metrics; a visualization system such as Grafana can display them.  
**Objective:** 3.1 and 3.3 — Monitoring essentials and GPU measures.

### 10. B — OOB management survives host OS failure

The BMC uses an out-of-band path for power control, remote console, sensor data, and recovery independent of the host OS. It must be isolated and secured because it has powerful control over the server.  
**Objective:** 2.5 and 2.6 — Cluster components and facility requirements.

### 11. B — GPU Operator manages the Kubernetes GPU stack

The GPU Operator automates deployment and lifecycle of components such as the driver, container toolkit, device plugin, feature discovery, MIG management, and DCGM-based monitoring, depending on configuration. It operates within Kubernetes; it does not replace Kubernetes.  
**Objective:** 1.1 — NVIDIA software stack.

### 12. C — 800 kW × 1.25 = 1,000 kW

PUE is total facility energy divided by IT equipment energy. For a steady-state planning approximation, total facility power is `IT load × PUE = 800 × 1.25 = 1,000 kW`. Final electrical design also accounts for codes, redundancy, derating, and equipment requirements.  
**Objective:** 2.3 — Power and cooling concepts.

### 13. A — Triton is the model server

Triton Inference Server supports multiple model backends plus serving features such as dynamic batching and concurrent execution. TensorRT may optimize a model that Triton then serves.  
**Objective:** 1.1 and 1.7 — Software stack and deployment lifecycle.

### 14. B — Gang scheduling allocates the full set together

A tightly coupled job cannot progress until all requested resources are present. Gang scheduling avoids holding a useless partial allocation while the job waits for the rest.  
**Objective:** 3.2 — Orchestration and job scheduling.

### 15. A — GPUDirect Storage shortens the storage data path

GDS provides a more direct path between storage and GPU memory in supported systems, reducing CPU involvement and extra copies. GPUDirect RDMA is the analogous concept for supported network-adapter communication.  
**Objective:** 2.1 and 2.5 — Hardware and cluster components.

### 16. B — CPUs optimize general control; GPUs optimize parallel throughput

CPUs have sophisticated control, caching, and a smaller number of powerful cores. GPUs devote more resources to many-way parallel computation and high throughput. Accelerated systems use them together.  
**Objective:** 1.8 — GPU versus CPU architecture.

### 17. B and E — Temperature plus active throttling evidence

A temperature near the thermal limit combined with reduced clocks and an active thermal-throttle reason directly supports the diagnosis. Model allocation and job priority do not demonstrate thermal throttling.  
**Objective:** 3.3 — GPU monitoring measures.

### 18. A — The stated constraints favor on-premises

Steady high utilization, an existing capable facility, and strict local control can favor on-premises infrastructure. The organization still must operate hardware, software, security, capacity, power, and cooling throughout the lifecycle.  
**Objective:** 2.4 — On-premises versus cloud.

### 19. A — Riva is NVIDIA's speech AI platform

Riva targets speech recognition, translation, and speech synthesis use cases. DCGM and BCM are infrastructure operations tools; NVSwitch is an interconnect.  
**Objective:** 1.6 — NVIDIA solution use cases.

### 20. A and D — Inspect communication and the data path

Waiting during gradient synchronization points to compute-fabric/collective issues. Waiting during reads points to storage or the data pipeline. These two bottlenecks can coexist and both must be measured.  
**Objective:** 2.2, 2.5, and 2.7 — Scaling, cluster components, and networking.

### 21. A — NIM provides packaged inference microservices

NVIDIA NIM microservices package optimized inference with standard service APIs for supported models. NVML is a management library, UEFI is firmware, and SNMP is a management protocol.  
**Objective:** 1.1 and 1.6 — Software stack and NVIDIA solution purpose.

### 22. B — Slurm fits queued, tightly coupled batch training

Slurm provides mature job queues, priorities, partitions, reservations, backfill, accounting, and resource allocation for HPC and large training. Kubernetes is commonly preferred for containerized services and cloud-native application orchestration, although environments may use both.  
**Objective:** 3.2 — Cluster orchestration and scheduling.

### 23. C — Training learns; inference applies

Training repeatedly adjusts weights using data and an optimization process. Inference uses learned parameters to produce outputs for new inputs and is commonly optimized for latency, throughput, and cost.  
**Objective:** 1.2 — Training versus inference.

### 24. B — A DPU handles infrastructure services

BlueField DPUs offload, accelerate, and isolate supported networking, storage, security, and virtualization services. This frees host CPU cycles and can create a separate trusted infrastructure domain.  
**Objective:** 2.10 — DPU purpose and benefits.

### 25. B — Limits are reducing effective clocks

High device activity combined with rising temperature, lower clocks, and reduced throughput indicates throttling. Utilization describes activity, not work completed per second, so it can remain high while performance falls.  
**Objective:** 3.3 — GPU monitoring measures.

### 26. A and D — Reproducibility needs inputs, code, configuration, and environment

Dataset, model/code, hyperparameters, framework/library/container versions, and related configuration define the run. Production workflows should also record evaluation, hardware/software compatibility, approvals, and lineage as appropriate.  
**Objective:** 1.7 — AI development and deployment lifecycle.

### 27. B — Endpoint demand exceeds uplink capacity

An oversubscribed network has more potential downstream traffic than the uplinks can carry simultaneously. Oversubscription may be economical for some traffic patterns but can harm distributed AI when many endpoints communicate concurrently.  
**Objective:** 2.8 — Data center networking concepts.

### 28. C — Deep learning uses multi-layer neural networks

Deep learning is a subset of machine learning, which is itself a subset of AI. Rule-based applications can be AI without being ML or DL.  
**Objective:** 1.3 — AI, ML, and DL concepts.

### 29. B — Small-file workloads stress metadata and IOPS

Opening and locating millions of files can bottleneck metadata operations and random I/O even when capacity and headline sequential throughput are adequate. Data sharding or packing may improve the access pattern.  
**Objective:** 2.1 and 2.5 — Hardware and cluster/storage requirements.

### 30. C — GPU Operator automates the node GPU software stack

The Operator manages the components Kubernetes nodes need to expose and operate GPUs. It neither replaces the orchestrator nor optimizes a model like TensorRT.  
**Objective:** 3.2 — Orchestration essentials.

### 31. B — NCCL provides GPU collectives

NCCL is optimized for operations such as all-reduce, all-gather, reduce, and broadcast among NVIDIA GPUs. It uses supported topology and transports; it is not itself the physical fabric.  
**Objective:** 1.1 — NVIDIA software stack.

### 32. B — Elastic cloud capacity fits temporary uncertainty

A short-lived, uncertain experiment with no facility is a strong elasticity use case. Capacity quota, region, topology, data movement, governance, and cost still need validation.  
**Objective:** 2.4 — On-premises versus cloud.

### 33. B — This is resource fragmentation

Total free capacity exists, but not in the required type, number, node, or topology. Bin packing, defragmentation strategies, queue policy, and topology-aware placement can help.  
**Objective:** 3.2 — Scheduling essentials.

### 34. C — TensorRT optimizes; Triton serves

The two products solve distinct but complementary problems. TensorRT produces/runs an optimized inference representation, while Triton handles production serving functions and supported backends.  
**Objective:** 1.1, 1.6, and 1.7 — Software, solutions, and deployment.

### 35. A — More tightly coupled resources inside a system/domain is scale-up

NVLink and NVSwitch provide high-bandwidth GPU communication in supported local or tightly coupled systems. Connecting additional servers over a scale-out fabric is a different growth dimension.  
**Objective:** 2.2 and 2.9 — Scaling and high-speed network options.

### 36. A — Metropolis targets video analytics and smart spaces

The Metropolis ecosystem, often with DeepStream, supports vision AI and intelligent video analytics. The other options are infrastructure management or virtualization technologies.  
**Objective:** 1.5 and 1.6 — AI use cases and NVIDIA solutions.

### 37. B and D — Power and heat removal are gating constraints

High-density accelerated racks require validated electrical capacity and an adequate cooling/environmental design. These constraints also influence rack layout, cabling, network topology, floor load, redundancy, and maintenance procedures.  
**Objective:** 2.3 and 2.6 — Power/cooling and facility requirements.

### 38. B — vGPU shares a supported GPU among VMs

NVIDIA vGPU provides managed profiles for VM-oriented sharing and requires supported hardware/software combinations and appropriate licensing. Passthrough assigns a physical GPU exclusively and therefore cannot assign that same full device exclusively to multiple VMs at once.  
**Objective:** 3.4 — Virtualizing accelerated infrastructure.

### 39. A — Monitor behavior and govern the next lifecycle action

Infrastructure can be healthy while model quality degrades because the input distribution or real-world relationship changed. Monitor model/data behavior, preserve lineage, and use controlled retraining, validation, rollout, or rollback.  
**Objective:** 1.7 — AI lifecycle.

### 40. A — RoCE carries RDMA over Ethernet

RoCE enables RDMA semantics on a properly designed Ethernet fabric. Congestion management, loss behavior, topology, telemetry, and configuration matter to performance.  
**Objective:** 2.8 and 2.9 — Network protocols and high-speed options.

### 41. B — BMC/OOB is independent of the host OS

The BMC provides remote power, console, sensor, inventory, and event access through the OOB network. Secure it as privileged infrastructure.  
**Objective:** 3.1 — Data center management essentials.

### 42. D — Progress came from a reinforcing full stack

Larger datasets, accelerated compute, algorithmic innovation, fast networks, optimized software, and accessible platforms reinforced one another. No single factor explains modern AI adoption.  
**Objective:** 1.4 — Factors contributing to AI improvement and adoption.

### 43. B — Liquid cooling shifts rather than removes facility work

Liquid cooling can handle high heat density effectively, but introduces coolant distribution, water-quality, leak-detection, redundancy, and maintenance requirements. It remains part of an engineered facility system.  
**Objective:** 2.3 and 2.6 — Cooling and facility requirements.

### 44. A — Passthrough assigns the whole GPU to one VM

Passthrough is suitable when a VM needs exclusive, near-native use of a physical GPU. It sacrifices sharing and requires supported IOMMU, firmware, hypervisor, device-topology, and guest-driver configuration.  
**Objective:** 3.4 — Virtualization considerations.

### 45. A — Separation protects predictable workload communication

Training collectives and storage I/O can both be bursty and bandwidth intensive. Separate or carefully isolated fabrics prevent one class from unexpectedly degrading the other and simplify capacity planning and failure isolation.  
**Objective:** 2.5 and 2.7 — Cluster and networking considerations.

### 46. B — Containers still depend on a compatible host path

Containers package user-space software but share the host kernel. GPU access still requires a compatible host NVIDIA driver plus the runtime/toolkit path that makes devices and libraries available to the container.  
**Objective:** 1.1 and 1.7 — Software stack and deployment components.

### 47. C — About 83%

`Speedup = 100 / 30 = 3.33`. `Efficiency = 3.33 / 4 = 0.833`, or about 83%. The gap from 100% reflects overhead such as communication, synchronization, serial work, or imbalance.  
**Objective:** 2.2 — Scale GPU infrastructure.

### 48. D — BlueField plus DOCA targets infrastructure offload

This is the DPU use case: execute supported software-defined network, storage, and security services near the data path and outside the host's primary application CPU domain. DOCA provides the supporting software framework.  
**Objective:** 2.10 — DPU purpose and benefits.

### 49. A and D — The GPU may be input-starved

Bursty activity followed by idle gaps often means the accelerator is waiting for CPU preprocessing, data-loader workers, storage, or another input stage. Measure those stages before adding GPUs, which may only amplify the starvation.  
**Objective:** 1.2 and 1.8 — Workload requirements and CPU/GPU architecture.

### 50. D — This combines on-premises and cloud capacity

Using private infrastructure for steady/sensitive work and public cloud for bursts is a hybrid strategy. It provides placement flexibility but adds identity, data movement, networking, tooling, observability, and governance complexity.  
**Objective:** 2.4 — On-premises versus cloud.

---

## Objective-Level Remediation

| If you missed... | Review |
|---|---|
| AI/ML/DL, training/inference | Chapter 1 sections 1.1–1.3 |
| CPU/GPU and memory | Chapter 1 section 1.4; Chapter 2 section 2.1 |
| NVIDIA software/product matching | Chapter 1 sections 1.5–1.7 |
| Scaling, storage, networking | Chapter 2 sections 2.2–2.5 |
| Cloud/on-premises | Chapter 2 section 2.6 |
| Power, cooling, facilities | Chapter 2 section 2.7 |
| DPU | Chapter 2 section 2.8 |
| Monitoring and GPU metrics | Chapter 3 sections 3.1–3.4 |
| Scheduling | Chapter 3 section 3.5 |
| Virtualization | Chapter 3 section 3.6 |

After review, answer missed questions again **without looking at the options**. If you can explain the rule and why each distractor is wrong, the concept is likely durable.

