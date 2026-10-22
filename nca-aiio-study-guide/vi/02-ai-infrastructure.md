# Chương 2 — AI Infrastructure

**Trọng số chính thức: 40%**  
**Phân bổ trong mock exam: 20/50 câu**

## Mục tiêu

Sau chương này, bạn cần có thể:

1. Xác định hardware requirement cho các training use case.
2. Giải thích cách scale GPU infrastructure.
3. Mô tả power và cooling trong data center.
4. So sánh on-premises, cloud và hybrid.
5. Nhận biết các component của accelerated cluster.
6. Xác định facility requirement.
7. Xác định networking requirement cho AI workload.
8. Giải thích data center network protocol và concept.
9. Ghép high-speed network option với use case.
10. Giải thích mục đích và lợi ích của DPU.

---

## 2.1 Bắt đầu từ Workload

Thiết kế hạ tầng phải bắt đầu từ requirement, không bắt đầu từ một product yêu thích. Cần thu thập:

- Model architecture và parameter count.
- Mục tiêu: training, fine-tuning, inference, simulation hay analytics.
- Numerical precision và optimization method.
- Dung lượng cần cho weights, activations, gradients, optimizer state và workspace.
- Dataset size, file-size distribution, preprocessing và access pattern.
- Single-node hay distributed execution.
- Concurrency, latency, throughput và availability target.
- Data governance, locality, sovereignty và security.
- Growth, burst, deadline, budget, power, cooling, rack và support constraint.

### GPU memory là hard constraint

GPU rất nhanh vẫn không phù hợp nếu workload không fit trong memory.

```text
Training memory gần đúng =
  weights + gradients + optimizer state + activations
  + communication/workspace buffers
```

Dung lượng thực tế phụ thuộc precision, optimizer, batch size, sequence length, architecture, parallelism và implementation. Không được chỉ lấy `parameter count × bytes/weight` làm toàn bộ yêu cầu training.

Khi thiếu memory có thể:

- Giảm batch size hoặc sequence length nếu chấp nhận được.
- Dùng mixed/lower precision khi quality và stability cho phép.
- Dùng activation checkpointing.
- Shard parameter, gradient và optimizer state.
- Dùng tensor, pipeline hoặc model parallelism.
- Chọn GPU có memory lớn hơn hoặc GPU fabric mạnh hơn.

### Cân bằng node

| Resource | Vai trò | Dấu hiệu thiếu |
|---|---|---|
| GPU | Parallel compute và accelerator memory | Compute lâu, model không fit |
| CPU | Input preparation, coordination, serial work | GPU chờ trong khi CPU saturated |
| System RAM | Cache dataset, preprocessing, application state | Paging, reload, OOM |
| Local storage | Cache, scratch, checkpoint | Input stall, ghi checkpoint chậm |
| PCIe/host I/O | Host-device transfer | Transfer bottleneck dù GPU idle |
| GPU fabric | Intra-system GPU communication | Multi-GPU scaling kém |
| Network fabric | Inter-node collective và storage access | Communication time cao, straggler |

---

## 2.2 Scale-Up và Scale-Out

### Scale-Up

Scale-up tăng tài nguyên trong một system hoặc tightly coupled domain, ví dụ thêm GPU trong server hoặc liên kết GPU qua NVLink/NVSwitch.

Ưu điểm:

- GPU communication có bandwidth cao và latency thấp.
- Topology đơn giản hơn multi-node.
- Hợp với model cần GPU phối hợp chặt chẽ.

Giới hạn:

- Giới hạn vật lý về GPU, memory, power, cooling và expansion.
- Fault domain có thể lớn nếu nhiều resource tích hợp chặt.

### Scale-Out

Scale-out thêm server/node kết nối bằng high-speed network.

Ưu điểm:

- Tăng tổng compute và memory.
- Mở rộng dần và chạy nhiều job đồng thời.
- Phù hợp large distributed training và high-throughput inference.

Thách thức:

