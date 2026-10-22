# NVIDIA-Certified Associate: AI Infrastructure and Operations

## Bộ tài liệu ôn tập độc lập — Tiếng Việt

Bộ tài liệu này giúp chuẩn bị cho kỳ thi **NVIDIA-Certified Associate: AI Infrastructure and Operations (NCA-AIIO)**. Cấu trúc bám theo study guide tháng 1/2026 và exam blueprint hiện hành của NVIDIA.

> Đây là tài liệu ôn tập độc lập, không được NVIDIA bảo trợ và không chứa câu hỏi bị rò rỉ hoặc sao chép từ đề thi thật. Các thuật ngữ kỹ thuật quan trọng được giữ bằng tiếng Anh vì kỳ thi sử dụng tiếng Anh.

## Thông tin kỳ thi

| Hạng mục | Thông tin chính thức |
|---|---|
| Cấp độ | Associate |
| Số câu | 50 |
| Thời gian | 60 phút |
| Ngôn ngữ | Tiếng Anh |
| Kiến thức đầu vào | Hiểu biết cơ bản về hạ tầng data center |
| Thời hạn chứng chỉ | Hai năm |

### Blueprint và phân bố câu hỏi mô phỏng

| Domain | Trọng số chính thức | Số câu trong mock exam |
|---|---:|---:|
| Essential AI Knowledge | 38% | 19 |
| AI Infrastructure | 40% | 20 |
| AI Operations | 22% | 11 |
| **Tổng** | **100%** | **50** |

## Danh sách tài liệu

1. [01-essential-ai-knowledge.md](./01-essential-ai-knowledge.md) — AI/ML/DL, training và inference, CPU/GPU, NVIDIA software stack, vòng đời AI, giải pháp và use case.
2. [02-ai-infrastructure.md](./02-ai-infrastructure.md) — Hardware sizing, scale-up/scale-out, cluster, storage, networking, facility, power/cooling, cloud/on-premises và DPU.
3. [03-ai-operations.md](./03-ai-operations.md) — Monitoring, GPU metrics, Kubernetes/Slurm, scheduling, troubleshooting, MIG, vGPU và passthrough.
4. [04-exam-cram-sheet.md](./04-exam-cram-sheet.md) — Bản tổng hợp cô đọng để ôn nhanh trước kỳ thi.
5. [05-four-week-study-plan.md](./05-four-week-study-plan.md) — Kế hoạch ôn tập trong bốn tuần.
6. [06-mock-exam-questions.md](./06-mock-exam-questions.md) — Đề thi thử 50 câu trong 60 phút, đúng tỷ lệ blueprint.
7. [07-mock-exam-answer-key.md](./07-mock-exam-answer-key.md) — Đáp án, giải thích, objective mapping và hướng dẫn đánh giá điểm.
8. [SOURCES.md](./SOURCES.md) — Nguồn chính thức dùng để đối chiếu nội dung.

## Phương pháp học khuyến nghị

Học theo bốn vòng:

1. **Understand — Hiểu:** đọc từng chương, tập trung vào quan hệ nguyên nhân–kết quả. Tự giải thích lại mỗi bảng bằng lời của mình.
2. **Retrieve — Gợi nhớ:** đóng tài liệu và trả lời checklist từ trí nhớ. Chỉ mở lại sau khi đã đưa ra câu trả lời.
3. **Apply — Áp dụng:** với mỗi công nghệ, hãy hỏi: *Nó giải quyết vấn đề gì? Chạy ở layer nào? Khi nào nên dùng giải pháp khác?*
4. **Simulate — Mô phỏng:** làm mock exam closed-book trong 60 phút, sau đó đọc giải thích cho cả câu sai và câu đoán đúng.

Nên tạo một error log gồm bốn cột: `Question`, `Why I missed it`, `Correct rule`, `Trigger words`. Làm lại các objective bị sai sau 48 giờ.

## Kỹ thuật làm bài

- Trung bình chỉ có khoảng **72 giây mỗi câu**. Vòng đầu trả lời câu chắc chắn và đánh dấu câu khó.
- Đọc câu cuối trước để xác định đề đang hỏi về *metric*, *component*, *architecture*, *benefit* hay *next action*.
- Chú ý các từ khóa như **lowest latency**, **highest isolation**, **elastic**, **deterministic**, **offload** và **out-of-band**.
- Với câu **Choose TWO**, đánh giá từng lựa chọn độc lập.
- Chọn giải pháp xử lý đúng bottleneck. Thêm GPU không sửa được storage chậm, network congestion, CPU preprocessing, power cap hoặc scheduling kém.
- Loại đáp án hoạt động sai layer. Ví dụ: TensorRT tối ưu inference; DCGM giám sát GPU; DPU offload hạ tầng; scheduler phân bổ job.

## Tiêu chí sẵn sàng

Bạn nên bước vào giai đoạn ôn cuối khi có thể:

- Đạt **80% trở lên** trong mock exam với giới hạn 60 phút.
- Đạt ít nhất **75% ở từng domain**.
- Giải thích được mọi mục trong cram sheet mà không nhìn tài liệu.
- Chẩn đoán utilization thấp dựa trên evidence thay vì lập tức thêm GPU.

## Nguồn chính thức nên bắt đầu

- [Trang chứng chỉ NCA-AIIO](https://www.nvidia.com/en-us/learn/certification/ai-infrastructure-operations-associate/)
- [Official NCA-AIIO Exam Study Guide](https://dam-cdn.nvd.orangelogic.com/AssetLink/x874j05hy3m3r2sor84kpvp70750m468.pdf)
- [NVIDIA AI Enterprise documentation](https://docs.nvidia.com/ai-enterprise/index.html)
- [NVIDIA technical documentation hub](https://docs.nvidia.com/)

