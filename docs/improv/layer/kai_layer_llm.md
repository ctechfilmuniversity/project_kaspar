---
layout: default
title: Layer LLM
parent: Live Rehearsal Improvisation
nav_order: 5
---



# LLM

**Authors:** Philip Gerdes

File Change History:

| Date       | Change       | Author |
| ---------- | ------------ | ------ |
| 2026-04-22 | Benchmarking | Philip |





#### LLM Benchmarking Setup

All benchmarks were run with the following parameters:

- Single GPU → `NVIDIA GeForce RTX 4090 (24 GB)`
- OS → `WSL (Ubuntu Distro)`
- Python Environment Management → `uv`
- Referenz-Datensatz: `FreedomIntelligence/sharegpt-deutsch`

Time to First Token (TTFT)  

| N = 10 queries   | vLLM    | SGLang |
| ---------------- | ------- | ------ |
| Mean TTFT (ms)   | 125.68  | 66.53  |
| Median TTFT (ms) | 84.61   | 70.08  |
| P99 TTFT (ms)    | 427.94* | 84.02  |

| N = 6200 queries | vLLM  | SGLang |
| ---------------- | ----- | ------ |
| Mean TTFT (ms)   | 34.57 | 48.74  |
| Median TTFT (ms) | 29.84 | 42.73  |
| P99 TTFT (ms)    | 79.27 | 153.23 |

Time per Output Token   (TPOT)  

| N = 10 queries   | vLLM  | SGLang |
| ---------------- | ----- | ------ |
| Mean TPOT (ms)   | 14.30 | 12.19  |
| Median TPOT (ms) | 14.27 | 12.23  |
| P99 TPOT (ms)    | 14.45 | 12.27  |

| N = 6200 queries | vLLM  | SGLang |
| ---------------- | ----- | ------ |
| Mean TPOT (ms)   | 14.32 | 12.17  |
| Median TPOT (ms) | 14.00 | 12.11  |
| P99 TPOT (ms)    | 20.80 | 12.85  |


Results

* TTFT is decisive for K.ai (latency minimization)
  * Focus on N = 6200 requests
    * Possible measurement error at N = 10 [*outlier on slide 3]
    * Longer runtime, so the figure may also reflect fatigue effects from thermal buildup
* Throughput (TPOT) is `~ 6 ms` faster with `SGLang`
* BUT the first token (start of response) is `~ 52 ms` faster with `vLLM`