# NCA-AIIO Mock Exam — Đáp án và giải thích

Chỉ mở file này sau khi hoàn thành đề thi thử 60 phút.

## Đáp án nhanh

| Câu | Đáp án | Câu | Đáp án | Câu | Đáp án | Câu | Đáp án | Câu | Đáp án |
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

Với câu **Chọn HAI**, chỉ tính một điểm khi chọn đúng cả hai và không chọn thêm đáp án khác.

## Diễn giải điểm

Đây là ngưỡng luyện tập, không phải passing score chính thức của NVIDIA.

| Điểm | Đánh giá | Hành động |
|---:|---|---|
| 45–50 | Rất tốt | Xem lại câu sai và làm thêm một lượt timed recall. |
| 40–44 | Có thể bước vào final review | Đóng gap, đặc biệt domain dưới 75%. |
| 35–39 | Đang phát triển | Đọc lại section liên quan và test lại sau 48 giờ. |
| Dưới 35 | Cần củng cố nền tảng | Học lại chương bằng active recall trước mock tiếp theo. |

### Câu hỏi theo domain

- **Essential AI Knowledge (19):** 1, 4, 6, 8, 11, 13, 16, 19, 21, 23, 26, 28, 31, 34, 36, 39, 42, 46, 49.
- **AI Infrastructure (20):** 2, 5, 7, 10, 12, 15, 18, 20, 24, 27, 29, 32, 35, 37, 40, 43, 45, 47, 48, 50.
- **AI Operations (11):** 3, 9, 14, 17, 22, 25, 30, 33, 38, 41, 44.

Mục tiêu khuyến nghị: **ít nhất 75% ở mỗi domain**.

---

## Giải thích chi tiết

### 1. B — DL nằm trong ML, ML nằm trong AI

AI là phạm vi rộng nhất. ML là cách để system học từ data; DL là họ ML dùng multi-layer neural network. AI còn gồm cả rule-based system không học từ data.  
**Objective:** 1.3 — AI, ML và DL.

### 2. C — Training cần nhiều memory hơn weights

Training có thể lưu gradients, optimizer state, activations, workspace và communication buffer. Footprint phụ thuộc precision, optimizer, batch/sequence size, architecture và parallelism.  
**Objective:** 2.1 — Hardware requirement.

### 3. C — Allocation không phải useful utilization

GPU đã được allocate vẫn có thể đang chờ input, synchronization, CPU preprocessing hoặc resource khác. Phải correlate scheduler với GPU activity và application throughput.  
**Objective:** 3.1 và 3.3 — Monitoring và GPU metric.

### 4. C — TensorRT tối ưu inference

TensorRT là inference optimizer/compiler và runtime giúp cải thiện latency/throughput. DCGM monitor GPU; BCM quản lý cluster; GDS cải thiện supported storage data path.  
**Objective:** 1.1 và 1.6 — Software stack và solution purpose.

### 5. B — Thêm networked server là scale-out

Scale-out thêm node và aggregate resource qua fabric. Scale-up tăng resource bên trong một system hoặc tightly coupled domain.  
**Objective:** 2.2 — Scale GPU infrastructure.

### 6. B — Online inference ưu tiên service latency/throughput

Interactive inference cần response latency, nhất là tail latency, đồng thời đạt request rate và availability. Time to train/optimizer state thuộc training.  
**Objective:** 1.2 — Training và inference.

### 7. C — Đo bottleneck trước

Scaling kém thường do collective communication, oversubscription, topology, storage, synchronization hoặc straggler. Thêm node có thể làm efficiency thấp hơn.  
**Objective:** 2.2 và 2.7 — Scaling và network requirement.

### 8. B — NGC phân phối optimized artifact

NGC Catalog cung cấp GPU-optimized container, model, Helm chart và resource. NVML là management library; Redfish là hardware-management interface; NVSwitch là interconnect.  
**Objective:** 1.1 — NVIDIA software stack.

### 9. C — DCGM Exporter kết nối telemetry với Prometheus

