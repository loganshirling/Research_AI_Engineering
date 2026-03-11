---
title: Source - Quantization for LLM Inference - Core Sources
type: source
status: active
topic: LLM Inference and Serving
course: AI Engineering
tags: [source, quantization, inference, serving]
related:
  - "[[Topic - Quantization Tradeoffs for LLM Inference]]"
  - "[[Topic - Eval Design for Serving Changes]]"
sources:
  - "https://docs.vllm.ai/en/latest/features/quantization/"
  - "https://docs.vllm.ai/en/stable/features/quantization/fp8/"
  - "https://nvidia.github.io/TensorRT-LLM/reference/precision.html"
  - "https://nvidia.github.io/TensorRT-LLM/blogs/quantization-in-TRT-LLM.html"
  - "https://arxiv.org/abs/2306.00978"
  - "https://arxiv.org/abs/2210.17323"
  - "https://arxiv.org/abs/2211.10438"
  - "https://pytorch.org/blog/quantization-aware-training/"
last_updated: 2026-03-11
review_status: researched
priority: high
---

# Source - Quantization for LLM Inference - Core Sources

## Why this source bundle matters

This source note groups the highest-signal references for practical LLM inference quantization: official serving docs from vLLM and TensorRT-LLM, plus the main PTQ and QAT papers/posts that explain where common formats come from and when they fail.

## Core source set

### 1. vLLM quantization docs
- vLLM's quantization overview lists the formats and toolchains that the project currently supports, including AutoAWQ, GPTQModel, INT4 W4A16, INT8 W8A8, FP8 W8A8, quantized KV cache, and TorchAO.[^-vllm-quant]
- The FP8 page says vLLM supports FP8 weight and activation quantization with hardware acceleration, with official W8A8 support on Hopper and Ada Lovelace GPUs and W8A16 support on Turing/Ampere via Marlin kernels.[^-vllm-fp8]
- The same page states FP8 can reduce model memory by about 2x and improve throughput by up to 1.6x with minimal accuracy impact in supported deployments.[^-vllm-fp8]

### 2. TensorRT-LLM precision and selection guidance
- TensorRT-LLM's numerical precision reference explains the scaling modes it uses for quantization and defines weight-only schemes such as W4A16 and W8A16, where activations remain FP16 or BF16.[^-trtllm-precision]
- NVIDIA's quantization blog frames quantization as a response to memory-capacity, memory-bandwidth, and compute bottlenecks, and recommends trying FP8 first when it fits the hardware and use case, then INT8 SmoothQuant, then AWQ and GPTQ when lower-bit compression is needed.[^-trtllm-quant-blog]

### 3. AWQ, GPTQ, and SmoothQuant
- AWQ positions itself as a hardware-friendly low-bit weight-only quantization method that protects salient weights using activation statistics and avoids backpropagation or reconstruction.[^-awq]
- GPTQ presents a one-shot PTQ method based on approximate second-order information and reports strong 3–4 bit results in the paper.[^-gptq]
- SmoothQuant is the main W8A8 PTQ reference. It shifts activation outliers into weights through an equivalent offline transformation, enabling training-free W8A8 quantization and reporting up to 1.56x speedup and 2x memory reduction in its experiments.[^-smoothquant]

### 4. PyTorch QAT guidance
- PyTorch's QAT post is a useful practical reference for the "PTQ is close but not good enough" case. It shows a QAT flow for LLMs and reports recovering much of the accuracy degradation introduced by PTQ in the examples they benchmarked.[^-pytorch-qat]

## Durable takeaways

- Quantization is not one thing. The most important split is **weight-only** vs **weight-plus-activation** quantization.[^-trtllm-precision][^-smoothquant]
- Hardware support is a first-order decision variable. A format without mature kernels is usually not a real production option, no matter how good the paper sounds.[^-vllm-fp8][^-trtllm-precision]
- PTQ is the default place to start because it is cheaper operationally, but QAT becomes the next lever when PTQ misses the quality bar.[^-gptq][^-awqt][^-smoothquant][^-pytorch-qat]
- Quantization does not remove every inference bottleneck. When long context and concurrency dominate, KV cache pressure and scheduler behavior can still be the limiting factor.[^-vllm-quant]

## Caveats and limitations

- Vendor docs are strongest for implementation support matrices and deployment guidance, not for neutral cross-stack comparisons.[^-vllm-quant][^-trtllm-precision]
- Paper-reported throughput gains are workload-specific. They should not be treated as guaranteed production wins.[^-gptq][^-smoothquant][^-trtllm-quant-blog]
- The "best" format is not stable across all hardware generations, kernels, and latency constraints.[^-vllm-fp8][^-trtllm-quant-blog]

## Related notes

- [[Topic - Quantization Tradeoffs for LLM Inference]]
- [[Topic - Eval Design for Serving Changes]]

[^-vllm-quant]: vLLM docs, "Quantization." Last verified 2026-03-11. https://docs.vllm.ai/en/latest/features/quantization/
[^-vllm-fp8]: vLLM docs, "FP8 W8A8." Last verified 2026-03-11. https://docs.vllm.ai/en/stable/features/quantization/fp8/
[^-trtllm-precision]: TensorRT-LLM docs, "Numerical Precision." Last verified 2026-03-11. https://nvidia.github.io/TensorRT-LLM/reference/precision.html
[^-trtllm-quant-blog]: NVIDIA TensorRT-LLM blog, "Speed up inference with SOTA quantization techniques in TRT-LLM." Last verified 2026-03-11. https://nvidia.github.io/TensorRT-LLM/blogs/quantization-in-TRT-LLM.html
[^-awq]: Ji Lin et al., "AWQ: Activation-aware Weight Quantization for LLM Compression and Acceleration." MLSys 2024 / arXiv 2023. Last verified 2026-03-11. https://arxiv.org/abs/2306.00978
[^-gptq]: Elias Frantar et al., "GPTQ: Accurate Post-Training Quantization for Generative Pre-trained Transformers." ICLR 2023 / arXiv 2022. Last verified 2026-03-11. https://arxiv.org/abs/2210.17323
[^-smoothquant]: Guangxuan Xiao et al., "SmoothQuant: Accurate and Efficient Post-Training Quantization for Large Language Models." ICML 2023 / arXiv 2022. Last verified 2026-03-11. https://arxiv.org/abs/2211.10438
[^-pytorch-qat]: PyTorch Blog, "Quantization-Aware Training for Large Language Models." July 30, 2024. Last verified 2026-03-11. https://pytorch.org/blog/quantization-aware-training/
