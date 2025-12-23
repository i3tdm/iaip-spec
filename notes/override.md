# Override — Rationale & Usage Notes

This note explains **why** IAIP override works the way it does in v0.

## Why override is per-kind

Different kinds have different risk profiles:

- `tasks` changes frequently and benefits from overlay-style evolution.
- `context` often needs safe updates without invalidating legacy artifacts.
- `guardrails` can be safety-critical and may require strict replacement.

Therefore, IAIP allows override to be configured **per kind**.

## Why an explicit algorithm exists

The protocol must remain deterministic across Consumers.
If "conflict" is left implicit, each Consumer will interpret it differently.

So IAIP defines:
- a per-kind override configuration (`mode`, `algorithm`)
- and a default algorithm (`markdown_sections`) that is simple and widely supported

Consumers MAY implement additional algorithms, but MUST be explicit about which one they use.

## Why `markdown_sections` is the default algorithm

Most real-world instruction sets are written in Markdown with headings.
Using headings as the conflict unit avoids:
- semantic guessing
- hidden heuristics
- "smart" conflict resolution that differs per tool

`markdown_sections` makes override deterministic without changing file formats.

## Recommended defaults

Suggested baseline choices:

- `tasks`: `mode: overlay`, `algorithm: markdown_sections`
- `context`: `mode: overlay`, `algorithm: markdown_sections`
- `guardrails`: `mode: replace_all` (safer for critical constraints)

These are recommendations only. Projects can choose otherwise.

## Goal

The goal is not to execute logic, but to help LLM-based tools:
- preserve legacy validity
- evolve rules safely
- reduce hallucinations during transitions
