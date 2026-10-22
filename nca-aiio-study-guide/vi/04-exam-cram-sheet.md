# NCA-AIIO Exam Cram Sheet — Tiếng Việt

Dùng file này trong 24–48 giờ cuối. Nếu một dòng chưa rõ, quay lại chương tương ứng thay vì học thuộc máy móc.

## Blueprint

| Domain | Trọng số | Số câu mock exam |
|---|---:|---:|
| Essential AI Knowledge | 38% | 19 |
| AI Infrastructure | 40% | 20 |
| AI Operations | 22% | 11 |

## Essential AI Knowledge

### Quan hệ khái niệm

```text
AI ⊃ Machine Learning ⊃ Deep Learning
```

- AI có thể rule-based; không phải lúc nào cũng học.
- ML học pattern từ data.
- DL dùng multi-layer neural network.
- Generative AI tạo content mới; không phải toàn bộ AI.

### Training và Inference

| Training | Inference |
|---|---|
| Học/cập nhật weight | Áp dụng weight đã học |
| Forward + backward + optimizer | Chủ yếu forward/token generation |
| Time to train, convergence, scaling | Latency, throughput, cost, availability |
| Dataset lớn, checkpoint | Request, batching, cache |
| Inter-GPU communication cao khi scale | Tối ưu theo service SLO |

- Batch lớn thường tăng throughput nhưng có thể làm request chờ lâu hơn.
- Training memory gồm weights, gradients, optimizer state, activations và workspace.

### CPU và GPU

- CPU: ít core phức tạp hơn, cache/control logic lớn, tốt cho serial/branch-heavy work.
- GPU: nhiều parallel unit, throughput và memory bandwidth cao, tốt cho matrix/tensor.
- CPU, storage hoặc I/O chậm có thể làm GPU khỏe vẫn idle.

### Ghép Product/Technology

| Nhu cầu | Từ khóa đáp án |
|---|---|
| Parallel GPU programming | CUDA |
| DL primitives | cuDNN |
| Multi-GPU collectives | NCCL |
| Optimize/compile inference | TensorRT |
| Serve model nhiều framework | Triton Inference Server |
| Packaged optimized model API | NIM microservice |
| Generative AI customization | NeMo |
| Speech AI | Riva |
| Healthcare imaging | MONAI |
| Video analytics | Metropolis / DeepStream |
| Robotics | Isaac |
| Digital twin | Omniverse |
| Edge AI | Jetson |
| Autonomous vehicle | DRIVE |
| Container/model/Helm chart | NGC |
| Enterprise software/support | NVIDIA AI Enterprise |
| Kubernetes GPU lifecycle | GPU Operator |
| GPU telemetry/health | DCGM |
| Cluster provisioning/admin | Base Command Manager |
| DPU programming | DOCA |

**Bẫy:** TensorRT tối ưu inference; Triton serve model. Chúng có thể dùng cùng nhau.

## AI Infrastructure

### Scale

- Scale-up: thêm/tăng resource trong một system hoặc NVLink/NVSwitch domain.
- Scale-out: thêm node qua InfiniBand hoặc high-speed Ethernet.
- Scaling loss đến từ communication, synchronization, imbalance, serial work và I/O.

```text
Speedup = T1 / Tn
Efficiency = Speedup / n
```

### Data Path

| Path | Mục đích |
|---|---|
| NVLink/NVSwitch | High-bandwidth GPU communication cục bộ/tightly coupled |
| InfiniBand | Low-latency RDMA-capable scale-out fabric |
| Ethernet/RoCE | Ethernet ecosystem với RDMA cho engineered AI fabric |
| PCIe | Host/device connectivity |
| GPUDirect RDMA | Supported NIC-to-GPU path, giảm CPU/copy |
| GPUDirect Storage | Supported storage-to-GPU path, giảm overhead |

- RDMA giảm CPU involvement/copy cho remote memory access.
- Oversubscription: potential downstream traffic lớn hơn uplink capacity.
- NCCL là collective library, không phải physical network.

### Network Plane

- Compute fabric: distributed workload communication.
- Storage fabric: dataset và checkpoint I/O.
- In-band: provisioning, user, service và general data.
- OOB: BMC/device recovery độc lập host OS.

