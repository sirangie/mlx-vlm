[![Upload Python Package](https://github.com/Blaizzy/mlx-vlm/actions/workflows/python-publish.yml/badge.svg)](https://github.com/Blaizzy/mlx-vlm/actions/workflows/python-publish.yml)
# MLX-VLM

MLX-VLM is a package for inference and fine-tuning of Vision Language Models (VLMs) and Omni Models (VLMs with audio and video support) on your Mac using MLX.

> **Personal fork note:** I'm using this primarily for experimenting with Gemma 4 and Phi-4 Multimodal. If you stumbled here, the [upstream repo](https://github.com/Blaizzy/mlx-vlm) is the one you probably want.
>
> **My tested models:** Gemma 4 27B (4-bit), Phi-4 Multimodal — both working well on M3 Max 128GB.
>
> **Quick note on memory:** For Gemma 4 27B at 4-bit, I found setting `--max-tokens 2048` helps avoid OOM spikes during long generations. YMMV.
>
> **Phi-4 Multimodal tip:** When doing audio+image at the same time, pass the image first in the prompt — I got better results that way than interleaving.

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
  - [Command Line Interface (CLI)](#command-line-interface-cli)
    - [Thinking Budget](#thinking-budget)
  - [Speculative Decoding](#speculative-decoding)
    - [DFlash (Qwen3.5)](#dflash-qwen35)
    - [Gemma 4 MTP](#gemma-4-mtp)
  - [Chat UI with Gradio](#chat-ui-with-gradio)
  - [Python Script](#python-script)
  - [Server (FastAPI)](#server-fastapi)
    - [Continuous Batching](#continuous-batching)
    - [Automatic Prefix Caching (APC)](#automatic-prefix-caching-apc)
    - [KV Cache Quantization](#kv-cache-quantization)
- [Activation Quantization (CUDA)](#activation-quantization-cuda)
- [Multi-Image Chat Support](#multi-image-chat-support)
  - [Supported Models](#supported-models)
  - [Usage Examples](#usage-examples)
- [Model-Specific Documentation](#model-specific-documentation)
- [Vision Feature Caching](#vision-feature-caching)
- [TurboQuant KV Cache](#turboquant-kv-cache)
- [Distributed Inference](#distributed-inference)
- [Fine-tuning](#fine-tuning)

## Model-Specific Documentation

Some models have detailed documentation with prompt formats, examples, and best practices:

| Model | Documentation |
|-------|---------------|
| DeepSeek-OCR | [Docs](https://github.com/Blaizzy/mlx-vlm/blob/main/mlx_vlm/models/deepseekocr/README.md) |
| DeepSeek-OCR-2 | [Docs](https://github.com/Blaizzy/mlx-vlm/blob/main/mlx_vlm/models/deepseekocr_2/README.md) |
| DOTS-OCR | [Docs](https://github.com/Blaizzy/mlx-vlm/blob/main/mlx_vlm/models/dots_ocr/README.md) |
| DOTS-MOCR | [Docs](https://github.com/Blaizzy/mlx-vlm/blob/main/mlx_vlm/models/dots_ocr/README.md) |
| GLM-OCR | [Docs](https://github.com/Blaizzy/mlx-vlm/blob/main/mlx_vlm/models/glm_ocr/README.md) |
| Phi-4 Reasoning Vision | [Docs](https://github.com/Blaizzy/mlx-vlm/blob/main/mlx_vlm/models/phi4_siglip/README.md) |
| MiniCPM-o | [Docs](https://github.com/Blaizzy/mlx-vlm/blob/main/mlx_vlm/models/minicpmo/README.md) |
| Phi-4 Multimodal | [Docs](https://github.com/Blaizzy/mlx-vlm/blob/main/mlx_vlm/models/phi4mm/README.md) |
| MolmoPoint | [Docs](https://github.com/Blaizzy/mlx-vlm/blob/main/mlx_vlm/models/molmo_point/README.md) |
