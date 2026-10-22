# Kế hoạch ôn NCA-AIIO trong bốn tuần

Kế hoạch giả định **60–90 phút mỗi ngày trong tuần** và **2–3 giờ mỗi ngày cuối tuần**. Có thể gộp ngày nếu cần, nhưng nên giữ active recall và timed mock exam.

## Mẫu một buổi học

1. **10 phút:** nhớ lại nội dung hôm trước mà không nhìn tài liệu.
2. **35–50 phút:** đọc và ghi chú section trong ngày.
3. **15 phút:** tự giảng lại như đang hướng dẫn đồng nghiệp.
4. **10 phút:** cập nhật flashcard và error log.

## Tuần 1 — Essential AI Knowledge

### Ngày 1: Blueprint và nền tảng

- Đọc [README.md](./README.md).
- Đọc Chương 1, section 1.1–1.2.
- Tự vẽ AI → ML → DL và nêu ít nhất ba driver của AI adoption.

### Ngày 2: Training và Inference

- Đọc section 1.3.
- Tạo lại bảng so sánh từ trí nhớ.
- Phân biệt scenario ưu tiên latency, throughput, time to train hay convergence.

### Ngày 3: CPU và GPU

- Đọc section 1.4.
- Giải thích CUDA core, Tensor Core, HBM, PCIe, NVLink và NVSwitch.
- Viết một ví dụ CPU bottleneck làm GPU idle.

### Ngày 4: NVIDIA Software Stack

- Đọc section 1.5.
- Flashcard: mặt trước là problem, mặt sau là technology.
- Tập trung TensorRT/Triton, NGC/AI Enterprise và DCGM/scheduler.

### Ngày 5: Lifecycle

- Đọc section 1.6.
- Viết lại lifecycle không nhìn tài liệu.
- Liệt kê artifact cần version để reproducible.

### Ngày 6–7: Solution và Use Case

- Đọc section 1.7–1.10.
- Ghép product với use case.
- Trả lời toàn bộ checklist Chương 1 trong một hoặc hai câu.

## Tuần 2 — AI Infrastructure

### Ngày 8: Workload Sizing

- Đọc section 2.1–2.2.
- Giải thích vì sao training memory nhiều hơn weights.
- Tự tính speedup và scaling efficiency cho hai ví dụ.

### Ngày 9: Cluster Anatomy

- Đọc section 2.3.
- Vẽ compute, storage, in-band và OOB plane.
- Giải thích access nào còn lại khi host OS crash.

### Ngày 10: Storage

- Đọc section 2.4.
- Phân biệt capacity, throughput, IOPS, metadata và latency issue.
- Phân biệt GPUDirect RDMA và GPUDirect Storage.

### Ngày 11–12: Networking

- Đọc section 2.5.
- So sánh NVLink/NVSwitch, InfiniBand, Ethernet/RoCE, PCIe, RDMA và NCCL.
- Giải thích oversubscription, leaf-spine, rail optimization và collective traffic.

### Ngày 13: Deployment Location

- Đọc section 2.6.
- Tạo ba scenario ưu tiên on-premises, cloud và hybrid.
- Luôn xét data gravity và TCO.

### Ngày 14: Facility và DPU

- Đọc section 2.7–2.10.
- Luyện PUE calculation.
- Giải thích chuỗi rack density → cooling → layout → cable → topology.
- Phân biệt BlueField/DOCA và DPU/GPU.

## Tuần 3 — AI Operations

### Ngày 15: Management Plane

- Đọc section 3.1–3.2.
- So sánh in-band/OOB.
- Giải thích BCM, BMC, Redfish và compatibility.

### Ngày 16–17: Monitoring

- Đọc section 3.3.
- Tạo metric map: activity, capacity, limit, reliability và application outcome.
- Giải thích DCGM → DCGM Exporter → Prometheus → Grafana.

### Ngày 18: Troubleshooting

- Đọc section 3.4.
- Với mỗi symptom, đi từ evidence đến hypothesis.
- Không chọn “add more GPUs” trước khi đo bottleneck.

### Ngày 19–20: Orchestration và Scheduling

- Đọc section 3.5.
- So sánh Kubernetes và Slurm.
- Tạo ví dụ gang scheduling, backfill, fragmentation, preemption và topology-aware placement.

### Ngày 21: Virtualization

- Đọc section 3.6–3.8.
- Lập decision table cho passthrough, vGPU, MIG, time slicing, container và bare metal.
- Nhớ toàn bộ compatibility stack cần validate.

## Tuần 4 — Tích hợp và thi thử

### Ngày 22: Cram Sheet lần đầu

- Đọc [04-exam-cram-sheet.md](./04-exam-cram-sheet.md).
- Đánh dấu từng dòng green/yellow/red.
- Quay lại chương cho mọi mục red.

### Ngày 23: Architecture Scenario

Với mỗi scenario, viết bottleneck, evidence và remedy:

- Distributed training scale kém.
- Online inference có p99 latency cao.
- GPU allocation cao nhưng useful activity thấp.
- Rack mới vượt cooling capacity.
- Nhiều small tenant lãng phí full GPU.

### Ngày 24: Product Matching

- Trộn flashcard.
- Trả lời hai chiều product → purpose và requirement → product.
- Ôn các cặp dễ nhầm.

### Ngày 25: Closed-Book Recall

- Tự dựng lại cram sheet trên một trang giấy.
- So sánh và chỉ học phần thiếu/sai.

### Ngày 26: Timed Mock Exam

- Làm [06-mock-exam-questions.md](./06-mock-exam-questions.md) closed-book.
- Đặt timer đúng 60 phút.
- Không mở đáp án cho tới khi hết giờ.

### Ngày 27: Error Analysis

- Dùng [07-mock-exam-answer-key.md](./07-mock-exam-answer-key.md).
- Với mỗi câu sai, ghi nguyên nhân: knowledge gap, đọc sai constraint, chọn sai layer hay đổi đáp án.
- Đọc lại objective tương ứng.

### Ngày 28: Final Review

- Trả lời lại câu sai mà không nhìn option.
- Ôn cram sheet và official blueprint.
- Dừng học nặng sớm để ngủ đủ.

## Readiness Target

- Overall timed mock: **80%+**.
- Mỗi domain: **75%+**.
- Không nhầm các cặp cốt lõi: training/inference, CPU/GPU, TensorRT/Triton, NVLink/scale-out network, MIG/vGPU/passthrough, DCGM/scheduler.
- Kết thúc mock còn ít nhất năm phút để xem câu flagged.

