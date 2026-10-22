# NCA-AIIO Mock Exam — 50 câu

## Hướng dẫn

- **Thời gian:** 60 phút.
- **Số câu:** 50.
- **Phân bố:** 19 Essential AI Knowledge, 20 AI Infrastructure, 11 AI Operations.
- Các domain được trộn để mô phỏng việc chuyển context trong kỳ thi.
- Trừ khi đề ghi **Chọn HAI**, chỉ chọn một đáp án.
- Với câu **Chọn HAI**, phải chọn đúng cả hai và không chọn thừa mới được tính điểm.
- Làm closed-book. Chỉ mở đáp án sau khi hết giờ.

Đây là đề luyện tập nguyên bản theo public blueprint, không phải đề thi chính thức và không sao chép live exam content.

## Phiếu trả lời

```text
 1.__  2.__  3.__  4.__  5.__  6.__  7.__  8.__  9.__ 10.__
11.__ 12.__ 13.__ 14.__ 15.__ 16.__ 17.__ 18.__ 19.__ 20.__
21.__ 22.__ 23.__ 24.__ 25.__ 26.__ 27.__ 28.__ 29.__ 30.__
31.__ 32.__ 33.__ 34.__ 35.__ 36.__ 37.__ 38.__ 39.__ 40.__
41.__ 42.__ 43.__ 44.__ 45.__ 46.__ 47.__ 48.__ 49.__ 50.__
```

---

### 1. Phát biểu nào mô tả đúng quan hệ giữa AI, machine learning và deep learning?

A. AI là tập con của machine learning, còn machine learning là tập con của deep learning.  
B. Deep learning là tập con của machine learning, còn machine learning là tập con của AI.  
C. Machine learning và deep learning là hai nhánh nằm ngoài AI.  
D. AI luôn bắt buộc phải sử dụng deep neural network.

### 2. Một kỹ sư ước lượng training memory bằng parameter count nhân với số byte của mỗi weight. Vì sao cách tính này chưa đủ?

A. GPU memory chỉ chứa input data, không chứa model weights.  
B. Phép tính chỉ áp dụng cho inference vì training sử dụng CPU memory.  
C. Training còn có thể lưu gradients, optimizer state, activations và workspace buffer.  
D. Parameter count chỉ quyết định network bandwidth, không liên quan memory.

### 3. Dashboard của scheduler cho thấy mọi GPU đều được allocate, nhưng DCGM báo GPU activity thấp và user thấy throughput thấp. Kết luận tốt nhất là gì?

A. Cluster đã được sử dụng hoàn toàn và hiệu quả.  
B. DCGM không thể monitor scheduled workload.  
C. Allocation không chứng minh useful utilization; cần kiểm tra job và data path.  
D. Phải xóa GPU memory trên mọi node ngay lập tức.

### 4. Team đã có trained model và muốn compile/optimize để inference trên NVIDIA GPU có latency thấp hơn. Technology nào giải quyết trực tiếp nhất?

A. DCGM  
B. Base Command Manager  
C. TensorRT  
D. GPUDirect Storage

### 5. Cluster architect thêm nhiều GPU server kết nối bằng high-speed fabric. Đây là phương pháp scale nào?

A. Scale-down  
B. Scale-out  
C. Scale-in  
D. Chỉ scale-up

### 6. Objective nào gắn trực tiếp nhất với interactive online inference service?

A. Giảm time to train qua nhiều epoch.  
B. Giữ tail latency thấp đồng thời đáp ứng request throughput.  
C. Tối đa optimizer-state memory.  
D. Tăng checkpoint size.

### 7. Training scale tốt từ một lên bốn node nhưng rất kém từ bốn lên mười sáu node. Nên làm gì trước?

A. Thêm ngay mười sáu node nữa.  
B. Thay model bằng rule-based system.  
C. Đo collective communication, topology, storage throughput và worker imbalance.  
D. Tắt monitoring để loại bỏ toàn bộ overhead.

### 8. Developer cần một nguồn curated GPU-optimized container, pretrained model và Helm chart. Nên dùng dịch vụ nào?

A. NVML  
B. NGC Catalog  
C. Redfish  
D. NVSwitch

### 9. Vai trò chính của DCGM Exporter là gì?

