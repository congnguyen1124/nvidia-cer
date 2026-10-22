# NCA-AIIO Exam Cram Sheet

Use this file for the final 24–48 hours. If a line is unclear, return to the referenced chapter rather than memorizing words without meaning.

## Blueprint

| Domain | Weight | Mock questions |
|---|---:|---:|
| Essential AI Knowledge | 38% | 19 |
| AI Infrastructure | 40% | 20 |
| AI Operations | 22% | 11 |

## Essential AI Knowledge

### Concept Hierarchy

```text
AI ⊃ Machine Learning ⊃ Deep Learning
```

- AI can be rule-based; it does not always learn.
- ML learns patterns from data.
- DL uses multi-layer neural networks.
- Generative AI creates new content and is a use-case category, not all AI.

### Training Versus Inference

| Training | Inference |
|---|---|
| Learns/updates weights | Applies trained weights |
| Forward + backward + optimizer | Primarily forward/token generation |
| Time to train, convergence, scaling | Latency, throughput, cost, availability |
| Large datasets, checkpoints | Requests, batching, caches |
| High inter-GPU communication at scale | Often optimized for service SLOs |

- Large batch usually raises throughput but can raise per-request waiting latency.
- Training memory includes weights, gradients, optimizer state, activations, and workspace.

### CPU Versus GPU

- CPU: fewer sophisticated cores, large caches/control logic, low-latency serial and branch-heavy work.
- GPU: many parallel units, high throughput and memory bandwidth, matrix/tensor work.
- They are complementary. CPU or I/O starvation can leave a healthy GPU idle.

### Product/Technology Match

| Need | Answer trigger |
|---|---|
| Parallel GPU programming | CUDA |
| DL primitives | cuDNN |
| Multi-GPU collectives | NCCL |
| Optimize/compile inference | TensorRT |
| Serve multi-framework models | Triton Inference Server |
| Packaged optimized model API | NIM microservice |
| Generative AI development/customization | NeMo |
| Speech AI | Riva |
| Healthcare imaging | MONAI |
| Video analytics | Metropolis / DeepStream |
| Robotics | Isaac |
| Digital twins | Omniverse |
| Edge AI | Jetson |
| Autonomous vehicles | DRIVE |
| Curated containers/models/Helm charts | NGC |
| Enterprise software/support | NVIDIA AI Enterprise |
| Kubernetes GPU software lifecycle | GPU Operator |
| GPU fleet health/telemetry | DCGM |
| Cluster provisioning/admin | Base Command Manager |
| DPU programming | DOCA |

**Classic trap:** TensorRT optimizes inference; Triton serves models. They work together.

## AI Infrastructure

### Scale

- Scale-up: more/tighter resources inside one system or NVLink/NVSwitch domain.
- Scale-out: more nodes over InfiniBand or high-speed Ethernet.
- Scaling loss comes from communication, synchronization, imbalance, serial work, and I/O.

```text
Speedup = T1 / Tn
Efficiency = Speedup / n
```

### Data Paths

| Path | Main purpose |
|---|---|
| NVLink/NVSwitch | High-bandwidth GPU communication in supported local/tightly coupled domains |
| InfiniBand | Low-latency, RDMA-capable scale-out compute/storage fabric |
| Ethernet/RoCE | Ethernet ecosystem with RDMA for engineered AI fabrics |
| PCIe | Host/device and device connectivity |
| GPUDirect RDMA | Supported NIC-to-GPU path with reduced CPU/copies |
| GPUDirect Storage | Supported storage-to-GPU path with reduced data-path overhead |

- RDMA reduces CPU involvement/copies for remote memory access.
- Oversubscription means potential downstream traffic exceeds uplink capacity.
- NCCL implements GPU collectives; it is not a physical network.

### Network Planes

- Compute fabric: distributed workload communication.
- Storage fabric: dataset and checkpoint I/O.
- In-band: provisioning, users, services, general data.
- OOB: BMC/device recovery and management independent of host OS.

### Storage

