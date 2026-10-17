# Inference performance: preliminary observations

## Measurements recorded during bring-up

| Metric | Recorded value | Important qualification |
|---|---:|---|
| Decode throughput | **16.22 tokens/s** | A single reported run in October 2026; not standardized |
| Approximate time per decoded token | 61.7 ms | Computed from the throughput above |
| Expert-cache hit rate | **95.54%** | Reported from a corresponding/nearby runtime observation |
| Other response rates | Approximately 15–17 tokens/s | Varied with prompt and cache conditions |
| Configured context | 8,192 tokens | Checked using the local model API |

### Limitations

These figures originate from contemporaneous build observations and the original note report. **Sanitized raw logs, exact launch arguments, the full quantization identifier, warm-up count, concurrent request count, benchmark prompts, and prefill throughput have not yet been collated for public reproducibility.**

These measurements were **not** produced by an unmodified Project Maya v1.1.14 build, nor should they be compared directly with official benchmark results.

The model is a roughly 320B-class **mixture-of-experts** model: total parameters are not all active for each generated token.

## Required data for a repeatable benchmark

- Measurement date; exact Git revision and local patch status
- Full model variant, conversion and quantization format
- PCIe link status and GPU/host-memory placement
- Prompt and generation lengths, warm-up passes, concurrent users
- Explicit distinction between prefill and decode throughput
- Actual wall-socket power, GPU temperatures and cooling configuration
- Trial count, minimum, median and maximum throughput

**A clean upstream build should be assessed in a separate environment without replacing the currently working Maya installation.**
