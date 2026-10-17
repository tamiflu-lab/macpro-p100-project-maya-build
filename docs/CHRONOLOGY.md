# Timeline and scope of the pre-release claim

This report documents a **locally modified Project Maya v1.1.9 build running inference on a real Tesla P100 / Quadro GP100 system on October 15, 2026**, before the **public release** of v1.1.14 on October 16, which announced experimental Pascal compute capability 6.x support.

| Date (2026) | Event | Evidence / scope |
|---|---|---|
| October 14 | Project Maya **v1.1.9** published | [Upstream release](https://github.com/mw00/project-maya/releases/tag/v1.1.9); local working baseline commit `4c9f1b1` |
| **October 15** | Locally adapted CUDA `sm_60` compatibility on the v1.1.9 working tree; brought up **4× Tesla P100 + 1× Quadro GP100** for inference | Contemporary build/initial-run logs and modified source files on the machine; raw logs are not published |
| October 16 | Project Maya **v1.1.14** publicly released with experimental Pascal 6.x support | [Upstream release](https://github.com/mw00/project-maya/releases/tag/v1.1.14) |
| October 17 | Published the Japanese hardware/build write-up | [note article](https://note.com/tamiflu_lab/n/ndbda115333f9) |

## What this establishes

- The author's **October 15 hardware bring-up predates the public v1.1.14 release on October 16**.
- The working tree was based on **v1.1.9 with uncommitted local compatibility changes**.
- Actual hardware troubleshooting involved CUDA `sm_60`, PCIe resource allocation, NextN/MTP handling, and GPU/host-RAM memory tiers.

## What it does not establish

- A "world first" implementation, or priority over the upstream maintainers or other contributors.
- Priority over an upstream pull request, unpublished branch, or someone else's private testing.
- That identical performance was achieved with unmodified upstream v1.1.14.
- That the local patches are different from, better than, or still needed alongside upstream experimental support.

**The chronological comparison is limited to publicly published releases, not development start dates.** As of this report, a clean v1.1.14 build has not been independently benchmarked on this specific hardware.

Sources: [v1.1.9](https://github.com/mw00/project-maya/releases/tag/v1.1.9), [v1.1.14](https://github.com/mw00/project-maya/releases/tag/v1.1.14), [original note article](https://note.com/tamiflu_lab/n/ndbda115333f9).
