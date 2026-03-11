---
title: Topic - Quantization Tradeoffs for LLM Inference
type: topic
status: active
topic: LLM Inference and Serving
course: AI Engineering
tags: [quantization, inference, serving]
related:
  - "[[Map - AI Engineering]]"
  - "[[Topic - LLM Inference Path, KV Cache, and Batching]]"
  - "[[Source - Quantization for LLM Inference - Core Sources]]"
sources:
  - "https://docs.vllm.ai/en/latest/features/quantization/"
  - "https://docs.vllm.ai/en/stable/features/quantization/fp8/"
  - "https://nvidia.github.io/TensorRT-LLM/reference/precision.html"
  - "https://nvidia.github.io/TensorRT-LLM/_sources/blogs/quantization-in-TRT-LLM.md.txt"
  - "https://arxiv.org/abs/2306.00978"
  - "https://arxiv.org/abs/2210.17323"
  - "https://arxiv.org/abs/2211.10438"
  - "https://pytorch.org/blog/quantization-aware-training/"
last_updated: 2026-03-11
review_status: researched
priority: high
core_status: core
---

# Topic - Quantization Tradeoffs for LLM Inference

## Why it matters

Quantization trades precision for smaller memory footprint, which can make larger models runnable and can improve throughput, but the size of the win depends on format support, kernels, hardware, and workload shape.[^vllm-quant][^vllm-fp8][^tllm-precision]

## 80/20 summary

- Weight-only quantization is usually the safest first move because it reduces model-weight memory while leaving activations in FP16 or BF16.[^tllm-precision]
- W8A8 and FP8-style methods can produce stronger memory and throughput gains, but only when the stack and hardware support them well.[^vllm-fp8][^smoothquant]
- AWQ and GPTQ are low-bit post-training methods for weights; SmoothQuant is the key source for practical W8A8 PTQ; QAT matters when PTQ misses your quality bar.[^awq][^gptq][^smoothquant][^pytorch-qat]

## Main buckets

### Weight-only

TensorRT-LLM defines INT4 and INT8 weight-only methods as W4A16 and W8A16, where weights are quantized and activations remain FP16 or BF16.[^tllm-precision] This is often the easiest path to memory reduction and a good default starting point.

### AWQ and GPTQ

AWQ protects salient weights using activation information and avoids backpropagation or reconstruction.[^awq] GPTQ is a one-shot PTQ method that uses approximate second-order information and reports strong 3–4 bit results in the paper.[^gptq]

### W8A8 and SmoothQuant

SmoothQuant is important because it makes W8A8 PTQ more viable by shifting activation outliers into weights through an equivalent offline transformation.[^smoothquant]

### FP8

vLLM’s FP8 page says W8A8 is officially supported on Hopper and Ada Lovelace GPUs, while Turing/Ampere support W8A16 weight-only FP8 with Marlin kernels.[^vllm-fp8] It also says FP8 can reduce model memory by about 2x and improve throughput by up to 1.6x in supported deployments.[^vllm-fp8]

TensorRT-LLM’s guidance is explicit: try FP8 first if it works for your hardware and use case, then experiment with INT8 SmoothQuant, then AWQ and GPTQ if needed.[^tllm-choose]

## Practical tradeoffs

- **Hardware support matters first.** A format without mature kernels is not a real serving option.[^vllm-fp8][^tllm-precision]
- **Workload shape matters.** Decode-heavy traffic can benefit more from weight compression; long-context traffic may still be limited by KV cache pressure.[^tllm-choose]
- **Quality tolerance matters.** Lower precision can shift task quality even when memory and throughput improve.[^awq][^gptq][^smoothquant]
- **PTQ first, QAT later.** PyTorch’s QAT post is most useful as evidence that QAT is the next lever when PTQ is close but not good enough.[^pytorch-qat]

## Common mistakes

1. Assuming 4-bit is always faster.
2. Measuring only one synthetic throughput number.
3. Ignoring scheduler and KV-cache bottlenecks.
4. Treating paper gains as guaranteed production gains.[^tllm-choose][^awq][^gptq][^smoothquant]

## What to read next

- [[Topic - Eval Design for Serving Changes]]
- [[Topic - LLM Inference Path, KV Cache, and Batching]]
- [[Source - Quantization for LLM Inference - Core Sources]]

[^vllm-quant]: vLLM docs, “Quantization.” Last verified 2026-03-11. https://docs.vllm.ai/en/latest/features/quantization/
[^vllm-fp8]: vLLM docs, “FP8 W8A8.” Last verified 2026-03-11. https://docs.vllm.ai/en/stable/features/quantization/fp8/
[^tllm-precision]: TensorRT-LLM docs, “Numerical Precision.” Last verified 2026-03-11. https://nvidia.github.io/TensorRT-LLM/reference/precision.html
[^tllm-choose]: TensorRT-LLM blog source, “Quantization in TensorRT-LLM.” Last verified 2026-03-11. https://nvidia.github.io/TensorRT-LLM/_sources/blogs/quantization-in-TRT-LLM.md.txt
[^awq]: Ji Lin et al., “AWQ: Activation-aware Weight Quantization for LLM Compression and Acceleration.” MLSys 2024 / arXiv 2023. Last verified 2026-03-11. https://arxiv.org/abs/2306.00978
[^gptq]: Elias Frantar et al., “GPTQ: Accurate Post-Training Quantization for Generative Pre-trained Transformers.” ICLR 2023 / arXiv 2022. Last verified 2026-03-11. https://arxiv.org/abs/2210.17323
[^smoothquant]: Guangxuan Xiao et al., “SmoothQuant: Accurate and Efficient Post-Training Quantization for Large Language Models.” ICML 2023 / arXiv 2022. Last verified 2026-03-11. https://arxiv.org/abs/2211.10438
[^pytorch-qat]: PyTorch Blog, “Quantization-Aware Training for Large Language Models.” July 30, 2024. Last verified 2026-03-11. https://pytorch.org/blog/quantization-aware-training/
