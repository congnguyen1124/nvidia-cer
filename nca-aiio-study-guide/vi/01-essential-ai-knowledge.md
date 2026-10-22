# Chương 1 — Essential AI Knowledge

**Trọng số chính thức: 38%**  
**Phân bổ trong mock exam: 19/50 câu**

## Mục tiêu

Sau chương này, bạn cần có thể:

1. Mô tả NVIDIA software stack trong môi trường AI.
2. So sánh yêu cầu kiến trúc của training và inference.
3. Phân biệt AI, machine learning và deep learning.
4. Giải thích các yếu tố làm AI phát triển và được áp dụng nhanh.
5. Nhận biết các AI use case và ngành ứng dụng chính.
6. Ghép đúng NVIDIA solution với mục đích sử dụng.
7. Mô tả software components trong vòng đời phát triển và triển khai AI.
8. So sánh kiến trúc CPU và GPU.

---

## 1.1 AI, Machine Learning và Deep Learning

Hãy nhớ quan hệ bao hàm:

```text
Artificial Intelligence
└── Machine Learning
    └── Deep Learning
        └── Nhiều hệ thống Generative AI hiện đại
```

### Artificial Intelligence — AI

AI là lĩnh vực rộng nhất, xây dựng hệ thống có khả năng thực hiện các nhiệm vụ thường gắn với trí tuệ con người: perception, hiểu ngôn ngữ, lập kế hoạch, reasoning và ra quyết định.

AI không bắt buộc phải học từ dữ liệu. Một expert system sử dụng các luật do con người viết vẫn là AI dù không dùng machine learning.

### Machine Learning — ML

ML là một tập con của AI. Thuật toán học pattern từ dữ liệu thay vì chỉ thực hiện các luật được lập trình sẵn.

- **Supervised learning:** học từ dữ liệu có label; thường dùng cho classification và regression.
- **Unsupervised learning:** tìm cấu trúc trong dữ liệu không có label; ví dụ clustering và dimensionality reduction.
- **Reinforcement learning:** agent học policy thông qua reward và penalty khi tương tác với environment.

### Deep Learning — DL

DL là một tập con của ML dựa trên neural network nhiều layer. DL phù hợp với dữ liệu có số chiều lớn hoặc unstructured data như ảnh, âm thanh, văn bản và video.

Training thường gồm:

1. Forward pass.
2. Tính loss.
3. Backpropagation.
4. Cập nhật weight bằng optimizer.
5. Lặp lại qua nhiều batch/epoch.

### Generative AI

Generative AI tạo nội dung mới như text, image, audio, video, code hoặc structured data dựa trên pattern học được. LLM, diffusion model và multimodal foundation model là các ví dụ.

Generative AI thường dùng deep learning, nhưng không đồng nghĩa với toàn bộ AI.

### Bẫy thường gặp

- AI và ML không phải hai từ đồng nghĩa; ML là một phương pháp trong AI.
- Không phải mọi ML algorithm đều là neural network.
- Inference thông thường không tiếp tục training model; nó áp dụng model đã học vào input mới.

---

## 1.2 Vì sao AI phát triển nhanh

Sự tiến bộ của AI đến từ nhiều yếu tố hỗ trợ lẫn nhau:

| Yếu tố | Ý nghĩa |
|---|---|
| Dữ liệu nhiều và tốt hơn | Dataset lớn, đa dạng giúp model học pattern phong phú hơn. |
| Accelerated computing | GPU xử lý song song matrix/tensor với throughput cao. |
| Algorithmic advances | Transformer, attention, optimizer tốt hơn, mixed precision và kiến trúc mới. |
| High-speed interconnect | Giao tiếp GPU-to-GPU và node-to-node nhanh giúp distributed training khả thi. |
| Optimized software | CUDA, library, framework, compiler, container và inference runtime tăng hiệu năng, giảm effort. |
| Cloud và hệ thống đóng gói | Hạ thấp rào cản để thử nghiệm và triển khai. |
| Open ecosystem | Pretrained model, framework và cộng đồng giúp rút ngắn time to solution. |

Ý chính là **full-stack co-design**: hiệu năng không chỉ đến từ GPU. Compute, memory, network, storage, library, framework, orchestration và operations phải phối hợp tốt.

---

## 1.3 Training và Inference

### Training

Training dạy model bằng cách xử lý dữ liệu nhiều lần, đo lỗi và cập nhật parameters. Một job training có thể chạy từ vài giờ đến nhiều ngày và sử dụng dataset rất lớn.

Ưu tiên hạ tầng thường gặp:

