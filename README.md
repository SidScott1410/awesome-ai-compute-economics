# Awesome AI Compute Economics

A curated list on what a token costs to produce, what it sells for, and who captures the difference.

This is not another inference-systems list. For papers on kernels, schedulers and engines, use [Awesome-LLM-Inference](https://github.com/xlite-dev/Awesome-LLM-Inference) (5k+ entries and stars) or [Awesome-LLM-System-Papers](https://github.com/AmadeusChan/Awesome-LLM-System-Papers). For "how to lower my bill," use [awesome-llm-token-optimization](https://github.com/pleasedodisturb/awesome-llm-token-optimization). This list covers the layer those leave out: price formation, margins, measured cost, traces, energy, and the market structure between the chip and the invoice. Only the handful of systems papers that changed the economics are kept here.

Rules for inclusion: primary sources over commentary, measured over claimed, open over paywalled. Every entry says what it is and why it is here in one line. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Read these five first

1. [Inference Economics of Language Models](https://arxiv.org/abs/2506.04645): cost per token vs speed, from hardware constraints up. The roofline for the whole subject.
2. [Epoch AI: AI Inference Price Trends](https://epoch.ai/data-insights/ai-inference-price-trends): how fast a fixed capability gets cheaper, and how unevenly.
3. [LLM Inference Unveiled](https://arxiv.org/abs/2402.16363): why decode is memory-bound, which is why batch size is the whole game.
4. [InferenceX](https://inferencex.semianalysis.com/about): what cost per token looks like on real chips, with recipes.
5. [DeepSeek inference overview](https://github.com/deepseek-ai/open-infra-index/blob/main/202502OpenSourceWeek/day_6_one_more_thing_deepseekV3R1_inference_system_overview.md): the only full-day cost and revenue disclosure from a frontier lab.

## Contents

- [Foundations](#foundations)
- [Price trends and deflation](#price-trends-and-deflation)
- [Price formation and margins](#price-formation-and-margins)
- [Serving systems that changed the economics](#serving-systems-that-changed-the-economics)
- [Kernels and decoding](#kernels-and-decoding)
- [Benchmarks](#benchmarks)
- [Workload traces](#workload-traces)
- [Prices and cost tools](#prices-and-cost-tools)
- [Energy and carbon](#energy-and-carbon)
- [Business and market layer](#business-and-market-layer)
- [Measured cost on real silicon](#measured-cost-on-real-silicon)

## Foundations

Why compute cost is the variable that explains the rest.

- [Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361) (Kaplan et al., 2020). Loss as a power law in compute, data and parameters. The origin of compute as the budget unit.
- [Training Compute-Optimal Large Language Models](https://arxiv.org/abs/2203.15556) (Hoffmann et al., 2022, "Chinchilla"). For a fixed compute budget, smaller models on more tokens. Reset the cost calculus of the field.
- [The Hardware Lottery](https://arxiv.org/abs/2009.06489) (Hooker, 2020). Ideas win when they fit the hardware of the day. The argument for why serving cost is an architecture question, not only a chip question.
- [Compute Trends Across Three Eras of Machine Learning](https://arxiv.org/abs/2202.05924) (Sevilla et al., 2022). Training compute doubling times, measured across 120+ systems.
- [LLM Inference Unveiled: Survey and Roofline Model Insights](https://arxiv.org/abs/2402.16363) (Yuan et al., 2024). The roofline model applied to every stage of inference; shows why decode is memory-bound. Companion tool: [LLM-Viewer](https://github.com/hahnyuan/LLM-Viewer).

## Price trends and deflation

How fast a fixed unit of capability gets cheaper, and why your bill does not follow.

- [Welcome to LLMflation](https://a16z.com/llmflation-llm-inference-cost/) (Appenzeller, a16z, 2024). The 10x-per-year decline for equivalent-quality inference; the most-cited figure in the space.
- [Epoch AI: AI Inference Price Trends](https://epoch.ai/data-insights/ai-inference-price-trends). Price to reach a fixed benchmark score over time, per benchmark; the measured version of the LLMflation claim, with the spread across tasks shown rather than averaged.
- [The Price of Progress: Algorithmic Efficiency and the Falling Cost of AI Inference](https://arxiv.org/abs/2511.23455) (Gundlach et al., 2025). Separates the open-weight price trend from hardware price progress to estimate algorithmic progress in inference.
- [Epoch AI](https://epoch.ai/). Data on training compute, hardware trends and inference price trends, with methodology published. The reference dataset for compute growth.
- [State of AI: An Empirical 100 Trillion Token Study with OpenRouter](https://arxiv.org/abs/2601.10088) (Aubakirova et al., OpenRouter and a16z, 2026). Real paid traffic across 300+ models: open-weight share, reasoning share, input length growth. The demand side of token economics.

## Price formation and margins

The section no other list has: how asking prices relate to cost, and what sits between them.

- [Inference Economics of Language Models](https://arxiv.org/abs/2506.04645) (Erdil, Epoch, 2025). A model of cost per token against serial speed under arithmetic, memory, network and latency constraints, with Pareto frontiers for real models and a comparison to API providers. The closest thing to a roofline for inference economics. Code: [ege-erdil/inference-economics](https://github.com/ege-erdil/inference-economics).
- [Cost-of-Pass: An Economic Framework for Evaluating Language Models](https://arxiv.org/abs/2504.13359) (Erol et al., 2025). Expected dollars per correct answer, combining accuracy and price; the unit a buyer should compare models on.
- [Beyond Benchmarks: The Economics of AI Inference](https://arxiv.org/abs/2510.26136) (Zhuang et al., 2025). Inference as a production activity: diminishing marginal cost, returns to scale, an optimal cost-effectiveness zone.
- [llm-price-compass](https://github.com/arc53/llm-price-compass) (arc53). Cloud GPU benchmarks turned into per-token cost and set against API prices; the self-host vs API decision in one table. Prior art for measured-cost comparisons.
- [routers.vantage.sh](https://routers.vantage.sh/methodology) (Vantage). Estimated per-provider billings and effective prices on OpenRouter from observed token volumes; the demand side of the router market, with methodology and confidence per row.
- [LMC Price Convergence Index](https://lmmarketcap.com/pricing-history) (lmmarketcap). Weekly dispersion of frontier-tier prices around the median in log space, with confidence intervals and per-provider variance contributions.
- [token-margin](https://github.com/SidScott1410/token-margin) (Reckon Research). Pre-registered join of seller asking prices to measured decode and prefill timings on rented GPUs. Phase 1: two of four Llama 3.1 8B sellers price below the rented-GPU floor at any batch size; n=4, limits listed.
- [gwern: AI economics directory](https://gwern.net/doc/ai/scaling/economics/index). A personal bibliography of the scaling-economics literature; not maintained as a list, but the best single reading pile on the subject.

## Serving systems that changed the economics

Five papers, each of which moved the cost of a served token by a multiple. The rest live in the systems lists linked at the top.

- [Orca: A Distributed Serving System for Transformer-Based Generative Models](https://www.usenix.org/conference/osdi22/presentation/yu) (Yu et al., OSDI 2022). Iteration-level scheduling and selective batching. The start of continuous batching.
- [Efficient Memory Management for Large Language Model Serving with PagedAttention](https://arxiv.org/abs/2309.06180) (Kwon et al., SOSP 2023). KV-cache paging; the paper behind vLLM. Turned memory waste into throughput.
- [Splitwise: Efficient Generative LLM Inference Using Phase Splitting](https://arxiv.org/abs/2311.18677) (Patel et al., ISCA 2024). Prefill and decode on different machines, with a cost and power design study on production traces.
- [DistServe: Disaggregating Prefill and Decoding for Goodput-optimized LLM Serving](https://arxiv.org/abs/2401.09670) (Zhong et al., OSDI 2024). Goodput under TTFT and TPOT SLOs as the objective, not raw throughput.
- [Taming Throughput-Latency Tradeoff in LLM Inference with Sarathi-Serve](https://arxiv.org/abs/2403.02310) (Agrawal et al., OSDI 2024). Chunked prefill and stall-free batching. Code: [microsoft/sarathi-serve](https://github.com/microsoft/sarathi-serve).

## Kernels and decoding

Two ideas that changed what a memory pass is worth.

- [FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness](https://arxiv.org/abs/2205.14135) (Dao et al., NeurIPS 2022). Attention cost reframed as HBM reads and writes rather than FLOPs. The IO-aware lens the rest of this list assumes.
- [Fast Inference from Transformers via Speculative Decoding](https://arxiv.org/abs/2211.17192) (Leviathan et al., ICML 2023). Draft-then-verify; more tokens per memory pass without changing the output distribution.

## Benchmarks

Measured throughput, latency and cost, with recipes you can rerun.

- [InferenceX (formerly InferenceMAX)](https://inferencex.semianalysis.com/about) (SemiAnalysis). Continuous, open-source benchmark across NVIDIA and AMD accelerators and the major serving frameworks; Pareto curves of throughput against interactivity with cost per million tokens. Code: [SemiAnalysisAI/InferenceX](https://github.com/SemiAnalysisAI/InferenceX). Launch write-up: [InferenceMAX: Open Source Inference Benchmarking](https://newsletter.semianalysis.com/p/inferencemax-open-source-inference).
- [MLCommons MLPerf Inference](https://mlcommons.org/benchmarks/inference/). The vendor-submitted industry benchmark; useful for peak numbers and the gap between peak and served.
- [Artificial Analysis](https://artificialanalysis.ai/). Independent, continuous measurement of hosted API endpoints: price, output speed, TTFT, by provider. The buyer-side complement to InferenceX's chip-side curves.

## Workload traces

Real request mixes. Nothing about serving cost is meaningful without one.

- [Azure LLM Inference Traces](https://github.com/Azure/AzurePublicDataset) (Microsoft). Timestamp, input tokens, output tokens for two production services (2023 for Splitwise, 2024 for DynamoLLM). CC-BY.
- [BurstGPT](https://github.com/HPMLL/BurstGPT) (Wang et al., 2024). Real-world LLM serving trace with burstiness and failures, Azure-hosted. Paper: [arXiv 2401.17644](https://arxiv.org/abs/2401.17644).
- [Mooncake trace](https://github.com/kvcache-ai/Mooncake) (Moonshot AI). Arrival times, token counts and block hashes from Kimi, enough to study cache reuse.

## Prices and cost tools

Turning tokens into dollars, with provenance.

- [OpenRouter models API](https://openrouter.ai/api/v1/models). Public per-token prices for every listing on the router; the only machine-readable cross-provider price source. The 100-trillion-token study above draws on the same platform.
- [tokenprice](https://github.com/SidScott1410/tokenprice). Hourly git-backed feed of cross-provider token prices with mandatory source URL, read time and provenance; cross-provider spread per model. Python API and CLI.
- [agent-cost](https://github.com/SidScott1410/agent-cost). Prices an agent run step by step and reports the share of spend that never reached the user (retries, discards, SLO misses). Parses Claude Code sessions.
- [vLLM](https://github.com/vllm-project/vllm) and [SGLang](https://github.com/sgl-project/sglang). The two open serving engines most of the benchmarks above run on; their release notes are a running record of where throughput came from.
- [FinOps Foundation: FOCUS](https://focus.finops.org/). The open billing-data specification; the vocabulary cloud finance teams use for compute spend.

## Energy and carbon

The cost line that does not deflate with software.

- [Energy and Policy Considerations for Deep Learning in NLP](https://arxiv.org/abs/1906.02243) (Strubell et al., 2019). The paper that put training energy on the agenda.
- [Carbon Emissions and Large Neural Network Training](https://arxiv.org/abs/2104.10350) (Patterson et al., 2021). Measured, not estimated, energy for several large models; the 4M framework.
- [From Words to Watts: Benchmarking the Energy Costs of Large Language Model Inference](https://arxiv.org/abs/2310.03003) (Samsi et al., 2023). Measured energy per inference for Llama models across GPU types and batch sizes; the first systematic inference-energy benchmark.
- [Power Hungry Processing: Watts Driving the Cost of AI Deployment?](https://arxiv.org/abs/2311.16863) (Luccioni et al., 2023). Inference energy across tasks and model sizes; generation costs orders of magnitude more than classification.
- [ML.ENERGY Leaderboard](https://ml.energy/leaderboard) and [benchmark paper](https://arxiv.org/abs/2505.06371) (Chung et al., 2025). Measured GPU joules per request for open models by task, with the harness and data open; v3 adds B200 and reasoning models.
- [SPEC Power](https://www.spec.org/power/). The standard server power-efficiency benchmark; the baseline for watts-per-work before accelerators.

## Business and market layer

Who sits between the chip and the bill.

- [NVIDIA: Why Cost per Token Is the Metric for AI Factories](https://blogs.nvidia.com/blog/lowest-token-cost-ai-factories/) (2026). The vendor framing, and the one-line formula everyone now uses: cost per M tokens = GPU $/hour / (tokens per GPU-second × 3600) × 1e6. Read as a sales document with a useful equation in it.
- [DeepSeek-V3/R1 Inference System Overview](https://github.com/deepseek-ai/open-infra-index/blob/main/202502OpenSourceWeek/day_6_one_more_thing_deepseekV3R1_inference_system_overview.md) (DeepSeek, 2025). The only frontier lab to publish a full day of inference cost and theoretical revenue, with node counts and token throughput. Read the caveats: theoretical revenue at list price, web traffic free, night discounts.
- [InferenceX v2 write-up](https://newsletter.semianalysis.com/p/inferencex-v2-nvidia-blackwell-vs) (SemiAnalysis, 2026). Cost-per-token Pareto frontiers across Blackwell, Hopper and MI355X with disaggregated serving. The current public reference for silicon-level token economics.
- [State of AI (a16z summary)](https://a16z.com/state-of-ai/). The investor-side framing of the 100-trillion-token study: open-weight growth, routing, and what demand looks like at scale.

## Measured cost on real silicon

Per-token cost built from instrumented runs rather than spec sheets.

- [reckon](https://github.com/reckonresearch/reckon) (Reckon Research). Open harness and measured corpus for served-token cost: TTFT and TPOT per batch and context on L40S, A100, H100 PCIe and SXM, and MI300X, with provenance labels (measured, spec, configured, simulated, estimate) on every field.
- [mismatch-tax](https://github.com/SidScott1410/mismatch-tax) (Reckon Research). Pre-registered analysis on the reckon corpus: how far measured decode steps sit above the memory-bandwidth roofline, per silicon, and which configurations pay the most. Results and defect register are in the repo.

## Not yet listed, wanted

- A GPU rental price index with provenance (hourly H100/B200 spot across clouds and neoclouds). Several exist behind dashboards; none publish a feed with source URLs.
- Per-token energy on current silicon (Blackwell, MI355X) with the recipe.
- A first-party inference cost disclosure other than DeepSeek's.

## Contributing

Open a PR. One line per entry, primary source, say why it belongs. See [CONTRIBUTING.md](CONTRIBUTING.md). Links are checked weekly by CI.

## License

[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) for the list text. Linked works keep their own licenses.
