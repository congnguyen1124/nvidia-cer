# Chương 3 — AI Operations

**Trọng số chính thức: 22%**  
**Phân bổ trong mock exam: 11/50 câu**

## Mục tiêu

Sau chương này, bạn cần có thể:

1. Mô tả AI data center management và monitoring essentials.
2. Giải thích cluster orchestration và job scheduling.
3. Chọn và diễn giải GPU health/performance metric quan trọng.
4. So sánh các lựa chọn virtualization cho accelerated infrastructure.

---

## 3.1 Vận hành toàn bộ Service, không chỉ GPU

AI operations có nhiều layer:

```text
Business/application outcome
  Quality, correctness, user experience, cost

Model service
  Request, latency, throughput, error, queue depth, batch size

Job/orchestration
  Queue wait, placement, failure, retry, utilization, fairness

Accelerated system
  GPU/CPU/memory, NVLink/PCIe, NIC, local disk, driver

Cluster infrastructure
  Compute fabric, storage, power, cooling, management service

Facility
  Utility power, UPS/PDU, temperature, humidity, leak, physical security
```

Dashboard GPU màu xanh không chứng minh AI service hoạt động tốt. Phải correlate infrastructure telemetry với scheduler, application và model-service metric.

### Four Signals

- **Latency:** công việc mất bao lâu.
- **Traffic:** có bao nhiêu work/request.
- **Errors:** lượng công việc thất bại hoặc sai.
- **Saturation:** resource gần giới hạn đến mức nào.

Với batch cluster, thêm queue wait, completion time, throughput và scheduling fairness.

---

## 3.2 Management Plane và Access

### In-Band Management

Dùng production/service network và host OS. Tiện cho quản trị thường ngày nhưng có thể mất khi OS hoặc production network lỗi.

### Out-of-Band — OOB Management

Dùng management path riêng, thường thông qua **Baseboard Management Controller — BMC**.

Khả năng thường có:

- Power on/off/reset và remote console.
- Hardware inventory và sensor.
- Firmware management và hardware event log.
- Recovery access khi host OS không hoạt động.

OOB network có quyền rất cao nên phải isolate, giới hạn access, dùng strong authentication, audit log và patch firmware.

### Redfish và SNMP

- **Redfish:** RESTful management interface dựa trên standard cho server/infrastructure hiện đại.
- **SNMP:** protocol phổ biến để monitor device và gửi alert; cần secure version và access control.

### NVIDIA Base Command Manager — BCM

BCM hỗ trợ cluster installation, provisioning, administration, inventory, monitoring, software/firmware management, networking và workload-manager integration. Vai trò rộng hơn GPU telemetry.

### Configuration và Change Control

Cần quản lý known-good configuration cho:

- Firmware, BIOS, BMC và switch software.
- OS image và kernel.
- Driver, CUDA, library, container toolkit và framework.
- Scheduler/orchestrator, plugin/operator.
- Network, storage, security và monitoring policy.

Rollout theo giai đoạn, validate compatibility, có rollback path và ghi lại thay đổi. Phiên bản “latest” không đồng nghĩa “compatible”.

---

## 3.3 NVIDIA GPU Management và Monitoring

### Tool và vai trò

| Tool | Vai trò chính |
|---|---|
| `nvidia-smi` | Local inventory, status, process và triage nhanh |
| NVML | Programmatic GPU management/monitoring library |
| DCGM | Fleet GPU telemetry, health, diagnostics, policy, accounting, NVSwitch monitoring |
| DCGM Exporter | Xuất DCGM metric theo Prometheus format |
| Prometheus | Scrape và lưu time-series metric |
| Grafana | Dashboard/visualization |
| BMC/Redfish | Hardware management độc lập host OS |

`nvidia-smi` hữu ích để kiểm tra nhanh một node, nhưng chạy thủ công trên hàng trăm node không phải monitoring architecture.

### GPU metric quan trọng

#### Utilization

