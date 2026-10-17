# GitHub Issue draft for `mw00/project-maya`

> **Do not post automatically.** Review the content and choose an appropriate issue/discussion category on the upstream repository after creating your GitHub account. This is a **field report**, not a claim of a bug in current upstream or a benchmark for v1.1.14.

**Suggested title:** Pre-v1.1.14 Pascal field report: GLM-5.3-Flash on 4× Tesla P100 + Quadro GP100 (sm_60)

---

Hello, and thank you for Project Maya!

I wanted to share a real-world Pascal multi-GPU field report. On **October 15, 2026**, while working from the **v1.1.9** baseline, I independently experimented with CUDA `sm_60` compatibility and managed to bring up inference on Tesla P100 / Quadro GP100. I noticed that official **v1.1.14**, with experimental Pascal support, was released on **October 16**. This is **not** a priority claim over other contributors or an assertion that the patches are different; I am sharing the hardware findings from my own bring-up. I have a 2019 Mac Pro (MacPro7,1) running Ubuntu 24.04, with the following GPUs:

- 4 × NVIDIA Tesla P100 PCIe 16 GB
- 1 × NVIDIA Quadro GP100 16 GB (also used for display output)
- All five GPUs report CUDA compute capability 6.0
- Intel Xeon W-3223, 96 GB RAM
- NVIDIA driver 580.178.04; CUDA 12.8 build environment

I was able to run a low-bit-quantized GLM-5.3-Flash-family model locally and obtain Japanese responses from the inference server. I recorded approximately **16.22 tokens/s decode** and **95.54% expert cache hit rate** for one measurement; other generations were roughly in the **15–17 tokens/s** range. The configured context was 8,192 tokens. These are observational numbers, **not yet a standardized benchmark**.

**Very important version distinction:** My working installation is **based on Project Maya v1.1.9 (`4c9f1b1`) with local CUDA `sm_60` compatibility modifications**. I understand that v1.1.14 introduced experimental Pascal support. I have **not** independently verified these results on a clean unmodified v1.1.14 build, and I do not want to imply that the numbers are for the official release.

Issues encountered in the older local baseline included:

1. Multi-GPU PCIe/BAR resource assignment (the working Ubuntu configuration includes `pci=realloc`).
2. CUDA compatibility of certain kernels with Pascal `sm_60`.
3. Splitting work and model data across five GPUs and host RAM, as the model files exceeded available total device VRAM.
4. A NextN/MTP startup compatibility issue; I temporarily used `STRATA_GLM_NO_MTP=1` in the local build.

The build log and original Japanese article are summarized here:

https://note.com/tamiflu_lab/n/ndbda115333f9

I am preparing a separate repository with hardware details and notes. I would be happy to share **sanitized**, reproducible benchmark results after testing a clean upstream version in an isolated environment. Is there a preferred benchmark command / set of measurements for Pascal reports?

Thanks again to the maintainers and contributors!

---

### Before posting this Issue

- [ ] Verify the version and commit information against the PC.
- [ ] Confirm model quantization and whether to disclose it.
- [ ] Add a reproducible benchmark using the upstream project's preferred command, if available.
- [ ] Confirm that any image labels were not altered by AI editing.
- [ ] Do not attach whole server logs, environment files, account credentials, internal IP addresses, or system serial numbers.
- [ ] Check whether the project prefers Issues, Discussions, or benchmark contribution formats.