- Aggregate compute throughput cao.
- GPU memory capacity và memory bandwidth lớn.
- GPU-to-GPU communication nhanh cho data, tensor hoặc pipeline parallelism.
- Storage và input pipeline đủ nhanh để GPU không bị đói dữ liệu.
- Checkpoint storage bền vững và có write throughput tốt.
- Job dài phải ổn định, có khả năng recovery.
- Khả năng scale qua nhiều GPU và node.

### Inference

Inference áp dụng model đã train vào input mới để tạo prediction hoặc output. Có hai dạng phổ biến:

- **Online/interactive inference:** ưu tiên response latency, đặc biệt p95/p99 latency.
- **Batch inference:** ưu tiên tổng throughput và cost/item.

Ưu tiên hạ tầng:

- Latency thấp cho real-time service.
- Throughput cao khi có nhiều request hoặc batch lớn.
- Tail latency ổn định và đạt SLO.
- Availability, autoscaling và load balancing.
- Cost/request hoặc cost/token.
- Model optimization, quantization, batching và caching.
- Placement phù hợp ở cloud, data center hoặc edge.

### Bảng so sánh

| Tiêu chí | Training | Inference |
|---|---|---|
| Mục đích | Học/cập nhật weight | Dùng weight đã học để xử lý dữ liệu mới |
| Công việc chính | Forward + backward + optimizer update | Chủ yếu forward pass/token generation |
| Pattern | Job phân tán, dài | Online service hoặc offline batch |
| Metric | Time to train, scaling efficiency, convergence | Latency, throughput, cost, availability |
| Dữ liệu | Dataset lớn, đọc lặp lại | Request mới, model weights và cache |
| Failure concern | Mất tiến độ; cần checkpoint | Lỗi người dùng/SLO; cần redundancy |

### Latency khác Throughput

- **Latency:** thời gian hoàn thành một request.
- **Throughput:** lượng công việc hoàn thành trong một đơn vị thời gian.

Batching thường tăng throughput nhưng có thể tăng thời gian request phải chờ. Không có cấu hình tối ưu cho mọi service; phải dựa vào SLO.

---

## 1.4 Kiến trúc CPU và GPU

### CPU phù hợp với gì?

CPU có số lượng core tương đối ít nhưng mỗi core mạnh, có cache lớn, branch prediction và control logic phức tạp. CPU tốt cho:

- OS và application logic tổng quát.
- Serial hoặc branch-heavy code.
- Data preprocessing và coordination.
- I/O control và orchestration.
- Công việc có ít parallelism.

### GPU phù hợp với gì?

GPU dành nhiều transistor cho các arithmetic unit song song và tối ưu throughput. GPU tốt cho:

- Matrix/tensor operation.
- Neural-network training và inference.
- Simulation, analytics, graphics và scientific computing có tính song song cao.
- Workload có arithmetic intensity và data parallelism lớn.

### CPU và GPU bổ trợ nhau

Một accelerated server cần cả hai. CPU chạy OS, chuẩn bị input và điều phối; GPU tăng tốc kernel song song. CPU preprocessing hoặc storage chậm có thể khiến GPU tốt vẫn idle.

### Khái niệm GPU cần nhớ

- **Streaming Multiprocessor — SM:** execution block chính của GPU.
- **CUDA cores:** xử lý floating-point và integer operation tổng quát.
- **Tensor Cores:** tăng tốc matrix operation dùng nhiều trong AI và lower-precision computation.
- **HBM/GPU memory:** lưu weights, activations, optimizer state và working data. Capacity quyết định workload có fit hay không; bandwidth ảnh hưởng tốc độ cấp dữ liệu cho compute.
- **ECC:** phát hiện/sửa một số lỗi memory, quan trọng với reliability.
- **PCIe:** kết nối GPU với host và các device khác.
- **NVLink:** high-bandwidth interconnect cho GPU communication trong hệ thống được hỗ trợ.
- **NVSwitch:** tạo switched NVLink fabric để nhiều GPU giao tiếp băng thông cao.

### Amdahl's Law theo cách dễ hiểu

Tăng tốc một phần không thể làm toàn bộ workflow nhanh vô hạn. Phần serial hoặc non-accelerated còn lại sẽ trở thành bottleneck. Vì vậy phải đo trước khi thêm accelerator.

---

## 1.5 NVIDIA AI Software Stack

Học theo **chức năng**, không học thuộc danh sách rời rạc:

```text
Applications và industry workflows
  NIM | NeMo | Riva | Metropolis | MONAI | Omniverse/Isaac

Frameworks, models và inference
  PyTorch/TensorFlow | TensorRT | Triton | RAPIDS

Accelerated libraries và communication
  CUDA-X | cuDNN | NCCL | Magnum IO | GPUDirect

Platform và lifecycle management
  AI Enterprise | NGC | GPU Operator | Run:ai | BCM | DCGM

System software và hardware
  Driver | CUDA runtime | GPU | DPU/SuperNIC | NVLink/NVSwitch | networking
```