DCGM Exporter chuyển selected DCGM field sang Prometheus exposition format. Prometheus scrape/lưu metric; Grafana có thể visualize.  
**Objective:** 3.1 và 3.3 — Monitoring và GPU metric.

### 10. B — OOB hoạt động độc lập host OS

BMC qua OOB cung cấp power control, remote console, sensor và recovery. Vì có quyền cao, OOB phải được isolate và bảo mật nghiêm.  
**Objective:** 2.5 và 2.6 — Cluster component/facility.

### 11. B — GPU Operator quản lý Kubernetes GPU stack

GPU Operator tự động lifecycle của driver, container toolkit, device plugin, feature discovery, MIG management và DCGM monitoring tùy cấu hình. Nó hoạt động bên trong, không thay thế Kubernetes.  
**Objective:** 1.1 — NVIDIA software stack.

### 12. C — 800 kW × 1,25 = 1.000 kW

PUE là total facility energy chia IT equipment energy. Ước lượng steady-state: `800 × 1,25 = 1.000 kW`. Final design còn phải xét code, redundancy, derating và equipment requirement.  
**Objective:** 2.3 — Power/cooling.

### 13. A — Triton là model server

Triton hỗ trợ nhiều backend cùng dynamic batching và concurrent execution. TensorRT có thể tối ưu model rồi Triton serve model đó.  
**Objective:** 1.1 và 1.7 — Software stack/deployment.

### 14. B — Gang scheduling cấp full set cùng lúc

Tightly coupled job không chạy được khi thiếu resource. Gang scheduling tránh giữ partial allocation idle trong lúc chờ phần còn lại.  
**Objective:** 3.2 — Scheduling.

### 15. A — GPUDirect Storage rút ngắn storage path

GDS tạo path trực tiếp hơn giữa storage và GPU memory trong supported system, giảm CPU và extra copy. GPUDirect RDMA áp dụng concept tương tự cho NIC communication.  
**Objective:** 2.1 và 2.5 — Hardware/cluster component.

### 16. B — CPU thiên control, GPU thiên parallel throughput

CPU có control/cache phức tạp và ít core mạnh; GPU có nhiều parallel unit. Accelerated system dùng cả hai bổ trợ nhau.  
**Objective:** 1.8 — GPU/CPU architecture.

### 17. B và E — Temperature và active throttle evidence

Temperature gần thermal limit cùng clock giảm và thermal-throttle reason active trực tiếp chứng minh throttling. Model allocation/job priority không chứng minh điều đó.  
**Objective:** 3.3 — GPU metric.

### 18. A — Constraint nghiêng về on-premises

Steady high utilization, facility có sẵn và strict local control thường phù hợp on-premises. Tổ chức vẫn phải tự vận hành lifecycle, security, power và cooling.  
**Objective:** 2.4 — On-premises/cloud.

### 19. A — Riva là speech AI platform

Riva phục vụ speech recognition, translation và speech synthesis. DCGM/BCM là operations tool; NVSwitch là interconnect.  
**Objective:** 1.6 — NVIDIA solution.

### 20. A và D — Kiểm tra communication và data path

Chờ gradient synchronization gợi ý compute fabric/collective issue. Chờ dataset read gợi ý storage/data pipeline. Hai bottleneck có thể tồn tại đồng thời.  
**Objective:** 2.2, 2.5 và 2.7.

### 21. A — NIM là packaged inference microservice

NIM đóng gói optimized inference và standard service API cho supported model. NVML là management library; UEFI là firmware; SNMP là management protocol.  
**Objective:** 1.1 và 1.6.

### 22. B — Slurm phù hợp queued distributed training

Slurm có queue, priority, partition, reservation, backfill, accounting và resource allocation cho HPC/large training. Kubernetes thường phù hợp containerized service; tổ chức có thể dùng cả hai.  
**Objective:** 3.2 — Orchestration/scheduling.

### 23. C — Training học, inference áp dụng

Training điều chỉnh weight bằng data và optimization. Inference dùng parameter đã học để tạo output cho input mới, thường tối ưu latency, throughput và cost.  
**Objective:** 1.2 — Training/inference.

### 24. B — DPU xử lý infrastructure service