- **GPU/SM utilization:** trong sample interval, kernel hoạt động bao nhiêu phần trăm thời gian.
- **Tensor activity:** Tensor Core activity khi metric hỗ trợ.
- **Memory-bandwidth activity:** áp lực lên GPU memory transfer.
- **Encoder/decoder utilization:** quan trọng với video pipeline.

High utilization không luôn nghĩa efficient; kernel chậm vẫn có thể làm GPU busy. Low utilization cũng không luôn xấu; latency service có thể chủ động giữ headroom.

#### Memory

- Used và total GPU memory.
- Memory-bandwidth utilization.
- Allocation failure hoặc OOM.
- ECC/memory error indicator.

Allocated memory không giống active compute. Model có thể chiếm gần hết memory nhưng không xử lý request nào.

#### Thermal, Power và Clock

- GPU temperature và thermal limit.
- Power draw và enforced power limit.
- SM/graphics và memory clock.
- Throttle reason.

Khi temperature hoặc power limit gây clock throttling, performance giảm dù utilization vẫn cao.

#### Reliability và Interconnect

- Correctable và uncorrectable ECC error.
- Xid/system error và health state.
- Retired/remapped memory page khi được hỗ trợ.
- PCIe replay/error và link characteristic.
- NVLink/NVSwitch link state, throughput và error.

Uncorrectable error hoặc fault lặp lại có thể yêu cầu drain node và hardware investigation. Cần correlate time, job, node, topology và event log.

### Metric theo workload

| Workload | Outcome metric | Supporting infrastructure metric |
|---|---|---|
| Distributed training | Time/step, samples/sec, scaling efficiency, loss | GPU activity, collective time, network, storage, throttle |
| Online inference | p50/p95/p99 latency, request/sec, error, cost/request | Queue, batch, GPU memory/activity, power, network |
| Batch inference | Items/sec, completion time, cost/item | GPU activity, I/O, scheduler wait |
| Shared cluster | Useful utilization, queue wait, success, fairness | Fragmentation, idle reservation, preemption, failed node |

### Monitoring design tốt

- Có healthy baseline.
- Time synchronization giữa system.
- Label theo node, GPU, job, tenant, model và topology.
- Sampling và retention phù hợp.
- Alert dựa trên symptom có thể hành động và sustained condition.
- Runbook có owner, impact, validation, remediation và escalation.
- Capacity/trend report ngoài incident alert.

Không alert dựa vào một spike tức thời không có context. Xem threshold, duration, rate of change và correlated signal.

---

## 3.4 Phương pháp Troubleshooting

1. **Xác định symptom:** training chậm, tail latency cao, job fail, OOM, low utilization hay hardware error.
2. **Xác định scope:** một GPU, node, rack, job, model hay cả cluster.
3. **Kiểm tra recent change:** software, firmware, topology, data, workload, power hoặc environment.
4. **Theo dependency path:** application → scheduler → GPU/CPU → interconnect → storage/network → facility.
5. **So sánh:** healthy peer, previous baseline hoặc known-good job.
6. **Test một hypothesis an toàn:** không thay nhiều biến cùng lúc.
7. **Validate recovery và lưu bài học:** cập nhật runbook/alert.

### Symptom map

| Symptom | Evidence nên xem trước |
|---|---|
| Low GPU utilization | CPU/preprocessing, data loader, storage, queue, batch, synchronization |
| High utilization, low app throughput | Kernel efficiency, precision, memory bandwidth, clock/throttle, communication |
| Một worker làm chậm job | Per-GPU/node metric, link error, thermal/power, storage, placement |
| OOM | Model/batch/sequence size, fragmentation, competing process, leak |
| Tail latency spike | Queue, batching delay, contention, cold start, throttle, network |
| Job liên tục fail trên một node | DCGM diagnostics, Xid/ECC/link event, system log; drain node |

---

## 3.5 Container, Orchestration và Scheduling

### Container

Container đóng gói application và user-space dependency nhưng chia sẻ host kernel. Container tăng portability/reproducibility nhưng không phải hardware VM. CUDA container vẫn cần compatible host driver và runtime path để expose GPU.