- Network communication và synchronization overhead.
- Topology-aware scheduling phức tạp hơn.
- Storage throughput phải tăng cùng compute.
- Nhiều device, cable và failure point hơn.

### Scaling Efficiency

Nếu một GPU chạy 100 phút, lý tưởng bốn GPU sẽ chạy 25 phút. Thực tế chậm hơn do communication, synchronization, imbalance, serial work và input bottleneck.

```text
Speedup(n) = thời gian với 1 resource / thời gian với n resource

Scaling efficiency = Speedup(n) / n
```

Khi efficiency đã giảm mạnh, thêm GPU chỉ làm lãng phí capacity. Hãy đo compute, collective communication, I/O và imbalance trước.

### Parallelism pattern

- **Data parallelism:** mỗi GPU giữ model replica, xử lý data khác nhau và đồng bộ gradient.
- **Tensor parallelism:** chia tensor operation qua nhiều GPU; communication rất thường xuyên.
- **Pipeline parallelism:** mỗi GPU chạy một stage; cần cân bằng stage và giảm bubble.
- **Expert parallelism:** phân phối Mixture-of-Experts; routing traffic có thể gây áp lực mạng.

---

## 2.3 Thành phần của Accelerated Cluster

```text
Users / CI pipelines
        |
Login, API và management nodes
        |
Scheduler / orchestrator / cluster manager
        |
-------------------------------------------------
| GPU compute nodes | High-speed compute fabric |
| Shared storage    | In-band service network   |
| BMCs và switches  | Out-of-band management    |
-------------------------------------------------
        |
Power, cooling, rack, cabling, security, monitoring
```

### Component chính

1. **GPU compute nodes:** GPU, CPU, RAM, local storage, NIC/SuperNIC và GPU interconnect.
2. **Compute fabric:** mang traffic của distributed workload và collective.
3. **Storage fabric/system:** cấp training data, nhận checkpoint/result.
4. **In-band network:** provisioning, user access, service và general data movement.
5. **Out-of-band network:** BMC, switch và device management độc lập với host OS.
6. **Management/login nodes:** authentication, cluster service, orchestration, monitoring và user entry.
7. **Scheduler/orchestrator:** thường là Kubernetes cho containerized service và Slurm cho queued HPC/AI job.
8. **Software platform:** OS, driver, CUDA, library, container runtime, framework và workload software.
9. **Facility:** power distribution, UPS/generator, cooling, rack, floor loading, fire protection và physical security.

### Vì sao tách network plane?

- Storage traffic nặng không nên làm gián đoạn latency-sensitive training collective.
- OOB access phải còn hoạt động khi OS hoặc production network lỗi.
- Management và user traffic cần policy riêng.

Có thể tách vật lý hoặc logical isolation bằng VLAN, VRF, VXLAN/EVPN. Dù theo cách nào vẫn phải bảo vệ bandwidth và fault domain.

### Resilience

- Loại bỏ single point of failure theo availability requirement.
- Dùng redundant PSU, PDU, network path và management service khi cần.
- Scheduler cần topology awareness để chọn resource khỏe và kết nối tốt.
- Checkpoint long-running job vào durable storage.
- Có spare capacity và replacement procedure.
- Test failover; redundancy chưa kiểm thử chỉ là giả định.

---

## 2.4 Storage cho AI

AI storage không chỉ là capacity.

| Yêu cầu | Ví dụ |
|---|---|
| Capacity | Dataset, model artifact, checkpoint lớn |
| Sequential throughput | Stream sample lớn tới nhiều node |
| IOPS | Nhiều small file hoặc random access |
| Metadata performance | Directory scan, hàng triệu file/object nhỏ |
| Write throughput | Distributed checkpoint thường xuyên |
| Durability | Bảo vệ tiến độ và governed asset |
| Parallel access | Nhiều worker đọc đồng thời |

### Data tier