BlueField DPU offload, accelerate và isolate networking, storage, security và virtualization function, giải phóng CPU và tạo trusted infrastructure domain riêng.  
**Objective:** 2.10 — DPU.

### 25. B — Limit làm giảm effective clock

High activity cùng temperature tăng, clock giảm và throughput giảm cho thấy throttling. Utilization chỉ mô tả activity, không phải work/second.  
**Objective:** 3.3 — GPU metric.

### 26. A và D — Reproducibility cần input, code, config và environment

Dataset, model/code, hyperparameter, framework/library/container version xác định run. Production còn nên lưu evaluation, compatibility, approval và lineage.  
**Objective:** 1.7 — AI lifecycle.

### 27. B — Endpoint demand vượt uplink capacity

Oversubscribed network có tổng potential downstream traffic lớn hơn uplink có thể mang đồng thời. Điều này có thể làm hại distributed AI khi nhiều endpoint giao tiếp cùng lúc.  
**Objective:** 2.8 — Networking concept.

### 28. C — DL dùng multi-layer neural network

DL là tập con của ML và ML là tập con của AI. Rule-based application có thể là AI mà không phải ML/DL.  
**Objective:** 1.3 — AI/ML/DL.

### 29. B — Small file gây áp lực metadata/IOPS

Mở và tìm hàng triệu file có thể làm nghẽn metadata/random I/O dù capacity và sequential throughput đủ. Data sharding/packing có thể cải thiện access pattern.  
**Objective:** 2.1 và 2.5.

### 30. C — GPU Operator tự động node GPU software stack

Operator quản lý component Kubernetes node cần để expose/operate GPU. Nó không thay orchestrator và không optimize model như TensorRT.  
**Objective:** 3.2 — Orchestration.

### 31. B — NCCL cung cấp GPU collective

NCCL tối ưu all-reduce, all-gather, reduce và broadcast giữa NVIDIA GPU. Nó dùng topology/transport có sẵn chứ không phải physical fabric.  
**Objective:** 1.1 — NVIDIA software stack.

### 32. B — Elastic cloud hợp temporary uncertainty

Experiment ngắn, demand chưa rõ và không có facility là use case tốt cho cloud elasticity. Vẫn phải kiểm tra quota, region, topology, data movement, governance và cost.  
**Objective:** 2.4 — Cloud/on-premises.

### 33. B — Resource fragmentation

Tổng free capacity đủ nhưng không đúng type, số lượng, node hoặc topology. Bin packing, policy và topology-aware placement có thể giúp.  
**Objective:** 3.2 — Scheduling.

### 34. C — TensorRT optimize, Triton serve

Hai product giải quyết hai lớp bổ trợ: TensorRT tạo/chạy optimized inference representation; Triton xử lý serving và backend.  
**Objective:** 1.1, 1.6 và 1.7.

### 35. A — Thêm tight resource trong system/domain là scale-up

NVLink/NVSwitch cung cấp high-bandwidth communication trong local/tightly coupled system. Kết nối thêm server qua scale-out fabric là hướng mở rộng khác.  
**Objective:** 2.2 và 2.9.

### 36. A — Metropolis cho video analytics/smart spaces

Metropolis, thường cùng DeepStream, hỗ trợ vision AI/intelligent video analytics. Các lựa chọn còn lại là infrastructure management hoặc virtualization.  
**Objective:** 1.5 và 1.6.

### 37. B và D — Power và heat removal là gating constraint

High-density rack cần đủ electrical capacity và cooling/environmental design. Constraint này còn ảnh hưởng layout, cable, topology, floor load và maintenance.  
**Objective:** 2.3 và 2.6.

### 38. B — vGPU chia supported GPU giữa VM

NVIDIA vGPU dùng managed profile cho VM sharing và cần hardware/software/licensing phù hợp. Passthrough gán GPU độc quyền nên không thể gán cùng full device cho nhiều VM đồng thời.  
**Objective:** 3.4 — Virtualization.

### 39. A — Monitor behavior và govern lifecycle action

