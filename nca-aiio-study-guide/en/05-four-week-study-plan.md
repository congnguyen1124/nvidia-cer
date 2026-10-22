# Four-Week NCA-AIIO Study Plan

This plan assumes **60–90 minutes on weekdays** and **2–3 hours on weekend days**. Compress it by combining adjacent days, but preserve retrieval practice and the timed mock exam.

## Daily Session Template

1. **10 minutes:** recall yesterday's concepts without notes.
2. **35–50 minutes:** read and annotate the day's section.
3. **15 minutes:** explain the topic aloud as if teaching a colleague.
4. **10 minutes:** update flashcards and the error log.

## Week 1 — Essential AI Knowledge

### Day 1: Blueprint and Foundations

- Read [README.md](./README.md).
- Read Chapter 1 sections 1.1–1.2.
- Be able to draw the AI → ML → DL hierarchy and name three drivers of AI adoption.

### Day 2: Training and Inference

- Read section 1.3.
- Build a comparison table from memory.
- Practice identifying whether a scenario prioritizes latency, throughput, time to train, or convergence.

### Day 3: CPU and GPU Architecture

- Read section 1.4.
- Explain CUDA cores, Tensor Cores, HBM, PCIe, NVLink, and NVSwitch at associate depth.
- Write one example of a CPU bottleneck that leaves a GPU idle.

### Day 4: NVIDIA Software Stack

- Read section 1.5.
- Make flashcards with `problem` on the front and `technology` on the back.
- Focus on TensorRT versus Triton, NGC versus AI Enterprise, and DCGM versus scheduler.

### Day 5: Lifecycle

- Read section 1.6.
- Reconstruct the lifecycle without notes.
- List the artifacts required for reproducibility.

### Days 6–7: Solutions and Use Cases

- Read sections 1.7–1.10.
- Match products to use cases.
- Review Chapter 1 until every checklist item can be answered in one or two sentences.

## Week 2 — AI Infrastructure

### Day 8: Workload Sizing

- Read sections 2.1–2.2.
- Explain why training memory is more than weights.
- Calculate speedup and scaling efficiency for two made-up examples.

### Day 9: Cluster Anatomy

- Read section 2.3.
- Draw compute, storage, in-band, and out-of-band planes.
- Explain what remains accessible if a host OS crashes.

### Day 10: Storage

- Read section 2.4.
- Distinguish capacity, throughput, IOPS, metadata, and latency problems.
- Explain GPUDirect RDMA versus GPUDirect Storage.

### Days 11–12: Networking

- Read section 2.5.
- Compare NVLink/NVSwitch, InfiniBand, Ethernet/RoCE, PCIe, RDMA, and NCCL.
- Explain oversubscription, leaf-spine, rail optimization, and collective traffic.

### Day 13: Deployment Location

- Read section 2.6.
- Create three scenarios favoring on-premises, cloud, and hybrid.
- Include data gravity and total cost, not only hardware price.

### Day 14: Facilities and DPU

- Read sections 2.7–2.10.
- Practice PUE calculations.
- Explain the chain from rack power density to cooling, floor layout, cable length, and topology.
- Explain BlueField versus DOCA and DPU versus GPU.

## Week 3 — AI Operations

### Day 15: Management Planes

- Read sections 3.1–3.2.
- Compare in-band and out-of-band access.
- Explain BCM, BMC, Redfish, and why version compatibility matters.

### Days 16–17: Monitoring

- Read section 3.3.
- Build a metric map for activity, capacity, limits, reliability, and application outcome.
- Explain the DCGM → DCGM Exporter → Prometheus → Grafana path.

### Day 18: Troubleshooting

- Read section 3.4.
- Work each symptom in the table from evidence to hypothesis.
- Practice resisting the “add more GPUs” answer.

### Days 19–20: Orchestration and Scheduling

- Read section 3.5.
- Compare Kubernetes and Slurm.
- Create examples of gang scheduling, backfill, fragmentation, preemption, and topology-aware placement.

### Day 21: Virtualization

- Read sections 3.6–3.8.
- Make a decision table for passthrough, vGPU, MIG, time slicing, containers, and bare metal.
- Recite the compatibility stack that must be validated.

## Week 4 — Integration and Exam Practice

### Day 22: First Cram-Sheet Pass

- Read [04-exam-cram-sheet.md](./04-exam-cram-sheet.md).
- Mark each line green, yellow, or red.
- Return to the chapters for every red item.

### Day 23: Architecture Scenarios

For each scenario, write the bottleneck, evidence, and likely remedy:

- Large distributed training with poor scaling.
- Online inference with high p99 latency.
- High GPU allocation but low useful activity.
- New racks beyond cooling capacity.
- Multiple small isolated tenants wasting full GPUs.

### Day 24: Product Matching

- Shuffle all NVIDIA product flashcards.
- Answer both directions: product → purpose and requirement → product.
- Review confusing pairs.

### Day 25: Closed-Book Recall

- Recreate the cram sheet from memory on one blank page.
- Check it against the original.
- Study only the missing or incorrect concepts.

### Day 26: Timed Mock Exam

- Take [06-mock-exam-questions.md](./06-mock-exam-questions.md) closed-book.
- Set a strict 60-minute timer.
- Do not open the answer key until time expires.

### Day 27: Error Analysis

- Use [07-mock-exam-answer-key.md](./07-mock-exam-answer-key.md).
- Record why every miss happened: knowledge gap, misread constraint, wrong layer, or changed answer.
- Re-read the mapped objective for every miss.

### Day 28: Final Review

- Re-answer only missed questions without options.
- Review the cram sheet and official blueprint.
- Stop heavy study early enough to sleep well.

## Readiness Targets

- Overall timed mock score: **80%+**.
- Each domain: **75%+**.
- No uncertainty about core distinctions: training/inference, CPU/GPU, TensorRT/Triton, NVLink/scale-out network, MIG/vGPU/passthrough, DCGM/scheduler.
- Able to finish the mock with at least five minutes for flagged questions.

