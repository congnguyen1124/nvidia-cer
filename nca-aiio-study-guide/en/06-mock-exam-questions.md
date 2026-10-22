# NCA-AIIO 50-Question Mock Exam

## Instructions

- **Time limit:** 60 minutes.
- **Questions:** 50.
- **Domain distribution:** 19 Essential AI Knowledge, 20 AI Infrastructure, 11 AI Operations.
- Questions are interleaved to simulate context switching.
- Unless a question explicitly says **Choose TWO**, select one answer.
- For a **Choose TWO** question, both selections must be correct to earn the point.
- Do not use notes. Record answers on the sheet below and open the answer key only after the timer expires.

This is an original practice exam aligned to the public blueprint. It is not an official NVIDIA exam and does not reproduce live exam content.

## Answer Sheet

```text
 1.__  2.__  3.__  4.__  5.__  6.__  7.__  8.__  9.__ 10.__
11.__ 12.__ 13.__ 14.__ 15.__ 16.__ 17.__ 18.__ 19.__ 20.__
21.__ 22.__ 23.__ 24.__ 25.__ 26.__ 27.__ 28.__ 29.__ 30.__
31.__ 32.__ 33.__ 34.__ 35.__ 36.__ 37.__ 38.__ 39.__ 40.__
41.__ 42.__ 43.__ 44.__ 45.__ 46.__ 47.__ 48.__ 49.__ 50.__
```

---

### 1. Which statement correctly describes the relationship among AI, machine learning, and deep learning?

A. AI is a subset of machine learning, which is a subset of deep learning.  
B. Deep learning is a subset of machine learning, which is a subset of AI.  
C. Machine learning and deep learning are unrelated branches outside AI.  
D. AI always requires a deep neural network.

### 2. An engineer estimates training memory by multiplying model parameters by the bytes used for each weight. Why is this estimate incomplete?

A. GPU memory stores only input data, not model weights.  
B. The estimate applies only to inference because training uses CPU memory.  
C. Training may also store gradients, optimizer state, activations, and workspace buffers.  
D. Parameter count determines network bandwidth but not memory use.

### 3. A scheduler dashboard shows that every GPU is allocated, but DCGM reports little GPU activity and users see low throughput. What is the best conclusion?

A. The cluster is fully and efficiently utilized.  
B. DCGM cannot monitor a scheduled workload.  
C. Allocation alone does not prove useful utilization; inspect the jobs and their data paths.  
D. GPU memory should be cleared on every node immediately.

### 4. A team has a trained model and wants to compile and optimize it for lower-latency NVIDIA GPU inference. Which technology most directly addresses this need?

A. DCGM  
B. Base Command Manager  
C. TensorRT  
D. GPUDirect Storage

### 5. A cluster architect adds more GPU servers connected by a high-speed fabric. Which scaling method is being used?

A. Scale-down  
B. Scale-out  
C. Scale-in  
D. Scale-up only

### 6. Which objective is most directly associated with an interactive online inference service?

A. Minimize time to train across many epochs.  
B. Maintain low tail latency while meeting request throughput.  
C. Maximize optimizer-state memory.  
D. Increase checkpoint size.

### 7. Training scales efficiently from one to four nodes but poorly from four to sixteen nodes. Which action should be performed first?

A. Add another sixteen nodes.  
B. Replace the model with a rule-based system.  
C. Measure collective communication, topology, storage throughput, and worker imbalance.  
D. Disable monitoring to remove all overhead.

### 8. A developer wants a curated source of GPU-optimized containers, pretrained models, and Helm charts. Which service should the developer use?

A. NVML  
B. NGC Catalog  
C. Redfish  
D. NVSwitch

### 9. What is the primary role of DCGM Exporter?

A. Compile neural networks into optimized inference engines.  
B. Schedule distributed jobs across GPU nodes.  
C. Expose selected DCGM telemetry in a format Prometheus can scrape.  
D. Update BMC firmware on all servers.

### 10. The host operating system on a compute node is unresponsive. Which path is designed to provide independent recovery access?

A. Model-serving network only  
B. Out-of-band management through the BMC  
C. Training data loader  
D. CUDA kernel stream

### 11. Which NVIDIA component automates the lifecycle of drivers, the container toolkit, device plugins, feature discovery, and monitoring components in a Kubernetes GPU cluster?

A. TensorRT  
B. NVIDIA GPU Operator  
C. Triton Inference Server  
D. NCCL

### 12. A data center has an 800 kW IT load and operates at a PUE of 1.25. Approximately what total facility power corresponds to that IT load?

A. 640 kW  
B. 800 kW  
C. 1,000 kW  
D. 1,250 kW

### 13. A platform team needs to serve models from multiple supported frameworks and use capabilities such as dynamic batching and concurrent model execution. Which component is the best fit?

A. Triton Inference Server  
B. DCGM Exporter  
C. Baseboard Management Controller  
D. NVLink

### 14. A distributed training job needs eight GPUs at the same time. The scheduler currently assigns two GPUs that remain idle while the job waits for six more. Which scheduling capability best prevents this waste?

