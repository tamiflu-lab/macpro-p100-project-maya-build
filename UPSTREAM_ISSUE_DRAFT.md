# Upstream issue draft — ready to submit manually

**Target:** https://github.com/mw00/project-maya/issues/new

**Status:** NOT SUBMITTED. GitHub integration lacks permission to create issues in the upstream repository. This file is an English draft for a user to review, copy, and submit from the GitHub website.

## Issue title

Tesla P100 (sm_60) field report: 4x P100 + Quadro GP100 on Mac Pro 2019 (locally modified v1.1.9)

## Issue body

Hi, and thanks for Project Maya! I wanted to share a real-hardware Pascal `sm_60` bring-up report, in case it is useful for the experimental GPU support work.

### Hardware and software
- **Host:** 2019 Mac Pro (MacPro7,1), Xeon W-3223, 96 GB RAM
- **GPUs:** 4 × Tesla P100 PCIe 16 GB + 1 × Quadro GP100 16 GB (also used for display); all five report **compute capability 6.0**
- **OS:** Ubuntu 24.04 LTS
- **Driver / build toolkit:** NVIDIA 580.178.04 / CUDA 12.8
- **Power:** stock Mac Pro PSU plus auxiliary SFX PSU
- **Model:** low-bit GLM-5.3-Flash-family model, ~320B MoE; context 8192

### Bring-up and observed results
On **October 15, 2026**, I managed to run local inference after adapting a working tree based on **Project Maya v1.1.9 (`4c9f1b1`)** with local `sm_60` CUDA compatibility changes. This predates the **public release** of v1.1.14 (October 16), which announced experimental Pascal support. **This is not a claim of priority over upstream or other contributors' work.**

The obstacles I encountered with the older local baseline included:
1. PCIe/BAR resource allocation with multiple GPUs; the working boot configuration uses `pci=realloc`.
2. CUDA kernels requiring `sm_60` compatibility fallbacks.
3. GPU/host-RAM memory tiering: nominally 80 GB VRAM across five **separate** devices, with additional host RAM used for model data.
4. NextN/MTP startup compatibility; the working configuration uses `STRATA_GLM_NO_MTP=1`.

The local server generated Japanese responses successfully. A recorded decode run was approximately **16.22 tokens/s**, with **95.54% expert-cache hits**; other observed runs were around **15–17 tokens/s**. These are **preliminary, non-standardized observations**, **not** benchmarks of unmodified v1.1.14 or results directly comparable to `./maya.sh --bench`.

I've published the build notes, hardware details, caveats, and chronology here:
https://github.com/tamiflu-lab/macpro-p100-project-maya-build

I noticed the Pascal P40 reports in #189 and #212. If a P100 benchmark would help, I can prepare sanitized measurements using the upstream benchmark procedure and test a **clean upstream version in a separate environment**, without replacing this working installation. Are there particular measurements or diagnostic outputs you would find most useful?

Thanks to the maintainers and contributors!

---

## Before submitting

- The report is a field observation from a locally modified v1.1.9 baseline, **not** a standard benchmark or test of unmodified v1.1.14.
- No "world-first" or priority claim is made.
- No secrets, internal IP addresses, raw logs, serial numbers, credentials, or unsafe wiring instructions are included.
- Only submit publicly if you are comfortable with the content; anyone can view an upstream issue.