NVIDIA Container Toolkit cho phép supported container runtime truy cập NVIDIA GPU.

### Kubernetes

Kubernetes phù hợp với containerized, service-oriented và cloud-native workload.

- **Pod:** deployable unit nhỏ nhất.
- **Node:** worker machine chạy pod.
- **Request/limit:** resource declaration cho placement/control.
- **Device plugin/DRA driver:** advertise và allocate specialized device như GPU.
- **Label, taint, toleration, affinity:** ảnh hưởng eligible node và placement.
- **DaemonSet:** chạy pod trên các node được chọn, phù hợp node-level agent/exporter.
- **Operator:** tự động lifecycle của complex software bằng controller và custom resource.

### NVIDIA GPU Operator

GPU Operator tự động cài đặt và quản lý lifecycle của software cần để dùng GPU trong Kubernetes: driver, container toolkit, device plugin, feature discovery, MIG management và DCGM-based monitoring tùy cấu hình.

GPU Operator không thay Kubernetes và không đảm bảo application code efficient.

### Slurm

Slurm là workload manager phổ biến cho HPC và large distributed training.

- Job và job step.
- Partition/queue.
- Resource request/allocation.
- Priority, reservation, dependency và limit.
- Backfill scheduling.
- Accounting và fair share.

Slurm phù hợp queued, long-running, tightly coupled batch job. Kubernetes phù hợp containerized application/service. Một tổ chức có thể dùng cả hai.

### Scheduling goal

- Useful utilization cao.
- Fairness và quota giữa team.
- Priority và deadline.
- Queue time ngắn.
- Data/topology locality.
- Isolation và policy compliance.
- Reliability và maintenance.

### Scheduling concept

- **Gang scheduling:** cấp toàn bộ resource cho distributed job cùng lúc, tránh partial allocation không chạy được.
- **Bin packing:** xếp workload dày để giữ block resource lớn còn trống.
- **Fragmentation:** tổng capacity đủ nhưng không đúng shape/type/topology.
- **Backfill:** chạy job nhỏ trong khoảng trống mà không làm trễ reserved high-priority job.
- **Preemption:** thu hồi resource từ lower-priority work; checkpoint giảm lost work.
- **Topology-aware placement:** đặt task giao tiếp nhiều trên path thuận lợi.
- **Quota/fair share:** kiểm soát quyền dùng resource theo thời gian.

**Allocated không đồng nghĩa useful.** GPU có thể được scheduler cấp nhưng job bị input-bound, sleep, fail hoặc reserve memory mà không compute.

---

## 3.6 Virtualization cho Accelerated Infrastructure

### Container và Virtual Machine

| Thuộc tính | Container | Virtual machine |
|---|---|---|
| Kernel | Chia sẻ host kernel | Có guest OS/kernel |
| Isolation | Process/namespace/cgroup | Hypervisor/hardware-assisted |
| Startup/overhead | Thường thấp hơn | Thường cao hơn |
| GPU access | Toolkit và device allocation | Passthrough hoặc vGPU |

Container có thể chạy bên trong GPU-enabled VM.

### GPU Passthrough

Gán toàn bộ physical GPU trực tiếp cho một VM.

Ưu điểm:

- Exclusive, gần native performance.
- Performance model đơn giản.

Tradeoff:

- GPU đó không chia cho VM khác cùng lúc.
- Consolidation thấp hơn.
- Cần IOMMU, firmware và hypervisor support.
- NVLink-connected GPU có thể có assignment constraint.

Phù hợp khi một VM cần toàn bộ GPU.

### NVIDIA vGPU

vGPU cho nhiều VM dùng resource của physical GPU thông qua profile và management software được hỗ trợ.

Ưu điểm:

- Consolidation và VM-level isolation.
- Flexible profile cho supported graphics/compute use case.
- Enterprise virtualization integration.

Tradeoff:

- Licensing và compatibility matrix quan trọng.
- Sharing mode/profile ảnh hưởng predictability.
- Hypervisor, host manager và guest driver phải tương thích.