A. Time slicing  
B. Gang scheduling  
C. Round-robin DNS  
D. Thermal throttling

### 15. Which technology provides a supported, more direct data path between storage and GPU memory to reduce CPU and extra-copy overhead?

A. GPUDirect Storage  
B. SNMP  
C. vGPU  
D. Redfish

### 16. Which comparison between CPUs and GPUs is most accurate?

A. CPUs have more simple arithmetic units and are always faster for tensor operations.  
B. CPUs emphasize low-latency general-purpose control; GPUs emphasize throughput for highly parallel work.  
C. GPUs replace CPUs for operating-system and orchestration tasks.  
D. CPUs and GPUs have identical execution architectures.

### 17. An operator suspects that a GPU is being thermally throttled. Which TWO observations most directly support the hypothesis?

A. The model file occupies GPU memory.  
B. GPU temperature is near its thermal limit.  
C. The job has a high scheduling priority.  
D. The BMC account uses multifactor authentication.  
E. Clock frequency drops while a thermal-throttle reason is active.

### 18. A regulated organization has steady, highly utilized AI workloads, an existing capable facility, and a strict requirement to keep sensitive data under direct local control. Which placement is most likely to fit?

A. On-premises infrastructure  
B. Unmanaged public endpoints  
C. Edge devices only, regardless of workload size  
D. A different cloud region every day

### 19. Which NVIDIA platform is most directly associated with speech recognition, translation, and text-to-speech applications?

A. NVIDIA Riva  
B. NVIDIA DCGM  
C. NVIDIA Base Command Manager  
D. NVIDIA NVSwitch

### 20. After a cluster grows, step time increases and GPUs frequently wait during both gradient synchronization and dataset reads. Which TWO areas should be investigated first?

A. Compute-fabric topology and collective communication  
B. The color of the server bezels  
C. The number of user documentation pages  
D. Storage/data-pipeline throughput  
E. The digital-badge expiration date

### 21. A team wants a packaged, optimized model inference service exposed through a standard API rather than assembling the serving stack from individual components. What is the best match?

A. NVIDIA NIM microservice  
B. NVML  
C. UEFI  
D. SNMP trap

### 22. A research environment primarily runs long, queued, multi-node distributed training jobs and needs mature priorities, partitions, reservations, and backfill. Which workload manager is the most natural fit?

A. Grafana  
B. Slurm  
C. TensorRT  
D. Redfish

### 23. Which statement about training and inference is correct?

A. Inference normally updates model weights with backpropagation after every request.  
B. Training never benefits from multiple GPUs.  
C. Training learns weights; inference applies learned weights to new inputs.  
D. Inference always requires more memory than training.

### 24. Which benefit most directly describes the role of a BlueField DPU in a data center?

A. Replacing all GPUs for neural-network matrix operations  
B. Offloading, accelerating, and isolating networking, storage, and security services  
C. Providing a public object-storage bucket  
D. Acting only as a passive Ethernet cable

### 25. GPU utilization remains high, but application throughput falls as temperature rises. Clocks are below their normal loaded values. What is the most likely explanation?

A. The model has become supervised learning.  
B. Thermal or power throttling is reducing effective performance.  
C. High allocation always guarantees maximum speed.  
D. NGC has deleted the container.

### 26. Which TWO records are most important for reproducing a model-training run?

A. Dataset and model/code versions  
B. The office seating plan  
C. The monitor's screen brightness  
D. Hyperparameters plus framework/container/library versions  
E. The name of the person who first opened the dashboard

### 27. What does network oversubscription mean?

A. Every link has more capacity than all endpoints can use.  
B. Potential traffic from downstream endpoints exceeds the available uplink capacity.  
C. All GPU memory has been allocated.  
D. The facility has two utility feeds.

### 28. Which statement best defines deep learning?

A. A method that never uses data  
B. Any manually written rule-based application  
C. A subset of machine learning based on multi-layer neural networks  
D. A data center cooling technique

### 29. A training dataset contains millions of very small files. Aggregate disk capacity is sufficient, but workers spend excessive time opening and locating files. Which storage characteristics deserve the most attention?

A. GPU Tensor Core count only  
B. Metadata performance and IOPS  
C. Monitor resolution  
D. PUE only

### 30. Which statement best describes the NVIDIA GPU Operator?

A. It replaces Kubernetes with a proprietary scheduler.  
B. It optimizes a trained model into a TensorRT engine.  
C. It automates deployment and lifecycle management of the GPU software stack on Kubernetes nodes.  
D. It is a physical GPU-to-GPU link.

### 31. Which library is designed for high-performance collective communication, such as all-reduce, among NVIDIA GPUs?

A. cuDNN  
B. NCCL  
C. Redfish  
D. DOCA Flow only

### 32. A startup needs 64 GPUs for a three-week experiment, has no suitable data center, and is uncertain about demand after the experiment. Which option is most aligned with these constraints?

A. Build a new private facility before testing.  
B. Use elastic cloud GPU capacity, subject to quota and availability checks.  
C. Buy capacity for ten years regardless of utilization.  
D. Run the workload only on BMC processors.