- Large streaming data → throughput.
- Millions of small files → IOPS and metadata.
- Checkpoint stalls → aggregate write path.
- Local NVMe → fast cache/scratch, often less durable.
- Shared parallel storage → concurrent workers and durable checkpoints.

### Placement

- On-premises: control, stable high utilization, data locality; requires CapEx, staff, lifecycle, facility.
- Cloud: speed and elasticity; watch recurring cost, availability/quota, egress, data governance.
- Hybrid: workload-by-workload fit; adds integration and governance complexity.
- Train near the data when movement cost/time dominates.

### Power and Cooling

```text
Energy (kWh) = Power (kW) × Time (h)
PUE = Total facility energy / IT equipment energy
Facility estimate ≈ IT load × PUE
```

- Almost all IT electrical power becomes heat.
- Plan circuits from supported design/rated values, redundancy, derating, and codes—not observed average alone.
- Higher rack density affects power, cooling, layout, cabling, topology, and floor loading.
- Liquid cooling supports high density but adds facility water, coolant, leak detection, and service requirements.

### DPU

- BlueField DPU: offloads, accelerates, and isolates network, storage, security, and virtualization services.
- Frees host CPU for applications.
- DOCA is the software framework.
- DPU ≠ GPU and DPU ≠ general CPU replacement.

## AI Operations

### Tool Chain

| Tool | Role |
|---|---|
| `nvidia-smi` | Local inspection/triage |
| NVML | Programmatic GPU management API/library |
| DCGM | Fleet GPU telemetry, health, diagnostics, policy, accounting |
| DCGM Exporter | Converts DCGM fields to Prometheus format |
| Prometheus | Scrapes/stores time series |
| Grafana | Dashboards/visualization |
| BMC/Redfish | Hardware management independent of host OS |

### GPU Signals

- Work: SM/GPU activity, tensor activity, memory bandwidth.
- Capacity: memory used/total; allocation alone is not active work.
- Limits: temperature, power, clocks, throttle reasons.
- Health: ECC, Xid, retired/remapped pages, PCIe/NVLink errors.
- Outcome: application throughput/latency, training step time, job failures.

**Classic trap:** 100% allocated does not mean 100% useful utilization.

### Scheduling

- Gang scheduling: allocate all resources for a distributed job together.
- Bin packing: pack jobs to preserve larger contiguous free blocks.
- Fragmentation: enough total resources, wrong shape/location/type.
- Backfill: run smaller jobs in gaps without delaying reserved work.
- Preemption: reclaim resources from lower-priority work.
- Topology-aware placement: place communicating tasks on favorable links.
- Slurm: queued HPC/large training jobs.
- Kubernetes: containerized services/cloud-native workloads.

### Virtualization

| Mode | Best clue |
|---|---|
| Passthrough | Entire physical GPU, exclusive to one VM |
| vGPU | Managed GPU sharing among VMs; profiles/licensing |
| MIG | Hardware-isolated spatial GPU instances on supported GPUs |
| Time slicing | Shared execution time; less predictable under contention |
| Container | Shares host kernel; needs compatible host driver/runtime path |

## Common Wrong-Answer Patterns

- Add GPUs before identifying CPU, storage, network, or scheduler bottlenecks.
- Treat GPU memory allocated as GPU compute utilized.
- Use an inference optimizer as a cluster monitor or scheduler.
- Use NVLink as a general replacement for a scale-out network.
- Assume a container includes and controls the physical host driver.
- Select passthrough when multiple VMs must share a GPU.
- Select time slicing when predictable hardware isolation is required.
- Design racks before validating power and cooling.
- Choose cloud or on-premises from price alone without utilization and data movement.

## Final Mental Model

For every scenario, identify:

1. **Goal:** quality, latency, throughput, utilization, isolation, or availability?
2. **Scope:** application, model, job, GPU, node, fabric, storage, or facility?
3. **Evidence:** which metric proves the bottleneck?
4. **Technology:** which component operates at that layer?
5. **Tradeoff:** what gets worse when this choice gets better?