Phù hợp khi nhiều VM tenant cần managed fractional GPU.

### Multi-Instance GPU — MIG

MIG chia một supported GPU theo không gian thành các isolated GPU instance với phần compute và memory resource riêng.

Ưu điểm:

- Hardware-enforced resource separation.
- Predictable slice cho small workload.
- Tăng utilization nếu workload không cần cả GPU.

Tradeoff:

- Chỉ hỗ trợ một số GPU/profile.
- Fixed resource shape có thể gây fragmentation.
- Có workload cần full GPU.
- Reconfiguration/orchestration cần planning.

Phù hợp khi nhiều workload nhỏ cần slice tách biệt và predictable.

### Time Slicing

Nhiều workload chia execution time trên GPU. Tăng concurrency nhưng không tạo dedicated memory/compute partition giống MIG; performance dưới contention ít predictable hơn.

### Chọn giải pháp

| Requirement | Lựa chọn điển hình |
|---|---|
| Một VM cần toàn bộ GPU | Passthrough |
| Nhiều VM cần managed GPU profile | vGPU |
| Nhiều workload cần isolated hardware slice | MIG |
| Flexible sharing, isolation không nghiêm | Time slicing/scheduler sharing |
| Bare-metal job cần toàn bộ performance | Direct physical allocation |

### Compatibility là thuộc tính của cả hệ thống

Phải validate cả matrix: GPU, firmware, BIOS/IOMMU, hypervisor, vGPU manager, guest/host driver, CUDA, container runtime, orchestrator, MIG profile và licensing. Từng component được support riêng không có nghĩa tổ hợp đó được support.

---

## 3.7 Scenario vận hành

### 100% allocated nhưng throughput thấp

So sánh DCGM activity/application throughput với scheduler allocation. Tìm idle reservation, blocked data loader, synchronization wait và fragmentation.

### Một GPU có uncorrectable error tăng dần

Correlate DCGM health, system log, job failure và interconnect data. Drain node nếu phù hợp, giữ evidence, chạy approved diagnostic và theo hardware support procedure.

### Small inference service lãng phí cả GPU

Nếu hardware/workload hỗ trợ, dùng MIG cho predictable isolated slice hoặc sharing method khác khi không cần strict isolation. Đo latency và interference trước production.

### Distributed job giữ vài GPU nhưng chờ phần còn lại

Dùng gang scheduling để chỉ start khi có đầy đủ resource set.

---

## 3.8 Checklist trước khi thi

- Vì sao OOB management quan trọng khi host failure?
- `nvidia-smi`, NVML, DCGM, DCGM Exporter, Prometheus và Grafana có vai trò gì?
- Metric nào phản ánh activity, memory pressure, throttling và hardware fault?
- Vì sao phải correlate infrastructure metric với application/job metric?
- GPU Operator tự động hóa gì?
- Khi nào dùng Kubernetes, Slurm hoặc cả hai?
- Gang scheduling, backfill, bin packing, fragmentation, preemption và topology-aware placement là gì?
- Container khác VM thế nào?
- Khi nào dùng passthrough, vGPU, MIG hay time slicing?
- Vì sao phải validate toàn bộ compatibility matrix?

## Nguồn chính

- [Official NCA-AIIO study guide](https://dam-cdn.nvd.orangelogic.com/AssetLink/x874j05hy3m3r2sor84kpvp70750m468.pdf)
- [NVIDIA DCGM overview](https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/index.html)
- [DCGM Exporter](https://docs.nvidia.com/datacenter/dcgm/latest/gpu-telemetry/dcgm-exporter.html)
- [NVIDIA GPU Operator](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/index.html)
- [NVIDIA vGPU features](https://docs.nvidia.com/knowledge-base/latest/vgpu-features.html)
- [NVIDIA GPU passthrough](https://docs.nvidia.com/vgpu/latest/grid-vgpu-user-guide/using-gpu-pass-through.html)
- [NVIDIA Base Command Manager](https://docs.nvidia.com/base-command-manager/product-manuals/admin/introduction)