Infrastructure có thể khỏe nhưng quality giảm do input distribution thay đổi. Cần monitor model/data, lưu lineage và retrain/validate/rollback có kiểm soát.  
**Objective:** 1.7 — AI lifecycle.

### 40. A — RoCE mang RDMA qua Ethernet

RoCE cung cấp RDMA semantic trên engineered Ethernet fabric. Congestion management, topology, loss behavior và telemetry đều quan trọng.  
**Objective:** 2.8 và 2.9.

### 41. B — BMC/OOB độc lập host OS

BMC cung cấp remote power, console, sensor, inventory và event access qua OOB. Phải bảo mật như privileged infrastructure.  
**Objective:** 3.1 — Data center management.

### 42. D — AI tiến bộ nhờ cả full stack

Dataset lớn, accelerated compute, algorithm innovation, fast network, optimized software và accessible platform hỗ trợ lẫn nhau. Không có một yếu tố duy nhất giải thích toàn bộ.  
**Objective:** 1.4 — AI adoption.

### 43. B — Liquid cooling chuyển chứ không xóa facility work

Liquid cooling xử lý heat density cao nhưng yêu cầu coolant distribution, water quality, leak detection, redundancy và maintenance.  
**Objective:** 2.3 và 2.6.

### 44. A — Passthrough gán full GPU cho một VM

Passthrough phù hợp khi VM cần exclusive, near-native use. Đổi lại là không share và phải có IOMMU, firmware, hypervisor, topology, guest-driver support.  
**Objective:** 3.4 — Virtualization.

### 45. A — Tách fabric bảo vệ workload communication

Training collective và storage I/O đều bursty/bandwidth intensive. Tách hoặc isolate giúp một loại traffic không làm giảm loại kia và cải thiện failure isolation.  
**Objective:** 2.5 và 2.7.

### 46. B — Container vẫn phụ thuộc host path

Container package user-space software nhưng chia sẻ host kernel. GPU access vẫn cần compatible NVIDIA driver và runtime/toolkit expose device/library.  
**Objective:** 1.1 và 1.7.

### 47. C — Khoảng 83%

`Speedup = 100 / 30 = 3,33`. `Efficiency = 3,33 / 4 = 0,833`, tức khoảng 83%. Phần thiếu do communication, synchronization, serial work hoặc imbalance.  
**Objective:** 2.2 — Scaling.

### 48. D — BlueField + DOCA xử lý infrastructure offload

DPU chạy supported network, storage và security service gần data path và tách khỏi application CPU domain. DOCA là software framework hỗ trợ.  
**Objective:** 2.10 — DPU.

### 49. A và D — GPU có thể bị input-starved

Burst ngắn xen idle gap thường nghĩa GPU chờ CPU preprocessing, data loader hoặc storage. Thêm GPU chỉ có thể làm starvation nặng hơn.  
**Objective:** 1.2 và 1.8.

### 50. D — Kết hợp on-premises và cloud là hybrid

Dùng private infrastructure cho steady/sensitive work và public cloud cho burst là hybrid. Nó linh hoạt nhưng tăng complexity về identity, data movement, network, observability và governance.  
**Objective:** 2.4 — On-premises/cloud.

---

## Ôn lại theo objective

| Nếu sai nhóm này | Đọc lại |
|---|---|
| AI/ML/DL, training/inference | Chương 1, section 1.1–1.3 |
| CPU/GPU và memory | Chương 1 section 1.4; Chương 2 section 2.1 |
| NVIDIA software/product | Chương 1 section 1.5–1.7 |
| Scaling, storage, networking | Chương 2 section 2.2–2.5 |
| Cloud/on-premises | Chương 2 section 2.6 |
| Power/cooling/facility | Chương 2 section 2.7 |
| DPU | Chương 2 section 2.8 |
| Monitoring/GPU metric | Chương 3 section 3.1–3.4 |
| Scheduling | Chương 3 section 3.5 |
| Virtualization | Chương 3 section 3.6 |

Sau khi ôn, trả lời lại câu sai **mà không nhìn option**. Khi bạn giải thích được rule và vì sao mỗi distractor sai, kiến thức đã vững hơn nhiều.

