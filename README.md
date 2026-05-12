# GFZS StackPack

Experimental searchable, lossless, SHA-verifiable archive pipeline for structured text datasets.

This repository is a public project brief and benchmark disclosure for GFZS StackPack.  
The implementation is not currently open source.

## Overview

GFZS StackPack is an experimental archive/container pipeline for structured text datasets.

The current prototype focuses on large collections of structured text archives, with StackExchange-style XML dump archives used as the first major test corpus. GFZS explores whether structured archives can be made smaller, searchable, and verifiable by preprocessing their internal structure before handing the resulting streams to mature backend compressors.

GFZS is not intended to replace compressors such as zstd, xz, 7z, or similar tools. Instead, it acts as a structure-aware preprocessing and container layer in front of existing compression backends.

The main goal is not only smaller output. The goal is a more useful archive format for structured corpora:

- lossless reconstruction of supported structured members
- SHA-based verification
- searchable container contents
- corpus-aware organization
- deterministic preprocessing
- backend-friendly packed streams
- safe fallback handling for unsupported inputs

## Current prototype capabilities

The current prototype has demonstrated the following capabilities on StackExchange-style XML archive collections:

- Lossless reconstruction of supported XML members
- SHA-256 verification for tested records
- Search directly inside the GFZS container
- Multi-source corpus-aware packing
- Structure-aware grouping of recurring XML member types
- Deterministic preprocessing for XML-like table data
- Backend-compressed container output
- Generic/raw fallback for unsupported or non-target records
- One-touch local Windows prototype workflow

The implementation is still experimental. Format details, internal record layout, tuning parameters, and search/index behavior may change between prototype versions.

This README documents observed prototype behavior and benchmark results. It is not a full format specification.

## Benchmark snapshot: 300-file StackExchange sample

The following benchmark comes from a completed local prototype run, not from a theoretical estimate.

The run used the current GFZS StackPack prototype on a mixed directory containing 300 source files, mostly StackExchange archive inputs. The resulting GFZS pack completed SHA verification for the tested records and passed a basic container search smoke test.

### Configuration summary

The exact low-level tuning profile is intentionally summarized here because the prototype format and profile selection logic are still evolving.

- Backend: zstd-based high-compression backend
- Compression profile: experimental high-compression prototype profile
- Input type: mostly StackExchange `.7z` archive sources
- Processing mode: structure-aware corpus packing
- Verification: SHA-256 based record verification
- Search: prototype container search smoke test

### Results

| Metric | Value |
|---|---:|
| Source files | 300 |
| StackExchange archive sources | 288 |
| Generic/auxiliary fallback records | 12 |
| Original input size | 2952.35 MB |
| GFZS output size | 1984.76 MB |
| Size reduction | 967.59 MB |
| Size reduction percentage | ~32.8% |
| Expanded structured XML processed | 15649.32 MB |
| Expanded XML → GFZS pack ratio | ~7.88× |
| SHA verification | OK |
| Search smoke test | OK |
| Approximate runtime | ~11.7 hours |

In this run, GFZS reduced a 2.95 GB source archive set to a 1.98 GB searchable and SHA-verifiable GFZS pack, while internally processing about 15.65 GB of expanded structured XML data.

These numbers are early prototype results and should be treated as experimental until reproduced independently and compared against additional baseline archive strategies.

## Benchmark context

This benchmark was not produced on a tuned workstation.