- **Object/archive storage:** capacity bền vững, tiết kiệm, source of truth.
- **Shared high-performance storage:** parallel access cho active workload/checkpoint.
- **Local NVMe/cache:** latency thấp cho staging/scratch nhưng có thể ephemeral.

### GPUDirect

- **GPUDirect RDMA:** trong cấu hình được hỗ trợ, NIC trao đổi data với GPU memory trực tiếp hơn, giảm CPU và copy.
- **GPUDirect Storage — GDS:** data path trực tiếp hơn giữa storage và GPU memory.

Các công nghệ này giảm overhead, không biến một storage system chậm thành storage nhanh.

### Dấu hiệu storage bottleneck

- GPU utilization tăng khi data đã được cache.
- I/O wait cao.
- Storage link saturated.
- Epoch startup lâu.
- Checkpoint pause dài.

Giải pháp có thể là tăng throughput, đổi data layout/sharding, cache hoặc cải thiện preprocessing trước khi thêm GPU.

---

## 2.5 Networking cho AI

### Bốn câu hỏi thiết kế

1. **Bao nhiêu traffic?** Ước lượng per-node và aggregate bandwidth.
2. **Nhạy với delay đến đâu?** Distributed collective có thể rất latency-sensitive.
3. **Có chịu được loss/retransmission không?** RDMA fabric cần cấu hình đúng.
4. **Topology là gì?** Oversubscription, hop count, rail alignment và path diversity ảnh hưởng performance.

### Thuật ngữ

| Term | Ý nghĩa |
|---|---|
| Bandwidth | Tốc độ truyền tối đa của link/path |
| Throughput | Lượng data hữu ích thực tế truyền được |
| Latency | Thời gian data/operation hoàn thành |
| Jitter | Độ biến thiên của latency |
| Packet loss | Packet bị drop trước khi tới đích |
| Oversubscription | Potential downstream traffic lớn hơn uplink capacity |
| Bisection bandwidth | Capacity qua đường cắt chia network thành hai nhóm |
| East-west traffic | Traffic giữa system trong data center |
| North-south traffic | Traffic vào/ra data center hoặc application boundary |
| RDMA | Remote memory access với ít CPU involvement/copy |
| Collective | Group operation như all-reduce, broadcast, all-gather |

### High-Speed Network Option

#### InfiniBand

High-performance fabric có native RDMA, latency thấp và phù hợp HPC/AI communication. Thường dùng cho compute hoặc storage fabric trong hệ thống AI lớn.

#### Ethernet và RoCE

Ethernet có ecosystem rộng, dùng cho enterprise, management, storage và AI fabric. High-performance AI Ethernet cần congestion control, topology và telemetry đúng. **RoCE** cung cấp RDMA over Ethernet.

#### NVLink và NVSwitch

Tạo high-bandwidth GPU communication domain trong hệ thống được hỗ trợ. Chúng bổ sung chứ không thay thế scale-out network: NVLink/NVSwitch xử lý tight GPU communication cục bộ; InfiniBand hoặc Ethernet kết nối server.

### Protocol/feature map

| Technology | Điểm cần nhớ |
|---|---|
| TCP/IP | Reliable general-purpose network; overhead có thể cao hơn RDMA path |
| UDP | Connectionless; application tự xử lý reliability nếu cần |
| RDMA | Low-copy, low-CPU remote memory access |
| RoCE | RDMA over Ethernet; cần engineered fabric |
| InfiniBand RDMA | Native high-performance communication model |
| GPUDirect RDMA | NIC-to-GPU path trực tiếp hơn trong supported system |
| NCCL | GPU collective library dùng topology/transport hiện có |

### Topology

- **Leaf-spine/fat-tree:** multi-path connectivity và bisection bandwidth cao nếu provision đúng.
- **Rail-optimized design:** căn các path tương ứng giữa node để collective traffic dễ dự đoán.
- **Non-blocking fabric:** thiết kế để simultaneous traffic không bị oversubscription ở design point.

### Congestion và hot spot