A. Compile neural network thành optimized inference engine.  
B. Schedule distributed job trên GPU node.  
C. Expose DCGM telemetry theo format mà Prometheus có thể scrape.  
D. Update BMC firmware trên mọi server.

### 10. Host OS của một compute node không phản hồi. Path nào được thiết kế để cung cấp recovery access độc lập?

A. Chỉ model-serving network  
B. Out-of-band management thông qua BMC  
C. Training data loader  
D. CUDA kernel stream

### 11. NVIDIA component nào tự động quản lý lifecycle của driver, container toolkit, device plugin, feature discovery và monitoring component trong Kubernetes GPU cluster?

A. TensorRT  
B. NVIDIA GPU Operator  
C. Triton Inference Server  
D. NCCL

### 12. Data center có IT load 800 kW và PUE 1,25. Total facility power gần đúng là bao nhiêu?

A. 640 kW  
B. 800 kW  
C. 1.000 kW  
D. 1.250 kW

### 13. Platform team cần serve model từ nhiều supported framework và dùng dynamic batching, concurrent model execution. Component nào phù hợp nhất?

A. Triton Inference Server  
B. DCGM Exporter  
C. Baseboard Management Controller  
D. NVLink

### 14. Distributed training job cần tám GPU cùng lúc. Scheduler đang cấp hai GPU nhưng chúng idle trong khi job chờ thêm sáu. Scheduling capability nào tránh lãng phí tốt nhất?

A. Time slicing  
B. Gang scheduling  
C. Round-robin DNS  
D. Thermal throttling

### 15. Technology nào tạo supported data path trực tiếp hơn giữa storage và GPU memory để giảm CPU/extra-copy overhead?

A. GPUDirect Storage  
B. SNMP  
C. vGPU  
D. Redfish

### 16. So sánh nào giữa CPU và GPU là chính xác nhất?

A. CPU có nhiều simple arithmetic unit hơn và luôn nhanh hơn cho tensor operation.  
B. CPU ưu tiên low-latency general control; GPU ưu tiên throughput cho highly parallel work.  
C. GPU thay thế CPU cho OS và orchestration.  
D. CPU và GPU có execution architecture giống nhau.

### 17. Operator nghi một GPU đang bị thermal throttling. Hai observation nào trực tiếp hỗ trợ giả thuyết? **Chọn HAI.**

A. Model file chiếm GPU memory.  
B. GPU temperature gần thermal limit.  
C. Job có scheduling priority cao.  
D. BMC account dùng multi-factor authentication.  
E. Clock frequency giảm trong khi thermal-throttle reason active.

### 18. Tổ chức regulated có workload ổn định, utilization cao, facility phù hợp và yêu cầu sensitive data nằm dưới local control trực tiếp. Placement nào có khả năng phù hợp nhất?

A. On-premises infrastructure  
B. Unmanaged public endpoint  
C. Chỉ edge device, bất kể workload size  
D. Mỗi ngày dùng một cloud region khác nhau

### 19. NVIDIA platform nào liên quan trực tiếp nhất tới speech recognition, translation và text-to-speech?

A. NVIDIA Riva  
B. NVIDIA DCGM  
C. NVIDIA Base Command Manager  
D. NVIDIA NVSwitch

### 20. Sau khi cluster mở rộng, step time tăng và GPU thường chờ cả khi gradient synchronization lẫn dataset read. Hai vùng nào cần điều tra trước? **Chọn HAI.**

A. Compute-fabric topology và collective communication  
B. Màu của server bezel  
C. Số trang user documentation  
D. Storage/data-pipeline throughput  
E. Ngày hết hạn digital badge

### 21. Team muốn packaged, optimized model inference service qua standard API thay vì tự lắp từng serving component. Lựa chọn nào tốt nhất?

A. NVIDIA NIM microservice  
B. NVML  
C. UEFI  
D. SNMP trap

### 22. Research environment chủ yếu chạy queued, long-running, multi-node distributed training và cần priority, partition, reservation, backfill trưởng thành. Workload manager nào tự nhiên nhất?

A. Grafana  
B. Slurm  
C. TensorRT  
D. Redfish

### 23. Phát biểu nào về training và inference là đúng?

A. Inference thường update model weight bằng backpropagation sau mỗi request.  
B. Training không bao giờ hưởng lợi từ nhiều GPU.  
C. Training học weight; inference áp dụng weight đã học vào input mới.  
D. Inference luôn cần nhiều memory hơn training.