The 300-file run was executed on a Lenovo Legion Y520 laptop (sry, but that's all I have, lol) with 16 GB RAM, using a HDD-backed working/output path, while other desktop and background tasks were active.

The runtime should therefore be interpreted as a rough prototype measurement on ordinary shared consumer hardware, not as a best-case performance result.

GFZS currently prioritizes correctness, verification, searchability, and archive structure over maximum speed.

## What GFZS does differently

Traditional archive workflows usually compress input files directly, either as individual files or as a folder-level byte stream.

GFZS takes a different approach for supported structured corpora. It looks inside archive inputs, identifies recurring structured member types, and stores related data in deterministic streams that are more suitable for later compression and search.

For StackExchange-style XML dumps, this means GFZS can recognize repeated XML member categories across many archives and pack them in a corpus-aware way before backend compression.

The result is an archive that can preserve supported content losslessly while also enabling prototype-level search and verification features.

## Verification model

GFZS is designed around deterministic reconstruction and verification.

For supported structured members, GFZS aims to reconstruct the original member content losslessly and verify it using SHA-based checks.

For unsupported inputs or records outside the current structured pipeline, GFZS uses generic/raw fallback paths.

Important distinction:

GFZS currently focuses on semantic/member-level lossless reconstruction of supported archive contents. It does not necessarily reconstruct the original source archive containers byte-for-byte.

In other words, the goal is to preserve and verify the contained data, not necessarily to reproduce the original `.7z` container bytes exactly.

## Search model

GFZS includes prototype-level search directly inside the generated container.

The current search implementation should be treated as a smoke-tested capability, not as a fully optimized search engine.

Future work may include:

- better search indexing
- partial extraction workflows
- faster lookup paths
- richer metadata listing
- selective reconstruction of matching records or member groups

## Current status

GFZS StackPack is a research prototype.

The current implementation has been tested primarily on StackExchange-style XML dump archives and related structured/text-like datasets. Broader datasets, independent reproduction, and stronger baseline comparisons are still needed.

The project is currently focused on answering this question:

Can structured archive collections become smaller, searchable, and verifiable at the same time if their internal structure is used before compression?

The current prototype results suggest that structure-aware preprocessing can provide meaningful benefits on StackExchange-style XML archive collections.

## Limitations

- GFZS is an experimental prototype, not a production-ready archive format.
- The current benchmark has not yet been independently reproduced.
- Current comparisons are primarily against the original source archive set.
- Full baseline comparisons against folder-level `7z`, `tar.zst`, `xz`, and other archive strategies are still needed.
- GFZS currently focuses on supported structured members, especially StackExchange-style XML data.
- It does not necessarily reconstruct original source archive containers byte-for-byte.
- Unsupported or non-target inputs may be stored through generic/raw fallback paths.
- Runtime can be high in maximum-compression prototype modes.
- Some preprocessing strategies may improve intermediate representation size without always improving final compressed size.
- The search implementation is currently prototype-level.
- Format details and internal record layout may change between prototype versions.
- Memory, disk speed, temporary directory placement, antivirus scanning, and background system load can significantly affect runtime.
- The current implementation prioritizes correctness, verification, and searchability over speed.

## Benchmark policy

GFZS benchmark reports should include both compression metrics and operational properties.

Recommended fields:

- original input size
- GFZS output size
- expanded structured data processed
- backend compressor family
- compression profile class
- runtime
- machine/storage context
- search smoke-test result
- SHA verification result
- fallback record count
- whether reconstruction is source-container-byte-exact or member-level lossless
- comparison baseline used

Preferred future baselines:

1. Original source archive set
2. Folder-level `7z -mx=9`
3. Folder-level `tar.zst`
4. Folder-level `xz`
5. GFZS searchable pack

## Non-goals

GFZS is not currently trying to be:

- a general-purpose replacement for 7z, zstd, xz, or tar
- a production-ready archival standard
- a byte-for-byte re-creator of every original source container
- a fully optimized search engine
- a finalized file format specification

GFZS is currently a prototype for structure-aware, searchable, verifiable packing of large structured text archive collections.

## Implementation disclosure

This public README intentionally avoids documenting low-level tuning parameters, internal stream layout, candidate scoring rules, and profile-specific packing details.

Those details are still changing and should be evaluated through controlled benchmarks before being treated as stable format behavior.

## Project goal

GFZS StackPack explores a simple idea:

Can structured archives become smaller, searchable, and verifiable at the same time if we preprocess them before handing them to mature compressors?

Current early results suggest that corpus-aware preprocessing can provide meaningful benefits on StackExchange-style XML archive collections, while preserving deterministic verification and enabling prototype-level search.
