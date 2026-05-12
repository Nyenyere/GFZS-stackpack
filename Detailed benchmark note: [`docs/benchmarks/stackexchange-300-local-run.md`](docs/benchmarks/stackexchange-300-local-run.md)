# Benchmark: 300-file StackExchange-style local prototype run

This benchmark documents a completed local GFZS StackPack prototype run on a mixed StackExchange-style XML archive sample.

The result is a public, sanitized benchmark summary. Raw logs, full internal reports, exact tuning parameters, candidate scoring details, stream layout information, and implementation details are intentionally not published in this repository.

GFZS StackPack is currently a research prototype. These numbers should be treated as early prototype results until independently reproduced and compared against additional archive baselines.

## Summary

| Metric | Value |
|---|---:|
| Source files | 300 |
| StackExchange-style archive sources | 288 |
| Supported structured XML members processed | 2,297 |
| Generic/auxiliary fallback records | 12 |
| Original input size | 2,952.35 MB |
| GFZS output size | 1,984.76 MB |
| Size reduction | 967.59 MB |
| Size reduction percentage | ~32.8% |
| Original source archive → GFZS ratio | ~1.49× |
| Expanded structured XML processed | 15,649.32 MB |
| Expanded XML → GFZS pack ratio | ~7.88× |
| StackExchange-style sources handled through searchable structured path | 288 / 288 |
| StackExchange-style source searchability in this run | 100% |
| Raw StackExchange-style fallback records | 0 |
| Search smoke test | OK |
| SHA-based verification | OK |
| Approximate runtime | ~11.7 hours |

## What was tested

The input directory contained 300 source files, mostly StackExchange-style archive inputs.

GFZS StackPack inspected the archive contents, identified recurring structured XML member families, grouped compatible members across many sources, and stored them through a structure-aware container path before backend compression.

The run produced a single GFZS pack from the mixed input directory.

The benchmark was executed as a local prototype run, not as a controlled laboratory benchmark.

## Main result

The run reduced:

```text
2,952.35 MB source archive set
→ 1,984.76 MB GFZS pack