### Storage

- Large streaming data → throughput.
- Hàng triệu small file → IOPS và metadata.
- Checkpoint stall → aggregate write path.
- Local NVMe → cache/scratch nhanh, thường ít durable.
- Shared parallel storage → concurrent worker và durable checkpoint.

### Placement

- On-premises: control, steady high utilization, data locality; cần CapEx, staff, lifecycle và facility.
- Cloud: nhanh, elastic; chú ý recurring cost, quota, egress và governance.
- Hybrid: chọn môi trường theo workload; tăng integration complexity.
- Train gần data nếu data movement là constraint chính.

### Power và Cooling

```text
Energy (kWh) = Power (kW) × Time (h)
PUE = Total facility energy / IT equipment energy
Facility estimate ≈ IT load × PUE
```

- Gần như toàn bộ electrical power của IT trở thành heat.
- Không thiết kế circuit chỉ dựa vào observed average power.
- Rack density ảnh hưởng power, cooling, floor, cable và topology.
- Liquid cooling hỗ trợ density cao nhưng cần coolant, leak detection và service planning.

### DPU

- BlueField DPU offload, accelerate và isolate network, storage, security, virtualization service.
- Giải phóng host CPU cho application.
- DOCA là software framework.
- DPU không thay GPU hoặc general CPU.

## AI Operations

### Tool Chain

| Tool | Vai trò |
|---|---|
| `nvidia-smi` | Local triage |
| NVML | GPU management API/library |
| DCGM | Fleet telemetry, health, diagnostics, accounting |
| DCGM Exporter | Đưa DCGM field thành Prometheus format |
| Prometheus | Scrape/lưu time series |
| Grafana | Dashboard |
| BMC/Redfish | Hardware management độc lập OS |

### GPU Signal

- Work: SM/GPU activity, tensor activity, memory bandwidth.
- Capacity: memory used/total; allocation không phải active work.
- Limit: temperature, power, clock, throttle reason.
- Health: ECC, Xid, remapped page, PCIe/NVLink error.
- Outcome: app throughput/latency, step time, job failure.

**Bẫy:** 100% allocated không đồng nghĩa 100% useful utilization.

### Scheduling

- Gang scheduling: cấp đủ resource cho distributed job cùng lúc.
- Bin packing: xếp job dày để giữ contiguous free block.
- Fragmentation: tổng resource đủ nhưng sai shape/location/type.
- Backfill: chạy job nhỏ trong gap mà không làm trễ reserved job.
- Preemption: thu hồi resource từ lower-priority work.
- Topology-aware placement: đặt task theo connection thuận lợi.
- Slurm: queued HPC/large training.
- Kubernetes: containerized service/cloud-native workload.

### Virtualization

| Mode | Dấu hiệu nhận biết |
|---|---|
| Passthrough | Toàn bộ physical GPU, exclusive cho một VM |
| vGPU | Managed GPU sharing giữa VM; profile/licensing |
| MIG | Hardware-isolated spatial GPU instance |
| Time slicing | Chia execution time; predictability thấp hơn khi contention |
| Container | Chia sẻ host kernel; cần compatible host driver/runtime |

## Bẫy đáp án phổ biến

- Thêm GPU trước khi xác định CPU, storage, network hoặc scheduler bottleneck.
- Nhầm GPU memory allocated với GPU compute utilized.
- Dùng inference optimizer như cluster monitor/scheduler.
- Xem NVLink là giải pháp thay mọi scale-out network.
- Cho rằng container tự mang và điều khiển physical host driver.
- Chọn passthrough khi nhiều VM phải share một GPU.
- Chọn time slicing khi cần predictable hardware isolation.
- Thiết kế rack trước khi xác nhận power/cooling.
- Chọn cloud/on-prem chỉ dựa vào giá compute mà bỏ qua utilization và data movement.

## Mental Model khi gặp Scenario

1. **Goal:** quality, latency, throughput, utilization, isolation hay availability?
2. **Scope:** application, model, job, GPU, node, fabric, storage hay facility?
3. **Evidence:** metric nào chứng minh bottleneck?
4. **Technology:** component nào hoạt động đúng layer?
5. **Tradeoff:** điều gì xấu đi khi lựa chọn này tốt lên?

