# Case study: GLM-5.3-Flash on 4× Tesla P100 + 1× Quadro GP100 in a 2019 Mac Pro

[Hardware](docs/HARDWARE.md) | [Chronology](docs/CHRONOLOGY.md) | [Build notes](docs/BUILD_NOTES.md) | [Benchmark notes](docs/BENCHMARKS.md) | [Upstream issue draft](UPSTREAM_ISSUE_DRAFT.md)

> **Independent community field report, not an official Project Maya release or fork.** This repository documents an actual five-GPU Pascal system and its integration problems. Full credits for Project Maya/Strata belong to its upstream maintainers at [mw00/project-maya](https://github.com/mw00/project-maya).

## Why this report matters: independent pre-release Pascal bring-up

On **October 15, 2026**, I got inference working on four Tesla P100s and one Quadro GP100 by adapting a local tree based on **Project Maya v1.1.9** (released October 14) for Pascal `sm_60`. The upstream release **v1.1.14**, which publicly introduced **experimental** support for compute capability 6.x, followed on **October 16**.

**This is a chronology of my own working setup, not a claim to have been the first person to implement Pascal support** or to have preceded upstream contributors' internal work or pull requests. The only chronology established here is that my recorded local test preceded the **public v1.1.14 release**. The working code was independently adapted from an older baseline; I have not established which patches overlap with upstream changes.

See [Chronology and scope of evidence](docs/CHRONOLOGY.md).

## Summary

I ran a **GLM-5.3-Flash-family ~320B MoE model** using a **2019 Mac Pro (MacPro7,1)** equipped with **four 16 GB Tesla P100 PCIe GPUs** and **one 16 GB Quadro GP100**. The GP100 was added for display output and also participates in inference. All five cards are Pascal **compute capability 6.0** devices.

The purpose was to investigate the cost/performance trade-off of used GPUs with comparatively affordable VRAM capacity—not to demonstrate a universally cheap, turnkey, power-efficient, or officially supported setup.

## Hardware and software

| Component | Configuration |
|---|---|
| Computer | Apple Mac Pro 2019, MacPro7,1 |
| CPU | Intel Xeon W-3223 |
| RAM | 96 GB installed (~94 GiB reported by OS) |
| GPUs | 4× NVIDIA Tesla P100 PCIe 16 GB + 1× Quadro GP100 16 GB |
| Nominal GPU VRAM | 80 GB total **across independent devices**, not a unified 80 GB allocation |
| GPU architecture | Pascal, compute capability 6.0 |
| OS | Ubuntu 24.04 LTS |
| NVIDIA driver | 580.178.04 |
| CUDA build environment | CUDA 12.8, with locally adapted `sm_60` compatibility changes |
| Power | Stock Mac Pro PSU plus an auxiliary external SFX PSU |
| Inference engine | **Locally modified Project Maya / Strata based on v1.1.9 (`4c9f1b1`)** |
| Model | Low-bit quantized GLM-5.3-Flash-family model; roughly 92 GiB of model data in the original build notes |
| Configured context | 8,192 tokens |

## Observed results (not a standardized benchmark)

- A recorded decode measurement: **16.22 tokens/s**.
- Recorded expert-cache hit rate: **95.54%** for that measurement.
- Other responses varied around **15–17 tokens/s**.
- A Japanese-language response was successfully obtained from the local inference server.

These figures are from **one particular machine and setup**. Input length, exact model variant, quantization, cache warming, concurrency and other measurement conditions are not yet fully standardized. Raw logs and a reproducible benchmark procedure need to be prepared. **Do not compare the numbers directly with the upstream benchmark table.**

## What required work

- **PCIe/BAR resource allocation**: Multiple GPUs were initially difficult to enumerate. The working Ubuntu boot configuration includes `pci=realloc`; this is a system-specific workaround, not a universal recommendation.
- **CUDA `sm_60`**: Locally adapted CUDA compatibility handling was needed with the older engine baseline.
- **GPU/RAM memory tiers**: The model's weight data exceeds total installed GPU VRAM, and GPU VRAM is not pooled into a single memory allocation. Host RAM is also used.
- **NextN/MTP**: The working configuration used `STRATA_GLM_NO_MTP=1` to bypass an incompatibility observed with the then-current engine/model conversion.
- **Power and cooling**: An auxiliary SFX PSU supplements the stock Mac Pro PSU; safe wiring instructions are deliberately not published.

## Important upstream-version distinction

The experimental Pascal support was released upstream on October 16, **after** this machine's October 15 bring-up on a locally modified v1.1.9 tree. This does **not** establish priority over contributors' work or unmerged patches.

The **tested working tree is based on Project Maya v1.1.9 with local changes**. Upstream [v1.1.14](https://github.com/mw00/project-maya/releases/tag/v1.1.14) subsequently introduced **experimental Pascal support**, explicitly including the Tesla P40/P100 families. **These results are not a validation of unmodified v1.1.14.** There is no assurance that patches developed for the old baseline are necessary, correct, or safe with the newer upstream code.

This repository intentionally does not redistribute the upstream engine or model weights.

## Links

- [Project Maya (upstream)](https://github.com/mw00/project-maya)
- [Pascal experimental support release v1.1.14](https://github.com/mw00/project-maya/releases/tag/v1.1.14)
- [GLM-5.3-Flash](https://huggingface.co/zai-org/GLM-5.3-Flash)
- [Original Japanese build report on note](https://note.com/tamiflu_lab/n/ndbda115333f9)

## Next steps

Gather scrubbed raw benchmark logs and the exact quantization/build parameters, test a clean upstream version **in an isolated environment**, and perform systematic comparisons of decode/prefill, power consumption, and long-duration stability. Photographs will only be added after verifying labels and removing identifying details.

## Issues and pull requests

This repository currently serves as a **build and experimental field report**, not a standalone implementation of Project Maya. It is normal for its **Issues** and **Pull requests** tabs to be empty. Use Issues for questions, bugs or follow-up tasks, and Pull requests for proposed changes to the files. A potential report to the **upstream** Project Maya maintainers has been drafted in [UPSTREAM_ISSUE_DRAFT.md](UPSTREAM_ISSUE_DRAFT.md); **it has not been submitted**. This repository is private pending final review, so external maintainers cannot access its contents yet.