Average bandwidth có thể bình thường trong khi một vài link quá tải. Theo dõi per-port utilization, queue, error, dropped packet và collective timing. Topology-aware placement giúp giảm cross-fabric traffic và straggler.

---

## 2.6 On-Premises, Cloud và Hybrid

### On-Premises

Ưu điểm:

- Kiểm soát trực tiếp hardware, topology, security, maintenance và data locality.
- Performance/cost dễ dự đoán cho steady high utilization.
- Phù hợp một số yêu cầu sovereignty hoặc isolated environment.
- Có thể customize network/storage sâu.

Thách thức:

- CapEx và procurement lead time.
- Rủi ro capacity: dư thừa hoặc thiếu.
- Tự chịu trách nhiệm power, cooling, facility và staff.
- Tự quản hardware lifecycle, upgrade và security.

### Public Cloud

Ưu điểm:

- Triển khai nhanh và elastic capacity.
- Phù hợp experiment, burst, temporary project và geographic reach.
- Managed service giảm một phần operation work.
- Không cần tự xây private facility cho capacity sử dụng.

Thách thức:

- Compute, storage và egress cost liên tục.
- Quota, region, topology và instance availability.
- Data movement time/cost.
- Governance, sovereignty và shared responsibility.

### Hybrid

Đặt mỗi stage ở môi trường phù hợp. Ví dụ: xử lý sensitive data và steady training on-premises, cloud cho burst experiment/inference. Đổi lại là complexity về identity, data, network, tool, observability và cost management.

### Decision table

| Requirement | Thường phù hợp |
|---|---|
| Bursty, uncertain, short-term demand | Cloud |
| Stable, high, long-term utilization | On-premises có thể kinh tế hơn |
| Strict local control/disconnected use | On-premises |
| Cần ngay, không có facility | Cloud |
| Dataset rất lớn đã ở local | Train gần data |
| Mỗi stage có nhu cầu khác nhau | Hybrid |

Không có lựa chọn tốt nhất cho mọi trường hợp. Phải tính TCO, utilization, staffing, data movement, risk và opportunity cost.

---

## 2.7 Power, Cooling và Facility

Accelerated system tạo electrical và thermal density cao. Facility planning phải hoàn thành trước khi thiết bị tới.

### Power concept

- **Power (kW):** tốc độ tiêu thụ energy tại một thời điểm.
- **Energy (kWh):** power tiêu thụ theo thời gian.
- **IT load:** compute, storage và networking equipment.
- **Facility load:** IT load cộng cooling, power conversion và overhead.
- **PUE:** `total facility energy / IT equipment energy`. Càng gần 1.0 thường càng hiệu quả.
- **Rated/nameplate power:** giá trị thiết kế/tối đa; không dùng average measured power để thiết kế circuit.
- **Power redundancy:** A/B feed, redundant PSU, UPS, generator và PDU theo availability target.

```text
Rack IT load ≈ tổng maximum planned device load
Facility estimate ≈ IT load × planned PUE
```

Final sizing phải tính code, circuit derating, redundancy, transient và vendor/facility requirement.

### Cooling concept

Gần như toàn bộ electrical power của IT trở thành heat.

- **Air cooling:** front-to-back airflow, hot/cold aisle, containment, airflow và pressure management.
- **Liquid cooling:** hỗ trợ density cao nhưng cần facility water loop, CDU, leak detection, water quality và service procedure.
- **Temperature/humidity:** nằm trong equipment/facility limit; tránh condensation và electrostatic risk.
- **Altitude:** không khí loãng làm giảm cooling capacity; có thể cần derating.
- **Contamination:** filtration và cleanliness bảo vệ equipment.

### Facility checklist

- Utility, UPS, generator, switchgear, PDU và branch circuit.
- Cooling capacity ở facility và từng rack.
- Rack dimension, rail, cable management và service clearance.
- Floor loading, loading dock, freight elevator, door và turning clearance.
- Fiber/copper, transceiver, cable length, bend radius và labeling.
- Grounding/bonding và code.
- Fire suppression và liquid leak detection.
- Physical security và environmental monitoring.
- Staging, burn-in, spare và maintenance access.

