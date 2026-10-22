# NVIDIA-Certified Associate: AI Infrastructure and Operations

## Independent Study Pack

This study pack is designed for the **NVIDIA-Certified Associate: AI Infrastructure and Operations (NCA-AIIO)** exam. It follows the official January 2026 study guide and the current NVIDIA exam blueprint.

> This is an independent study resource. It is not endorsed by NVIDIA and does not contain leaked or copied exam questions. Product capabilities change; verify version-specific details in the current NVIDIA documentation.

## Official Exam Snapshot

| Item | Official detail |
|---|---|
| Level | Associate |
| Questions | 50 |
| Time | 60 minutes |
| Language | English |
| Prerequisite | Basic understanding of data center infrastructure |
| Certification validity | Two years |

### Blueprint and Mock-Exam Allocation

| Domain | Official weight | Questions in this pack's 50-question mock exam |
|---|---:|---:|
| Essential AI Knowledge | 38% | 19 |
| AI Infrastructure | 40% | 20 |
| AI Operations | 22% | 11 |
| **Total** | **100%** | **50** |

## Files

1. [01-essential-ai-knowledge.md](./01-essential-ai-knowledge.md) — AI/ML/DL, training versus inference, CPU/GPU, the NVIDIA software stack, lifecycle, solutions, and use cases.
2. [02-ai-infrastructure.md](./02-ai-infrastructure.md) — Hardware sizing, scale-up/scale-out, cluster design, storage, networking, facilities, power/cooling, cloud/on-premises, and DPUs.
3. [03-ai-operations.md](./03-ai-operations.md) — Management, telemetry, GPU metrics, Kubernetes/Slurm scheduling, troubleshooting, MIG, vGPU, and passthrough.
4. [04-exam-cram-sheet.md](./04-exam-cram-sheet.md) — High-density final review and common exam traps.
5. [05-four-week-study-plan.md](./05-four-week-study-plan.md) — A practical preparation schedule.
6. [06-mock-exam-questions.md](./06-mock-exam-questions.md) — A 50-question, 60-minute practice exam with the official domain distribution.
7. [07-mock-exam-answer-key.md](./07-mock-exam-answer-key.md) — Answers, explanations, objective mappings, and score interpretation.
8. [SOURCES.md](./SOURCES.md) — Official blueprint and primary NVIDIA documentation used for verification.

## Recommended Study Method

Use four passes:

1. **Understand:** Read each chapter without memorizing. Explain each diagram or table aloud in your own words.
2. **Retrieve:** Close the file and answer the checkpoint questions from memory. Reopen it only after committing to an answer.
3. **Apply:** For every technology, ask: *What problem does it solve? Where does it run? What would I choose instead?*
4. **Simulate:** Take the mock exam closed-book in 60 minutes, then review every explanation—including questions you guessed correctly.

Create a small error log with four columns: `Question`, `Why I missed it`, `Correct rule`, and `Trigger words`. Re-test only the missed objectives 48 hours later.

## Exam Technique

- Budget about **72 seconds per question**. On the first pass, answer obvious questions and flag uncertain ones.
- Read the last sentence first: determine whether the question asks for a *metric*, *component*, *architecture*, *benefit*, or *next action*.
- Treat words such as **lowest latency**, **highest isolation**, **elastic**, **deterministic**, **offload**, and **out-of-band** as design constraints.
- For “choose TWO” questions, evaluate every option independently. Do not assume two similar options must form a pair.
- Prefer the answer that solves the stated bottleneck. More GPUs do not fix slow storage, network congestion, CPU preprocessing, a power cap, or poor scheduling.
- Eliminate options that operate at the wrong layer. For example, TensorRT optimizes inference; DCGM monitors data center GPUs; a DPU offloads infrastructure services; a scheduler places jobs.

## Readiness Gate

You are likely ready for a final review when you can:

- Score **80% or higher** on the mock exam under the 60-minute limit.
- Score at least **75% in every domain**, not only overall.
- Explain every item on the cram sheet without notes.
- Diagnose a utilization problem using evidence rather than immediately adding GPUs.

## Official Starting Points

- [NCA-AIIO certification page](https://www.nvidia.com/en-us/learn/certification/ai-infrastructure-operations-associate/)
- [Official NCA-AIIO exam study guide (PDF)](https://dam-cdn.nvd.orangelogic.com/AssetLink/x874j05hy3m3r2sor84kpvp70750m468.pdf)
- [NVIDIA AI Enterprise documentation](https://docs.nvidia.com/ai-enterprise/index.html)
- [NVIDIA technical documentation hub](https://docs.nvidia.com/)

