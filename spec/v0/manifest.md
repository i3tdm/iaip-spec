# IAIP v0 — Manifest

This document defines the normative structure and rules for the `manifest.iai` file.

The manifest is the **single entry point** of IAIP:
- It defines which `kinds` are active.
- It defines how each active kind composes its effective instruction set.
- It does **not** execute anything. It only describes **orientation** inputs for a Consumer.

Consumers MUST validate that every loaded `.iai` file declares the expected `kind` for its manifest slot.
A kind mismatch is a structural error and Consumers MUST STOP.


---

## 1. File Name and Location

The manifest file name is:

- `manifest.iai`

The manifest path is chosen by the user (e.g. IDE/Agent config). IAIP does not require a fixed directory name.

---

## 2. Manifest Top-Level Keys

The manifest supports the following top-level keys:

- `kind` (common key) — MUST be `"manifest"`.
- `version: "v0"` — explicit IAIP spec version for this manifest
- `name` (common key) — optional human-readable name.
- `enabled` — optional allow-list of kinds to activate.
- `disabled` — optional deny-list of kinds to deactivate.
- `<kind>` blocks — optional per-kind configuration blocks.

- If `version` is absent, a Consumer MUST assume `v0`.
- If `version` is present and unsupported, a Consumer MUST STOP and surface the incompatibility.

---

## 3. Activation (enabled / disabled)

### 3.1. Default

If `enabled` is not provided, the Consumer MUST treat the set of active kinds as **empty** (inert-by-default).

### 3.2. enabled

If `enabled` is provided, it is an explicit allow-list. Only kinds listed in `enabled` are active.

### 3.3. disabled

If `disabled` is provided, it is an explicit deny-list. Any kind listed in `disabled` MUST be considered inactive even if it appears in `enabled`.

### 3.4. Precedence

If a kind appears in both `enabled` and `disabled`, the Consumer MUST treat it as **disabled** (deny overrides allow).

---

## 4. Kind Blocks (Optional)

The manifest MAY include optional per-kind blocks, such as:

````yaml
context:
  metadata: ""
  override: ""
  include:
    merge: ["./context.extra.iai"]
    override: []
````

A kind block is never required. A kind may be active without having a block.

A kind block MAY contain:

* `metadata` (inline instructions for that kind)
* `override` — override configuration for this kind (mode/algorithm).
* `include` — a configuration object that MAY contain:
  * `merge` — list of `.iai` files to be added to the instruction set (same kind).
  * `override` — list of `.iai` files with precedence (first wins).

Note: `<kind>.override` refers to override configuration (mode/algorithm),
while `include.override` refers to override input files.

No other control keys are allowed inside `metadata`.

---

## 5. Semantic Base File (`<kind>.iai`)

For each active kind `K`, the Consumer MUST attempt to load a **semantic base file** named:

* `<K>.iai`

This semantic base file is **not** discovered by scanning directories. The Consumer MUST only attempt the exact file name `<K>.iai` relative to the manifest directory (or an implementation-defined base directory anchored at the manifest path).

### 5.1. Rule

If `<K>.iai` exists, it MUST be treated as the **first input** for that kind’s instruction set.

If `<K>.iai` does not exist, the base input for that kind is simply **absent** (empty).

### 5.2. Purpose

This rule allows a repository to provide simple, semantically named baseline files (e.g. `context.iai`, `guardrails.iai`) without requiring explicit include lists for small projects.

The manifest remains authoritative because:

* only `enabled/disabled` decides activation,
* and `include/override` decides composition.

---

## 6. metadata vs include (Exclusivity)

For each kind block:

* `metadata` and `include` MUST NOT coexist.

If a kind block contains both `metadata` and `include`, the Consumer MUST:

1. emit a warning, and
2. prioritize `include` and ignore `metadata`.

This rule guarantees determinism even in invalid states.

---

## 7. include Schema

`include` is a configuration object that MAY contain:

  * `merge` — list of `.iai` files to be added to the instruction set (same kind).
  * `override` — list of `.iai` files with precedence (first wins).

Example:

```yaml
tasks:
  override:
    mode: overlay
    algorithm: markdown_sections
  include:
    merge:
      - "./tasks.team.iai"
      - "./tasks.project.iai"
    override:
      - "./tasks.override.iai"
```

---

## 8. Kind Composition Overview (Normative)

For each active kind `K`, the Consumer MUST compose the effective instruction set in this order:

1. **Semantic base file**: load `<K>.iai` if it exists.
2. **Kind block input**:
   * if `metadata` exists (and `include` does not): apply `metadata` as an additional input.
   * if `include` exists: apply `include.merge` and `include.override` as defined.
   * If `<kind>.override` is absent, the Consumer MUST behave as `mode: replace_all`.
   * If `<kind>.override.mode` is present and `algorithm` is absent, the Consumer MUST use the default algorithm for that mode (defined in `resolution.md`)
3. If no inputs exist after resolution, the kind resolves to an **empty instruction set**.


The detailed resolution algorithm is defined in `spec/v0/resolution.md`.

---

## 9. No Implicit Discovery Beyond `<kind>.iai`

IAIP does not allow:

* directory scanning,
* glob resolution,
* recursive include directives inside kind files,
* or any implicit loading mechanism beyond the exact semantic base file name `<kind>.iai`.

Only:

* `manifest.iai` (entry point),
* explicit `include` lists,
* and the exact semantic base file `<kind>.iai`
  are permitted.

---

## 10. Conformance Requirements (Manifest)

A conforming Consumer MUST:

* respect inert-by-default behavior,
* implement allow/deny activation with deny precedence,
* implement semantic base file loading `<kind>.iai` for active kinds,
* implement metadata/include exclusivity (warn + include wins),
* validate that included files match the expected kind,
* and follow deterministic merge/override rules as defined in `resolution.md`.