### 33. A cluster has eight free GPUs in total, but a four-GPU job cannot start because no eligible node or topology domain has four matching GPUs. What is this condition called?

A. Quantization  
B. Fragmentation  
C. Backpropagation  
D. Data labeling

### 34. Which workflow correctly combines inference optimization and serving?

A. Use Grafana to compile the model, then Redfish to serve it.  
B. Use DCGM to train the model, then the BMC to batch requests.  
C. Optimize the model with TensorRT and serve it through Triton Inference Server.  
D. Use NVLink to version the dataset and PUE to deploy it.

### 35. A server gains additional tightly interconnected GPUs through a supported NVLink/NVSwitch domain. Which term best describes this change?

A. Scale-up  
B. Scale-out across Ethernet only  
C. Out-of-band management  
D. Cloud bursting

### 36. Which NVIDIA technology family is most directly associated with intelligent video analytics and smart spaces?

A. NVIDIA Metropolis  
B. NVIDIA DCGM  
C. NVIDIA vGPU Manager  
D. NVIDIA Base Command Manager

### 37. A new accelerated rack has much higher density than the existing racks. Which TWO items must be validated before installation?

A. Whether the certification badge can be printed  
B. Electrical distribution and rack-level power capacity  
C. Whether every user prefers the same text editor  
D. Cooling capacity, airflow/liquid requirements, and environmental limits  
E. The color theme of the monitoring dashboard

### 38. Several independent virtual machines need managed fractional access to one supported physical GPU. Which technology is designed for this use case?

A. GPU passthrough of the same full GPU to every VM simultaneously  
B. NVIDIA vGPU with supported profiles and licensing  
C. A BMC serial console  
D. GPUDirect Storage only

### 39. A deployed model's input distribution changes over time and quality declines even though infrastructure health is normal. Which lifecycle practice addresses this risk?

A. Monitor data/model behavior and trigger governed retraining or rollback.  
B. Increase rack power without collecting model metrics.  
C. Disable versioning to simplify operations.  
D. Replace the out-of-band switch.

### 40. Which technology provides RDMA semantics over an appropriately engineered Ethernet network?

A. RoCE  
B. SATA  
C. Redfish  
D. MIG

### 41. Which tool or interface remains most relevant for remote power control and console access when the host OS cannot boot?

A. TensorRT runtime  
B. BMC through the out-of-band management network  
C. Model registry  
D. Training optimizer state

### 42. Which combination best explains the recent acceleration of AI capability and adoption?

A. Less data, slower networks, and removal of software libraries  
B. Rule-based systems alone and declining compute capacity  
C. Only larger office buildings  
D. More data, accelerated computing, algorithmic advances, high-speed interconnects, and optimized software

### 43. Which statement about liquid cooling for dense AI systems is most accurate?

A. It eliminates all facility planning.  
B. It can support high heat density but requires coolant infrastructure, leak detection, and service planning.  
C. It reduces every GPU's memory requirement.  
D. It is an inference compiler.

### 44. One virtual machine requires exclusive access to an entire physical GPU and does not need to share it with other VMs. Which assignment method is the best fit?

A. GPU passthrough  
B. Prometheus scraping  
C. Time slicing among many VMs  
D. Dynamic batching

### 45. Why might a large AI cluster separate compute-fabric and storage-fabric traffic?

A. To ensure heavy storage I/O does not unpredictably disrupt latency-sensitive training collectives  
B. To make every cable the same length regardless of topology  
C. To remove the need for switches  
D. To make all storage local and ephemeral

### 46. A CUDA application and its user-space libraries are packaged in a container. What is still required on the host?

A. No GPU-related software of any kind  
B. A compatible NVIDIA driver and a runtime path that exposes the GPU to the container  
C. A separate physical GPU for every container layer  
D. TensorRT even if the application never performs inference

### 47. A job takes 100 minutes on one GPU and 30 minutes on four GPUs. Approximately what is its four-GPU scaling efficiency?

A. 30%  
B. 75%  
C. 83%  
D. 120%

### 48. Host CPUs are heavily loaded by software-defined networking, storage, and security functions, reducing cycles available to AI applications. Which addition most directly targets the problem?

A. A larger monitor  
B. A second model registry  
C. More office network printers  
D. A BlueField DPU with supported DOCA-based offloads

### 49. GPUs repeatedly alternate between short bursts of activity and long idle gaps during training. Which TWO causes should be examined before adding GPUs?

A. CPU preprocessing/data-loader performance  
B. The font used in the training log  
C. The certification exam price  
D. Storage throughput and data availability  
E. The color of the rack doors

### 50. An enterprise keeps sensitive, steady-state training on-premises but temporarily uses cloud GPUs for burst experiments. What deployment model is this?

A. Scale-up only  
B. Out-of-band management  
C. GPU passthrough  
D. Hybrid infrastructure

---

Stop the timer. Record your elapsed time, then grade with [07-mock-exam-answer-key.md](./07-mock-exam-answer-key.md).

