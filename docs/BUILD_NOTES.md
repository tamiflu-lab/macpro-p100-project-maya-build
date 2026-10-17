# Build notes and Pascal compatibility troubleshooting

This is an **observational field report**, not an official setup guide or instructions for modifying power supplies.

## Version context

On **October 15, 2026**, inference ran on **4× Tesla P100 PCIe 16 GB and 1× Quadro GP100** using a **locally modified Project Maya v1.1.9** tree. Upstream v1.1.14 announced experimental Pascal support in its **October 16 public release**. This document does not claim priority over upstream development work. See the [timeline and scope](CHRONOLOGY.md).

## 1. Multi-GPU PCIe / BAR resource allocation

Installing several older GPUs caused difficulties allocating PCIe resources and enumerating all devices. The investigation included bringing the system up with fewer cards installed and checking detection incrementally. The ultimately working Ubuntu boot configuration included `pci=realloc`.

- **Observed result:** All five cards were enumerated by `nvidia-smi`.
- **Not established:** That the same parameter is necessary or appropriate on other Mac Pros, firmware versions, or systems.
- **Caution:** Altering bootloader/kernel parameters can make a system unbootable. Have a recovery method available.

## 2. CUDA Pascal compute capability `sm_60`

Both Tesla P100 PCIe and Quadro GP100 report **compute capability 6.0**. The working Project Maya tree is based on **v1.1.9, commit `4c9f1b1`**, and contains uncommitted local changes affecting:

- CMake configuration,
- the Python startup/inference code,
- multiple CUDA kernels in `src/kernels/cuda/`, and
- local compatibility code such as `include/strata/kernels/pascal_compat.cuh`.

Some older-baseline kernels depended on instructions unavailable on these `sm_60` devices. The work explored alternate compatibility implementations and rebuilt the code for Pascal. **The unreviewed local source patches are not included in this report.**

Upstream [v1.1.14](https://github.com/mw00/project-maya/releases/tag/v1.1.14) later announced experimental Pascal support. Detailed source-diff analysis and reproducible testing on a clean release remain outstanding. The running installation has **not** been overwritten with a new release.

## 3. Host RAM and multi-GPU memory tiers

The model data in the build notes was roughly **92 GiB**. The five GPUs have a total of 80 GB nominal VRAM across separate devices, which is not enough for an all-GPU placement of that model data. The engine uses system RAM as well.

An example of the recorded layer allocation:

| Device | Model layer range |
|---|---|
| Tesla P100 #1 | 0–11 |
| Tesla P100 #2 | 12–22 |
| Tesla P100 #3 | 23–32 |
| Tesla P100 #4 | 33–40 |
| Quadro GP100 | 41–44 |

This table does **not** imply that all mixture-of-experts weights resided on the GPUs. Expert caching and placement depend on the model, quantization, and runtime.

## 4. NextN / MTP compatibility

An early startup issue involved NextN weights and their handling by the then-current model conversion / engine. During troubleshooting, the working configuration used `STRATA_GLM_NO_MTP=1` to disable an auxiliary prediction path and allow inference using the primary model. This workaround may be unnecessary in newer upstream releases.

## 5. Work still needed for reproducibility

1. Compare the modified historical version against upstream in **separate test environments**.
2. Record the exact model variant, quantization, and hashes.
3. Fix benchmark prompts, input/output token counts, warm-up runs, and concurrency.
4. Measure prefill and decode rates separately.
5. Publish sanitized results including GPU tiering, temperatures, and actual system power.

Related pages: [hardware](HARDWARE.md) and [benchmark notes](BENCHMARKS.md).
