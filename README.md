# Attention, in order

An interactive explainer of attention mechanisms, arranged **chronologically by first public appearance** (2014 → 2026). It starts with scaled dot-product attention as a step-by-step widget (Q×Kᵀ → scale → mask → softmax → ×V). After that, each mechanism is presented as a response to a cost left by the ones before it.

Every card answers the same questions: what existed, what problem people hit, the mechanism, **what it buys, what it gives up, and when you would actually choose it**, plus what it left for the next paper.

## Links

- **Live link:** https://attention-in-order-2026.netlify.app/
- **GitHub repository:** https://github.com/kunal2016/attention-in-order

## Run / deploy

It is a single static file with no build step: `site/index.html`.

- **Netlify:** drag the `site/` folder onto app.netlify.com/drop, or connect the repo (`netlify.toml` sets the publish directory to `site`).
- **Vercel:** `vercel deploy site` or import the repo and set the output directory to `site`.
- **GitHub Pages:** serve the `site/` folder.
- **Locally:** open `site/index.html` in a browser.

`page.html` is the same page without the `<html>/<head>/<body>` wrapper (used for the Claude artifact preview).

## Interactive parts (each one demonstrates a single idea)

| Widget | What it shows |
|---|---|
| Pipeline (top) | Why ÷√d_k exists: at d_k = 256 without scaling, softmax goes one-hot. Why the mask is a table of −∞. |
| Priority map | Mechanisms plotted by date and by the problem they attacked, so you can see the field's priorities shift. |
| Mask grids | Dense, strided, top-k, sliding window, sinks, NSA, DSA, CSA/HCA patterns side by side. |
| Associative-memory demo | Linear attention vs the delta rule vs Gated DeltaNet when a key is overwritten. |
| RoPE frequency chart | How PI, NTK-aware and YaRN each rescale RoPE wavelengths (drag the scale factor). |
| KV-cache calculator | MHA vs GQA vs MQA vs MLA cache for a chosen model shape, context length and number of active users. Defaults (48 layers, 8 KV heads, head dim 128, bf16, 32K) give 6.44 GB per user for GQA. |
| Layer-schedule strip | How the share of attention layers in a hybrid stack sets the KV cache. |
| Per-card verdicts | Which bill each mechanism pays down, and whether it fits a 2K chatbot versus a 1M-token agent. |

## What the timeline shows

Vanilla attention was not wrong; it was expensive. It sends two bills (compute that grows as n², a KV cache that grows with every token) and needs position supplied from outside. In date order:

- **Compute was attacked first, and mostly abandoned.** 2019–20 brought fixed sparse patterns, top-k, sliding windows and linear attention. After FlashAttention (May 2022) made exact attention fast, frontier models largely stopped approximating attention for training; only windows survived as a layer type.
- **Memory was named in 2019 but paid for in 2023–24.** MQA (Nov 2019) identified the decode-time KV bill; the field acted when serving at scale made it binding: GQA (May 2023), MLA (May 2024), then fewer stored entries with CSA (Apr 2026).
- **Length came in two waves.** Representing position (relative positions 2018, RoPE and ALiBi 2021), then stretching existing weights once LLaMA shipped with a 2K window: PI, NTK-aware and YaRN within about ten weeks in 2023, one of them from a Reddit post. DroPE (Dec 2025) questions explicit position at long range.
- **Old ideas return once their missing piece exists.** Top-k (2019) → DSA (2025) with a cheap indexer; linear attention (2020) → Gated DeltaNet (2024) with delta writes and gating, used as most layers of a hybrid; fixed sparse blocks (2019) → trained blocks in NSA and CSA.
- **Exactness is never fully given up.** Every long-context design keeps an exact path: a recent window, a few attention layers, or selected real tokens.
- **What the pattern suggests next** (a reading, not a forecast): deciding what to keep of the far past (learned compression and eviction, better selectors, memory across chunks), less explicit position at long range, and the ratio of exact to cheap layers tuned per workload.

## Chronology and sources

Unless noted, the date is the **arXiv v1 submission date**, taken from the arXiv abstract page's submission history.