### 24. Lợi ích nào mô tả trực tiếp nhất vai trò của BlueField DPU trong data center?

A. Thay mọi GPU cho neural-network matrix operation  
B. Offload, accelerate và isolate networking, storage, security service  
C. Cung cấp public object-storage bucket  
D. Chỉ hoạt động như passive Ethernet cable

### 25. GPU utilization vẫn cao nhưng application throughput giảm khi temperature tăng. Clock thấp hơn loaded value bình thường. Giải thích có khả năng nhất là gì?

A. Model đã trở thành supervised learning.  
B. Thermal hoặc power throttling làm giảm effective performance.  
C. High allocation luôn đảm bảo maximum speed.  
D. NGC đã xóa container.

### 26. Hai record nào quan trọng nhất để reproduce một model-training run? **Chọn HAI.**

A. Dataset và model/code version  
B. Office seating plan  
C. Screen brightness của monitor  
D. Hyperparameter cùng framework/container/library version  
E. Tên người đầu tiên mở dashboard

### 27. Network oversubscription nghĩa là gì?

A. Mọi link có nhiều capacity hơn tất cả endpoint có thể dùng.  
B. Potential traffic từ downstream endpoint lớn hơn available uplink capacity.  
C. Toàn bộ GPU memory đã được allocate.  
D. Facility có hai utility feed.

### 28. Phát biểu nào định nghĩa deep learning tốt nhất?

A. Phương pháp không bao giờ dùng data  
B. Mọi rule-based application do con người viết  
C. Tập con của machine learning dựa trên multi-layer neural network  
D. Kỹ thuật làm mát data center

### 29. Training dataset gồm hàng triệu file rất nhỏ. Tổng capacity đủ nhưng worker mất nhiều thời gian mở và tìm file. Đặc tính storage nào cần chú ý nhất?

A. Chỉ GPU Tensor Core count  
B. Metadata performance và IOPS  
C. Monitor resolution  
D. Chỉ PUE

### 30. Phát biểu nào mô tả NVIDIA GPU Operator tốt nhất?

A. Thay Kubernetes bằng proprietary scheduler.  
B. Optimize trained model thành TensorRT engine.  
C. Tự động deployment/lifecycle của GPU software stack trên Kubernetes node.  
D. Là physical GPU-to-GPU link.

### 31. Library nào được thiết kế cho high-performance collective communication, ví dụ all-reduce, giữa NVIDIA GPU?

A. cuDNN  
B. NCCL  
C. Redfish  
D. Chỉ DOCA Flow

### 32. Startup cần 64 GPU trong ba tuần cho experiment, không có data center phù hợp và chưa biết nhu cầu sau experiment. Lựa chọn nào phù hợp nhất?

A. Xây private facility mới trước khi thử nghiệm.  
B. Dùng elastic cloud GPU capacity, sau khi kiểm tra quota/availability.  
C. Mua capacity mười năm bất kể utilization.  
D. Chạy workload trên BMC processor.

### 33. Cluster có tổng cộng tám GPU trống, nhưng job bốn GPU không start vì không có eligible node/topology domain chứa bốn GPU cùng loại. Đây là gì?

A. Quantization  
B. Fragmentation  
C. Backpropagation  
D. Data labeling

### 34. Workflow nào kết hợp inference optimization và serving đúng?

A. Dùng Grafana compile model, rồi Redfish serve.  
B. Dùng DCGM train model, rồi BMC batch request.  
C. Optimize bằng TensorRT và serve qua Triton Inference Server.  
D. Dùng NVLink version dataset và PUE deploy.

### 35. Một server được thêm GPU có tight interconnect qua supported NVLink/NVSwitch domain. Thuật ngữ nào mô tả thay đổi này?

A. Scale-up  
B. Chỉ scale-out qua Ethernet  
C. Out-of-band management  
D. Cloud bursting

### 36. NVIDIA technology family nào liên quan trực tiếp nhất đến intelligent video analytics và smart spaces?

A. NVIDIA Metropolis  
B. NVIDIA DCGM  
C. NVIDIA vGPU Manager  
D. NVIDIA Base Command Manager

### 37. Một accelerated rack mới có density cao hơn nhiều rack hiện có. Hai hạng mục nào phải được validate trước khi lắp? **Chọn HAI.**

