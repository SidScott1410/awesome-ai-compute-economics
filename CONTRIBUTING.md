# Contributing

Thanks. The bar is high on purpose; a short list people trust beats a long one they skim.

## What belongs

- Primary sources: the paper, the dataset, the benchmark repo, the company's own disclosure. Not the news article about it.
- Measured over claimed. A vendor slide with no recipe does not qualify. A benchmark with a rerunnable config does.
- Open over paywalled. Paywalled analysis is accepted only when it is the sole source of a number the field relies on, and the entry must say it is paywalled.
- On topic: cost, price, throughput, energy, traces, and the market structure around compute. General LLM papers go elsewhere.

## Entry format

One bullet, one line, under the section it fits:

```
- [Title](https://primary-url) (First author et al., venue year). What it is. Why it is here.
```

- Title as published. arXiv links use the `/abs/` form.
- Author and year so the reader can place it without clicking.
- Two short sentences max. State the contribution, then the reason it matters to compute economics.
- No adjectives like "seminal" or "groundbreaking". If it matters, the second sentence says why.

## Self-promotion

Allowed, with the same bar. Say in the PR that you are the author. Your entry must sit in the section where it belongs, not at the top.

## Process

1. Fork, add the line, open a PR.
2. CI checks that every link resolves.
3. A maintainer reads the source before merging. Expect a day or two.

## Removals

Dead links, superseded datasets, or benchmarks that stopped publishing are removed. Open an issue if you think something should go.
