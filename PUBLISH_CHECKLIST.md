# Pre-publication review checklist

This repository is currently **private**. Review these items before making it public or submitting an upstream report.

## Attribution and accuracy

- [ ] State clearly that this is an independent field report, **not an official Project Maya release or fork**.
- [ ] Credit and link to [Project Maya](https://github.com/mw00/project-maya) and the model's upstream developers.
- [ ] Distinguish the **October 15 local v1.1.9-based experiment** from the **October 16 public v1.1.14 Pascal release**.
- [ ] Make **no world-first or upstream priority claim**.
- [ ] Do not attribute our local measurements to an unmodified upstream v1.1.14 build.
- [ ] Do not redistribute proprietary code, upstream engine files, or model weights.
- [ ] Choose an appropriate license for original documentation if desired; no license is currently provided.

## Secrets and personal information

- [ ] Exclude API keys, access tokens, browser cookies, SSH keys, and NAS/SMB credentials.
- [ ] Exclude local IP addresses, MAC addresses, device serials, and personal home-directory paths.
- [ ] Do not publish unverified multi-PSU wiring instructions or relay pinouts.
- [ ] Compare photos with originals, especially model labels altered by AI-based processing.
- [ ] Check room details, receipts, screens, and account information visible in photos.

## Technical claims

- [ ] Hardware: **4× P100 + 1× GP100**, 96 GB system RAM, NVIDIA driver 580.178.04.
- [ ] Describe 80 GB as **nominal VRAM across separate devices**, not one unified GPU.
- [ ] Qualify **16.22 tokens/s** and **95.54% expert-cache hits** as single-setup observations.
- [ ] Describe CUDA `sm_60`, PCIe/BAR, and NextN/MTP changes as *system-specific* observations.
- [ ] Check the technical terms, version tags, dates, links, and English-language interpretation.

## Publication and upstream reporting

1. Confirm all README links and file contents on GitHub.
2. Add only reviewed photographs and sanitized excerpts when available.
3. Change the visibility from **Private** to **Public** only after an explicit owner review.
4. Consider submitting the draft in `UPSTREAM_ISSUE_DRAFT.md` as an upstream **Issue or Discussion**, once the report is ready.
5. Leave this repository's own Issues/Pull requests empty until there is an actual question, bug, or change proposal.

This repository documents experimental hardware bring-up and known limitations; it is not a general-purpose guide or official benchmark leaderboard.