A. Certification badge có in được không  
B. Electrical distribution và rack-level power capacity  
C. Mọi user có thích cùng text editor không  
D. Cooling capacity, airflow/liquid requirement và environmental limit  
E. Color theme của monitoring dashboard

### 38. Nhiều VM độc lập cần managed fractional access vào một supported physical GPU. Technology nào được thiết kế cho use case này?

A. Passthrough cùng một full GPU độc quyền cho mọi VM đồng thời  
B. NVIDIA vGPU với supported profile và licensing  
C. BMC serial console  
D. Chỉ GPUDirect Storage

### 39. Input distribution của deployed model thay đổi theo thời gian và quality giảm dù infrastructure health bình thường. Lifecycle practice nào xử lý rủi ro này?

A. Monitor data/model behavior và kích hoạt governed retraining hoặc rollback.  
B. Tăng rack power mà không thu model metric.  
C. Tắt versioning để đơn giản operations.  
D. Thay OOB switch.

### 40. Technology nào cung cấp RDMA semantic qua appropriately engineered Ethernet network?

A. RoCE  
B. SATA  
C. Redfish  
D. MIG

### 41. Tool/interface nào liên quan nhất tới remote power control và console access khi host OS không boot được?

A. TensorRT runtime  
B. BMC qua out-of-band management network  
C. Model registry  
D. Training optimizer state

### 42. Tổ hợp nào giải thích tốt nhất sự tăng tốc gần đây của AI capability và adoption?

A. Ít data hơn, network chậm hơn và bỏ software library  
B. Chỉ rule-based system và compute capacity giảm  
C. Chỉ office building lớn hơn  
D. Nhiều data, accelerated computing, algorithm advance, high-speed interconnect và optimized software

### 43. Phát biểu nào về liquid cooling cho dense AI system là chính xác nhất?

A. Loại bỏ toàn bộ facility planning.  
B. Có thể hỗ trợ heat density cao nhưng cần coolant infrastructure, leak detection và service planning.  
C. Giảm memory requirement của mọi GPU.  
D. Là inference compiler.

### 44. Một VM cần exclusive access tới toàn bộ physical GPU và không cần share với VM khác. Assignment method nào phù hợp nhất?

A. GPU passthrough  
B. Prometheus scraping  
C. Time slicing giữa nhiều VM  
D. Dynamic batching

### 45. Vì sao large AI cluster có thể tách compute-fabric traffic và storage-fabric traffic?

A. Để heavy storage I/O không làm gián đoạn unpredictable latency-sensitive training collective  
B. Để mọi cable dài bằng nhau bất kể topology  
C. Để không cần switch  
D. Để mọi storage đều local và ephemeral

### 46. CUDA application và user-space library được đóng gói trong container. Host vẫn cần gì?

A. Không cần GPU-related software nào  
B. Compatible NVIDIA driver và runtime path expose GPU vào container  
C. Một physical GPU riêng cho mỗi container layer  
D. TensorRT dù application không làm inference

### 47. Job chạy 100 phút trên một GPU và 30 phút trên bốn GPU. Four-GPU scaling efficiency xấp xỉ bao nhiêu?

A. 30%  
B. 75%  
C. 83%  
D. 120%

### 48. Host CPU bị tải nặng bởi software-defined networking, storage và security function, làm giảm cycle cho AI application. Bổ sung nào xử lý trực tiếp nhất?

A. Monitor lớn hơn  
B. Model registry thứ hai  
C. Thêm office printer  
D. BlueField DPU với supported DOCA-based offload

### 49. GPU liên tục xen kẽ giữa burst activity ngắn và idle gap dài trong training. Hai nguyên nhân nào cần kiểm tra trước khi thêm GPU? **Chọn HAI.**

A. CPU preprocessing/data-loader performance  
B. Font dùng trong training log  
C. Giá kỳ thi chứng chỉ  
D. Storage throughput và data availability  
E. Màu rack door

### 50. Enterprise giữ sensitive, steady-state training on-premises nhưng tạm dùng cloud GPU cho burst experiment. Đây là deployment model nào?

A. Chỉ scale-up  
B. Out-of-band management  
C. GPU passthrough  
D. Hybrid infrastructure

---

Dừng timer, ghi lại thời gian và chấm bằng [07-mock-exam-answer-key.md](./07-mock-exam-answer-key.md).