### Ghép component với mục đích

| Component | Mục đích chính | Từ khóa nhận biết |
|---|---|---|
| NVIDIA driver | Cho OS/software giao tiếp với GPU | Foundation; compatibility quan trọng |
| CUDA | Parallel computing platform và programming model | Chạy general-purpose work trên NVIDIA GPU |
| CUDA-X | Các domain library được tối ưu | AI, HPC, data processing |
| cuDNN | Primitive tăng tốc deep neural network | DL operation cho framework |
| NCCL | Collective communication giữa GPU | All-reduce và distributed training |
| TensorRT | Tối ưu/compile và chạy inference | Latency thấp, throughput cao |
| Triton Inference Server | Serve model từ nhiều framework | Serving, batching, concurrent execution |
| NIM microservices | Inference microservice được đóng gói và tối ưu | Standard API, triển khai nhanh |
| NeMo | Phát triển/customize generative AI | Build, customize, evaluate model/agent |
| NGC Catalog | Hub cho container, model, Helm chart và resource | Curated GPU-optimized artifacts |
| NVIDIA AI Enterprise | Enterprise software platform và support | Validated stack, security update, SLA |
| GPU Operator | Tự động quản lý GPU software trong Kubernetes | Driver, toolkit, plugin, monitoring |
| Network Operator | Tự động hóa networking component trong Kubernetes | High-performance network enablement |
| DCGM | GPU telemetry, health, diagnostics, policy, accounting | Operations/monitoring, không serve model |
| Base Command Manager | Provision, administer và monitor cluster | OS provisioning, inventory, health |
| Run:ai | GPU workload orchestration/resource management | Scheduling, sharing, quota, utilization |
| vGPU | Chia GPU cho VM theo profile được hỗ trợ | VM-based virtualization |
| MIG | Chia GPU thành hardware-isolated instance | Predictable partition |
| DOCA | Software framework cho BlueField DPU/SuperNIC | Network, storage, security offload |

### TensorRT và Triton

Đây là cặp rất dễ nhầm:

- **TensorRT:** optimize/compile model và cung cấp runtime inference.
- **Triton Inference Server:** serve model, nhận request, batching và concurrent execution.
- Có thể dùng cùng nhau: TensorRT-optimized model được Triton serve.

### NGC và AI Enterprise

- **NGC:** catalog và distribution channel cho container, model, Helm chart và artifact.
- **NVIDIA AI Enterprise:** enterprise platform gồm validated component, lifecycle branch, security update và support.

---

## 1.6 Vòng đời AI Development và Deployment

Một lifecycle điển hình:

1. **Define problem và success criteria:** quality, latency, cost, risk, business value.
2. **Collect và govern data:** thu thập, label, secure, catalog và kiểm soát access.
3. **Prepare data:** clean, transform, tokenize, augment và chia train/validation/test.
4. **Develop/select model:** thử architecture, pretrained model, feature và hyperparameter.
5. **Train/customize:** pretrain, fine-tune, transfer learning hoặc adaptation.
6. **Evaluate/validate:** accuracy, bias, robustness, safety và nonfunctional requirement.
7. **Optimize:** quantization, pruning, fusion, compile hoặc kernel tuning.
8. **Package/register:** version model, dependency, container, metadata và approval.
9. **Deploy/serve:** batch job, API, streaming service hoặc edge application.
10. **Monitor:** infrastructure health, service SLO, model/data drift, quality và cost.
11. **Retrain/retire:** update, rollback, lưu lineage và decommission asset cũ.

### Reproducibility và Governance

Cần version ít nhất:

- Data và model.
- Source code và container image.
- Framework, CUDA, driver và library compatibility.
- Hyperparameter và random seed khi phù hợp.
- Evaluation result và approval history.
- Deployment configuration và rollback target.

**MLOps** áp dụng automation, versioning, CI/CD, governance và monitoring cho ML lifecycle. Infrastructure monitoring chỉ là một phần của MLOps; còn phải theo dõi model và data behavior.

---

## 1.7 Ghép NVIDIA Solution với bài toán