| Date | Mechanism | Primary source | Note |
|---|---|---|---|
| 2014-09-01 | Additive attention (prologue) | [arXiv 1409.0473](https://arxiv.org/abs/1409.0473) | |
| 2017-05-08 | Learned absolute position embeddings | [arXiv 1705.03122](https://arxiv.org/abs/1705.03122) (ConvS2S) | Before the Transformer; the Transformer paper cites ConvS2S for learned positions. |
| 2017-06-12 | Scaled dot-product, multi-head attention | [arXiv 1706.03762](https://arxiv.org/abs/1706.03762) | |
| 2017-06-12 | Sinusoidal positions | [arXiv 1706.03762](https://arxiv.org/abs/1706.03762) §3.5 | |
| 2018-03-06 | Relative position representations *(added)* | [arXiv 1803.02155](https://arxiv.org/abs/1803.02155) | T5's bucketed bias ([arXiv 1910.10683](https://arxiv.org/abs/1910.10683), Oct 2019) is described on the same card. |
| 2019-04-23 | Sparse Transformer (fixed sparse patterns) | [arXiv 1904.10509](https://arxiv.org/abs/1904.10509) | The arXiv page didn't show its history to my tools; the date is from an arXiv mirror (alphaxiv). |
| 2019-11-06 | Multi-query attention (MQA) | [arXiv 1911.02150](https://arxiv.org/abs/1911.02150) | |
| 2019-12-25 | Top-k sparse attention | [arXiv 1912.11637](https://arxiv.org/abs/1912.11637) | |
| 2020-04-10 | Sliding-window attention (Longformer) | [arXiv 2004.05150](https://arxiv.org/abs/2004.05150) | Earlier local windows: Image Transformer ([1802.05751](https://arxiv.org/abs/1802.05751), Feb 2018) and the Sparse Transformer's local band. |
| 2020-06-29 | Linear attention | [arXiv 2006.16236](https://arxiv.org/abs/2006.16236) | Earlier origin: Shen et al. ([1812.01243](https://arxiv.org/abs/1812.01243), Dec 2018). |
| 2021-02-22 | Delta rule for linear attention | [arXiv 2102.11174](https://arxiv.org/abs/2102.11174) | Parallel training came later: [arXiv 2406.06484](https://arxiv.org/abs/2406.06484), 2024-06-10. |
| 2021-02-24 | Hybrid layer schedules *(added)* | [arXiv 2102.12459](https://arxiv.org/abs/2102.12459) (SRU++) | Followed by Gated State Spaces ([2206.13947](https://arxiv.org/abs/2206.13947), Jun 2022), H3 ([2212.14052](https://arxiv.org/abs/2212.14052), Dec 2022) and Jamba ([2403.19887](https://arxiv.org/abs/2403.19887), 2024-03-28). |
| 2021-04-20 | RoPE | [arXiv 2104.09864](https://arxiv.org/abs/2104.09864) | |
| 2021-08-27 | ALiBi | [arXiv 2108.12409](https://arxiv.org/abs/2108.12409) | |
| 2022-05-27 | FlashAttention *(added)* | [arXiv 2205.14135](https://arxiv.org/abs/2205.14135) | Builds on online softmax ([1805.02867](https://arxiv.org/abs/1805.02867), May 2018) and Rabe & Staats ([2112.05682](https://arxiv.org/abs/2112.05682), Dec 2021). |
| 2023-05-22 | Grouped-query attention (GQA) | [arXiv 2305.13245](https://arxiv.org/abs/2305.13245) | |
| 2023-06-27 | Position interpolation *(added)* | [arXiv 2306.15595](https://arxiv.org/abs/2306.15595) | kaiokendev's [SuperHOT post](https://kaiokendev.github.io/til#extending-context-to-8k) did the same thing concurrently. |
| 2023-06-29 | NTK-aware scaled RoPE | [Reddit r/LocalLLaMA post by bloc97](https://www.reddit.com/r/LocalLLaMA/comments/14lz7j5/ntkaware_scaled_rope_allows_llama_models_to_have/) | No paper. Reddit needs sign-in, so the day is from archived copies and citing sources. Upper bound: a [TGI issue](https://github.com/huggingface/text-generation-inference/issues/512) linking the post was opened 30 Jun 2023. |
| 2023-08-31 | YaRN | [arXiv 2309.00071](https://arxiv.org/abs/2309.00071) | |
| 2023-09-29 | Attention sinks (StreamingLLM) | [arXiv 2309.17453](https://arxiv.org/abs/2309.17453) | Related: Evan Miller, [Attention Is Off By One](https://www.evanmiller.org/attention-is-off-by-one.html) (Jul 2023). |
| 2024-05-07 | Multi-head latent attention (MLA) | [arXiv 2405.04434](https://arxiv.org/abs/2405.04434) (DeepSeek-V2) | Introduced inside a model paper. |
| 2024-12-09 | Gated DeltaNet | [arXiv 2412.06464](https://arxiv.org/abs/2412.06464) | |
| 2025-02-16 | Native Sparse Attention *(added)* | [arXiv 2502.11089](https://arxiv.org/abs/2502.11089) | |
| 2025-05-10 | Gated attention *(added)* | [arXiv 2505.06708](https://arxiv.org/abs/2505.06708) | |
| 2025-09-29 | DeepSeek Sparse Attention *(added)* | [DeepSeek-V3.2-Exp on Hugging Face](https://huggingface.co/deepseek-ai/DeepSeek-V3.2-Exp); [FlashMLA changelog "2025.09.29"](https://github.com/deepseek-ai/FlashMLA) | Release date; no arXiv v1 at launch. |
| 2025-12-13 | DroPE | [arXiv 2512.12167](https://arxiv.org/abs/2512.12167) | Card checked against the full paper, including Table 2 and the recalibration setup (0.5%–20% of pretraining across runs). |
| 2026-04-24 | CSA + HCA (DeepSeek-V4) | [DeepSeek API news, V4 Preview](https://api-docs.deepseek.com/news/news260424/); report [arXiv 2606.19348](https://arxiv.org/abs/2606.19348) (submitted 2026-04-26, despite the 2606 prefix) | Release date used. Partial RoPE (64 of 512 dims), compression ratios (two uncompressed layers, then alternating 4/128), window 128 and index top-k 512 are from the [V4-Flash config.json](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash/blob/main/config.json). |

### Places where the order is easy to get wrong

- **Learned positions came before sinusoidal.** ConvS2S (May 2017) predates the Transformer (June 2017).
- **"DeepSeek sparse attention" covers three separate launches:** NSA (Feb 2025, paper), DSA (Sep 2025, V3.2-Exp), CSA/HCA (Apr 2026, V4). Some third-party write-ups say V3.2 introduced CSA; the V4 report defines CSA as compression followed by DSA.
- **DroPE (Dec 2025) predates CSA/HCA (Apr 2026)**, so it is not the newest item.
- **The delta rule is from 2021**; 2024 brought the parallel training algorithm and then Gated DeltaNet.
- **Several mechanisms have earlier origins than the paper usually credited.** Cards name them: linear attention (Shen 2018), FlashAttention's online softmax (2018) and linear-memory attention (Dec 2021), local windows (Image Transformer 2018), hybrids (SRU++ 2021 before H3).
- **NTK-aware scaling has no paper**, so it has the weakest date source in the list, and the page says so.

### Mechanisms added beyond the usual list

Relative position representations, hybrid layer schedules, FlashAttention, position interpolation, NSA, gated attention and DSA. Each one follows the same card format (original date, motivation, mechanism, advantage, cost, place in the timeline). Additive attention (2014) is included as a prologue.

## Review

Before publishing, the page was checked by two independent reviewers: one reviewed it as a reader would, the other re-verified every date and technical claim against primary sources. Their corrections are applied (V4 layer layout, DroPE budget range, Falcon's KV heads, FlashAttention's final normalization, ALiBi's wording, earlier-origin credits).

## Accuracy notes

- Performance numbers (for example "93.3% smaller KV cache" or "22.2× faster") are the original authors' claims, quoted from their abstracts.
- Values in the widgets are random or illustrative, and are labelled that way on the page. The KV calculator uses the standard formula `2 × layers × KV heads × head_dim × tokens × bytes`; for MLA it uses DeepSeek-V2's 512 + 64 dims per token per layer.
- Dates were checked on the arXiv abstract pages (submission history, v1) or on the official release page listed above in September 2026.
