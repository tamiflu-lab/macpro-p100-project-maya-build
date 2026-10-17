# Hardware, auxiliary power, and cooling

These specifications describe the observed setup as of October 17, 2026, with additional details taken from the October 15 bring-up notes.

## Hardware

| Component | Configuration | Evidence / qualification |
|---|---|---|
| Workstation | Apple Mac Pro 2019, MacPro7,1 | Original build record |
| CPU | Intel Xeon W-3223 | Original build record |
| System memory | 96 GB installed | Approximately 94 GiB reported by the OS |
| GPU 0–3 | 4× NVIDIA Tesla P100-PCIE-16GB | All enumerated by `nvidia-smi` |
| GPU 4 | 1× NVIDIA Quadro GP100 16 GB | Added for display output; also used for inference |
| Compute capability | 6.0 on all five GPUs | `nvidia-smi` |
| Operating system | Ubuntu 24.04.5 LTS | `/etc/os-release` |
| NVIDIA driver | 580.178.04 | `nvidia-smi` |
| CUDA build environment | CUDA 12.8 | October 15 build record |
| Inference context | 8,192 tokens | Local model API response |

The GPUs have **80 GB of nominal device VRAM in total, distributed among five separate devices**. The memory is **not a single unified 80 GB allocation**. The system also uses host RAM in the inference engine's memory hierarchy.

## Power supplies

The system uses the **stock Mac Pro power supply plus an auxiliary external SFX PSU** to provide additional GPU power.

| GPU | Number of cards | Configured power limit per card; NOT measured consumption |
|---|---:|---:|
| Tesla P100 | 4 | 150 W |
| Quadro GP100 | 1 | 118 W |

**Electrical safety warning:** Using multiple power supplies, relay-based power sequencing, nonstandard adapters, or custom connectors can create electrical shock, overcurrent, fire, and hardware-damage hazards. This repository provides **no wiring schematic, PSU modification instructions, pinout, or electrical safety certification**. Any such system requires qualified review of power distribution, grounding, protection, and sequencing.

## Cooling and measurement limitations

- The configured GPU power limit is not the same as actual wall-socket power draw.
- PCIe Tesla P100 cards require sufficient cooling airflow.
- Comparable controlled measurements of temperatures, fans, sustained load, and system-wide power draw remain future work.
- This is not a manufacturer-supported standard hardware configuration.

## Read-only diagnostics example

```bash
nvidia-smi --query-gpu=index,name,memory.total,compute_cap,driver_version,power.limit --format=csv,noheader
cat /etc/os-release
```

Before publishing diagnostic output, review it for serial numbers, usernames, local host details, and other identifying information.