| Nhu cầu | Technology family phù hợp |
|---|---|
| General GPU programming | CUDA |
| Distributed GPU collectives | NCCL |
| Optimized inference engine | TensorRT / TensorRT-LLM |
| Multi-framework model serving | Triton Inference Server |
| Packaged inference API | NIM microservices |
| Generative AI customization | NeMo |
| Speech AI | Riva |
| Healthcare imaging AI | MONAI |
| Video analytics/smart spaces | Metropolis và DeepStream |
| Robotics simulation/development | Isaac; Omniverse-based simulation |
| Industrial digital twin | Omniverse |
| Edge AI | Jetson |
| Autonomous vehicle | DRIVE |
| Enterprise AI software | NVIDIA AI Enterprise |
| Curated GPU-optimized asset | NGC Catalog |
| Integrated AI system | DGX systems |
| Validated scalable AI cluster | DGX SuperPOD |
| Kubernetes GPU enablement | GPU Operator |
| Data center GPU telemetry | DCGM |
| Cluster provisioning/admin | Base Command Manager |
| Infrastructure offload/isolation | BlueField DPU + DOCA |

Không nên nghĩ một product bao phủ toàn bộ lifecycle. Production solution thường kết hợp hardware, networking, software, orchestration và operations.

---

## 1.8 Use Case và Industry

### Workload pattern phổ biến

- **Computer vision:** classification, object detection, segmentation, quality inspection, medical imaging.
- **NLP và generative AI:** search, summarization, assistant, code generation, document processing.
- **Speech AI:** speech recognition, translation, text-to-speech.
- **Recommendation:** ranking và personalization.
- **Forecasting/anomaly detection:** demand, risk, fraud và predictive maintenance.
- **Simulation/digital twins:** thử và tối ưu system trước khi triển khai vật lý.
- **Robotics/autonomous systems:** perception, planning, control, synthetic data.

### Ví dụ theo ngành

| Ngành | Use case |
|---|---|
| Healthcare/life sciences | Medical imaging, drug discovery, genomics, clinical documentation |
| Financial services | Fraud detection, risk modeling, document processing, assistant |
| Manufacturing | Visual inspection, predictive maintenance, digital twin, robotics |
| Retail | Recommendation, demand forecasting, intelligent store, supply chain |
| Telecommunications | Network optimization, anomaly detection, customer service |
| Automotive | Autonomous driving, simulation, in-vehicle AI, manufacturing |
| Public sector | Research, disaster response, document analysis, smart infrastructure |
| Media/entertainment | Rendering, content generation, recommendation, video processing |

### Ghép hạ tầng với workload

- Large distributed training cần fast interconnect và storage throughput cao.
- Latency-sensitive edge application cần locality, power efficiency và deployment nhỏ gọn.
- Regulated workload có thể ưu tiên data sovereignty và isolation.
- Bursty experiment thường phù hợp với cloud elasticity.
- Steady high-utilization workload có thể phù hợp với on-premises.

---

## 1.9 Tư duy qua scenario

### Training GPU có utilization thấp

Không thêm GPU ngay. Kiểm tra data loading, CPU preprocessing, storage, collective communication, synchronization và batch size.

### Online service không đạt latency target

Đo riêng queue time, preprocessing, model execution và postprocessing. Cân nhắc optimized inference engine, replica, load balancing, caching và batching phù hợp. Batch quá lớn có thể tăng latency.

### Cần môi trường reproducible nhanh

Dùng validated container từ NGC, pin version/digest và ghi nhận compatibility. Container đóng gói user-space dependency nhưng vẫn cần host driver tương thích.

---

## 1.10 Checklist trước khi thi

Bạn phải trả lời được mà không nhìn tài liệu:

- ML nằm trong AI hay ngược lại? DL nằm ở đâu?
- Training memory gồm gì ngoài model weights?
- Vì sao distributed training cần fast GPU interconnect?
- Batching ảnh hưởng latency và throughput thế nào?
- CPU và GPU được tối ưu cho loại công việc nào?
- Tensor Cores, HBM, NVLink và NVSwitch dùng để làm gì?
- TensorRT khác Triton thế nào?
- NGC cung cấp những loại artifact nào?
- GPU Operator và DCGM làm gì?
- NeMo, NIM, Riva, MONAI, Metropolis, Jetson và DGX phù hợp với bài toán nào?
- Cần version những gì để workflow reproducible?

## Nguồn chính

- [Official NCA-AIIO study guide](https://dam-cdn.nvd.orangelogic.com/AssetLink/x874j05hy3m3r2sor84kpvp70750m468.pdf)
- [NVIDIA AI Enterprise software overview](https://docs.nvidia.com/ai-enterprise/software/latest/overview.html)
- [NVIDIA AI Enterprise application software](https://docs.nvidia.com/ai-enterprise/software/latest/application-software.html)
- [NVIDIA NGC documentation](https://docs.nvidia.com/ngc/index.html)
- [NVIDIA GPU Operator](https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/index.html)
- [NVIDIA TensorRT](https://developer.nvidia.com/TensorRT)
- [What is AI inference?](https://www.nvidia.com/en-us/glossary/ai-inference/)