### Dependency chain

```text
Power/cooling constraint
    → ít system mỗi rack hơn
    → nhiều rack/floor space hơn
    → nhiều hoặc dài cable/switch hơn
    → topology, latency, cost và deployment thay đổi
```

Không thể thiết kế facility và network độc lập.

---

## 2.8 Data Processing Unit — DPU

NVIDIA BlueField DPU kết hợp high-speed networking, programmable processing và hardware accelerator. DPU chạy infrastructure service tách khỏi host CPU.

### Lợi ích chính

- **Offload:** chuyển networking, storage, security và virtualization work khỏi host CPU.
- **Accelerate:** chạy supported infrastructure function hiệu quả gần data path.
- **Isolate:** đặt trusted infrastructure control trong domain riêng với tenant/application.
- **Free host CPU:** dành CPU cycle cho application/AI.
- **Policy consistency:** thực thi service/policy ở server edge.

### DOCA

```text
BlueField = DPU hardware/platform
DOCA      = software framework/ecosystem
```

### CPU, GPU và DPU

| Processor | Trọng tâm thiết kế |
|---|---|
| CPU | General-purpose control, serial/diverse workload |
| GPU | Parallel compute, matrix/tensor throughput |
| DPU | Infrastructure processing, data movement, security, isolation |

DPU không thay GPU cho neural-network training và không thay CPU cho mọi general-purpose logic.

---

## 2.9 Scenario theo bottleneck

### Bốn node scale tốt, mười sáu node scale kém

Đo collective time, hot spot, oversubscription, topology placement, straggler, storage load và synchronization. Thêm node không sửa saturated fabric.

### Rack mới vượt facility limit

Không tùy tiện rải server sang rack khác. Phải tính lại power, cooling, layout, cable distance, switch count, topology và failure domain như một hệ thống.

### Host CPU bận với network/security

DPU có thể offload và isolate supported function. Phải xác nhận toàn bộ hardware/software path hỗ trợ.

### Training dừng lâu khi checkpoint

Kiểm tra aggregate write bandwidth, metadata pressure, coordination và fabric contention. Thêm compute thường không xử lý được pause này.

---

## 2.10 Checklist trước khi thi

- Vì sao model weight size không đủ để tính training memory?
- Scale-up và scale-out khác gì?
- Điều gì làm giảm scaling efficiency?
- Vì sao tách compute, storage, in-band và OOB traffic?
- Khi nào throughput, IOPS, metadata performance và latency quan trọng?
- InfiniBand, Ethernet/RoCE và NVLink khác nhau thế nào?
- RDMA, GPUDirect RDMA, NCCL và oversubscription là gì?
- Constraint nào thiên về on-premises, cloud hay hybrid?
- PUE đo gì?
- Vì sao power, cooling, rack density và networking phải co-design?
- DPU có thể offload, accelerate và isolate gì?

## Nguồn chính

- [Official NCA-AIIO study guide](https://dam-cdn.nvd.orangelogic.com/AssetLink/x874j05hy3m3r2sor84kpvp70750m468.pdf)
- [NVIDIA DGX SuperPOD reference architecture](https://docs.nvidia.com/dgx-superpod/reference-architecture-scalable-infrastructure-b200/latest/)
- [DGX SuperPOD network fabrics](https://docs.nvidia.com/dgx-superpod/reference-architecture/scalable-infrastructure-b300/latest/network-fabrics.html)
- [DGX data center planning](https://docs.nvidia.com/dgx-superpod/design-guides/dgx-superpod-data-center-design-h100/latest/planning.html)
- [NVIDIA AI Enterprise infrastructure software](https://docs.nvidia.com/ai-enterprise/planning-resource/ai-enterprise-infrastructure-software/latest/software-suite.html)

