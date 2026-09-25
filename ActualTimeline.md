# What does the timeline actually show?

As a list, the mechanisms look like a menu of alternatives. In date order they read as a sequence of bills. Vanilla attention (June 2017) was not wrong, it was expensive: compute that grows as n², a KV cache that grows with every token, and no built-in sense of position. Each later mechanism is someone paying less of one of those bills, and the order shows which bill mattered at each moment.

1. **Compute was attacked first, and mostly abandoned.** The first wave went after n²: fixed sparse patterns (Apr 2019), top-k (Dec 2019), sliding windows (Apr 2020) and linear attention (Jun 2020). Once FlashAttention (May 2022) made exact attention fast on GPUs, frontier models largely stopped approximating attention for training. Only windows survived, as one layer type among several.
2. **Memory was named early but paid for late.** MQA identified the decode-time KV bill in November 2019, but the field acted on it only when serving at scale made it the binding cost: GQA in May 2023, MLA in May 2024. The fixes move from sharing heads, to shrinking each entry, to storing fewer entries (DeepSeek-V4's CSA/HCA, April 2026).
3. **Length came in two waves.** The first wave was about how to represent position: relative positions (2018), then RoPE and ALiBi (2021). The second came once open LLaMA weights shipped with a 2K window and the question became how to stretch existing models. PI, NTK-aware scaling and YaRN all appeared within about ten weeks in mid-2023, and one of them came from a Reddit post, not a lab. DroPE (December 2025) then asks whether explicit position is needed at long range at all.
4. **Old ideas come back once their missing piece exists.** Top-k (2019) came back as DeepSeek Sparse Attention (2025) once a cheap indexer could choose the k. Linear attention (2020) came back as Gated DeltaNet (2024) once the delta rule and gating fixed its memory, and it returned as most of the layers in a hybrid, not as a replacement for attention. Hand-drawn sparse blocks (2019) came back as trained, GPU-shaped blocks in NSA (2025) and CSA (2026).
5. **Exactness is never fully given up.** Every long-context design keeps some exact path: a window of recent tokens, a few full-attention layers, or a selected set of real tokens. The argument is over how much exactness to keep, and where.

This matches the usual summary of the arc (exactness, then memory, then length, then memory again), with one addition that only the dates show: before memory there was a compute wave, and FlashAttention ended most of it.

**What comes next, reading the pattern.** At a million tokens the far past is either stored (linear growth) or pooled (lossy). I would expect the next work to be on deciding what to keep: learned, content-dependent compression and eviction, better selectors, and memory that crosses chunk boundaries. I would also expect position to become less explicit at long range, and the ratio of exact to cheap layers to be tuned per workload.

## Mechanisms beyond the usual list

| Mechanism | Date | Source for the date |
|---|---|---|
| Relative position representations (Shaw et al.; T5 bucketed bias, Oct 2019) | 6 Mar 2018 | arXiv 1803.02155, v1 submission history |
| Hybrid layer schedules (SRU++; later H3, Dec 2022, and Jamba, 28 Mar 2024) | 24 Feb 2021 | arXiv 2102.12459, v1 submission history |
| FlashAttention (IO-aware exact attention) | 27 May 2022 | arXiv 2205.14135, v1 submission history |
| Position interpolation (Meta) | 27 Jun 2023 | arXiv 2306.15595, v1 submission history |
| Native Sparse Attention (DeepSeek) | 16 Feb 2025 | arXiv 2502.11089, v1 submission history |
| Gated attention (output gate, Qwen) | 10 May 2025 | arXiv 2505.06708, v1 submission history |
| DeepSeek Sparse Attention (lightning indexer, V3.2-Exp) | 29 Sep 2025 | DeepSeek-V3.2-Exp release (DeepSeek API news, 29 Sep 2025) and the FlashMLA changelog entry "2025.09.29" |

## Dates worth flagging

- **Learned absolute positions come before sinusoidal.** They appear in ConvS2S (arXiv 1705.03122, 8 May 2017), a month before the Transformer paper (12 Jun 2017), which cites ConvS2S for them.
- **"Compressed and sparse attention as DeepSeek does it" covers three separate launches:** NSA (Feb 2025), DSA (Sep 2025) and CSA/HCA (Apr 2026). Some third-party write-ups credit CSA to V3.2; the V4 report defines CSA as compression followed by DSA.
- **The delta rule dates from 22 Feb 2021** (arXiv 2102.11174). The 2024 papers made it trainable in parallel and then added gating.
- **NTK-aware scaling has no paper.** It was a Reddit post from late June 2023, so its date is the weakest in the timeline, and the app says so.
