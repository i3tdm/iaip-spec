# IAIP v0 — Resolution

This document defines the normative algorithm a Consumer MUST follow to resolve
the effective instruction set for each active kind.

Resolution is deterministic: two Consumers following this document MUST produce
the same resolved instruction set given the same repository state.

---

## 1. Definitions

- **Consumer**: a tool that reads IAIP (IDE integration, agent runner, CLI, etc.).
- **Manifest**: the entry point file `manifest.iai`.
- **Kind**: a semantic category of instructions (e.g. `context`, `guardrails`).
- **Active Kind**: a kind selected by `enabled` and not excluded by `disabled`.
- **Semantic Base File**: the file named `<kind>.iai` for a given kind.
- **Kind Block**: an optional section in the manifest under the kind name.
- **Instruction Set**: the final resolved content for a kind, composed from one or more inputs.
- **Input**: an instruction source (a semantic base file, a metadata block, or an included file).

---

## 2. Inputs and Format

All `.iai` files are parsed according to `spec/v0/file-format.md`.
Each `.iai` file MAY contain:
- YAML frontmatter (machine-structured keys),
- and a free-text body (human/LLM-readable rules).

A Consumer MUST treat both as part of the instruction input.

---

## 3. Activation Set

Given a parsed `manifest.iai`, the Consumer MUST compute the active kinds:

1) If `enabled` is missing:
   - ActiveKinds = empty set.
2) Else:
   - ActiveKinds = set(enabled)
3) If `disabled` exists:
   - ActiveKinds = ActiveKinds \ set(disabled)

If a kind appears in both lists, it is inactive (deny overrides allow).

---

## 4. Resolution Base Directory

All relative file paths MUST be resolved relative to the directory containing `manifest.iai`,
unless the Consumer provides an explicit base directory anchored to the manifest path.

The Consumer MUST NOT scan directories.

---

## 5. Per-Kind Resolution Algorithm (Normative)

For each active kind `K` in ActiveKinds:

### Step 1 — Initialize Inputs
Initialize an ordered list of Inputs as empty:

- Inputs = []

### Step 2 — Load Semantic Base File (`<K>.iai`)
Attempt to load the semantic base file:

- BasePath = `<K>.iai` resolved relative to the manifest directory.

If BasePath exists:
- parse it as a `.iai` file
- validate it declares the expected kind (`kind: K`) if present
- append it to Inputs

If BasePath does not exist:
- do nothing

This step is performed regardless of whether a kind block exists in the manifest.

### Step 3 — Read Kind Block (Optional)
If the manifest contains a kind block for `K`, read it.

Let:
- HasMetadata = kindBlock has `metadata`
- HasInclude  = kindBlock has `include`

#### Step 3.1 — metadata/include Exclusivity
If HasMetadata and HasInclude:
- Consumer MUST emit a warning
- Consumer MUST ignore `metadata`
- Consumer MUST proceed using `include` only

#### Step 3.2 — Apply metadata (if present and include absent)
If HasMetadata and not HasInclude:
- treat `metadata` as an input of kind `K`
- append the metadata input to Inputs

`metadata` MUST contain only keys valid for the target kind.
Protocol control keys (such as `include`, `merge`, `override`) are forbidden inside `metadata`.

#### Step 3.3 — Apply include (if present)
If HasInclude:
- read `include.merge` (list) and `include.override` (list)
- resolve each path relative to the manifest directory
- each referenced file MUST be parsed as `.iai`
- each referenced file MUST match the expected kind `K` (see Section 6)

##### include.merge
For each file in `include.merge` in order:
- append it to Inputs

##### include.override
`include.override` defines precedence. The first valid override file wins.

The Consumer MUST:
- parse override files in order
- the first successfully loaded override input becomes OverrideInput

If no override file loads successfully:
- proceed without override

If OverrideInput exists, the Consumer MUST apply it according to the per-kind override configuration:

- Let OverrideMode be:
  - `<kind>.override.mode` if present
  - otherwise `replace_all` (default)

- Let OverrideAlgorithm be:
  - `<kind>.override.algorithm` if present
  - otherwise the default algorithm for the selected mode

Mode: replace_all (default)
- If OverrideInput exists, it replaces the entire Inputs list as:
  - Inputs = [OverrideInput]
(Override wins over base + metadata + merge)

Mode: overlay
- The Consumer MUST NOT discard previously collected inputs.
- The Consumer MUST apply OverrideInput as a precedence layer over the existing instruction set.
- Conflicts MUST be detected using the selected OverrideAlgorithm.
- When a conflict is detected, the OverrideInput instruction MUST take precedence over the conflicting prior instruction.
- If no conflict is detected, prior instructions MUST remain valid.

If the Consumer cannot reliably determine conflict under the selected OverrideAlgorithm, it MUST STOP and surface the ambiguity to the user.


### Step 4 — Empty Instruction Set
If Inputs is empty after Steps 2 and 3:
- the resolved instruction set for kind `K` is empty

A Consumer MUST handle empty instruction sets gracefully.

### Step 5 — Return Resolved Instruction Set
The resolved instruction set for kind `K` is the ordered Inputs list (or the single override input).

---

## 6. Cross-Kind Validation

When the Consumer loads a `.iai` file for a kind slot `K` (base file or included file):

- If the file declares `kind: X` and `X != K`,
  the Consumer MUST treat this as an error and MUST NOT apply the file to kind `K`.

The Consumer MUST surface the error to the user (see `spec/v0/errors.md`).

This prevents cross-kind injection and preserves determinism.

---

## 7. Determinism Requirements

A conforming Consumer MUST:
- not scan directories,
- not load any file not explicitly named or referenced,
- resolve paths relative to the manifest directory,
- apply the same precedence rules for all kinds,
- and implement the override replacement rule exactly.

---

## 8. Notes on Safety (Non-execution)

Resolution only produces instruction inputs.
Consumers MUST NOT interpret IAIP as an execution plan.
Any execution behavior (tool calls, commands, scripts) is out of scope for IAIP v0.

The `prompt` kind may define interaction constraints (e.g. "do not execute"),
but it does not grant execution capabilities.

### Kind Validation (Structural Integrity)

For every loaded `.iai` file, the Consumer MUST validate the file header:

- The file MUST contain `kind: <expected_kind>` where `<expected_kind>` is the kind slot that caused the file to be loaded
  (e.g., `context.iai` MUST have `kind: context`).
- If the file declares a different kind (e.g., `kind: manifest` inside `context.iai`), the Consumer MUST STOP.

Rationale:
- This prevents accidental "recursive manifests" and eliminates ambiguous behavior.


### Override Algorithms

The root section (text outside headings) behaves as a named section with an empty path.

OverrideAlgorithm: markdown_sections (v0)
- The Consumer MUST split free-form text into sections using Markdown headings (`#`, `##`, `###`, ...).
- Each section is identified by its full heading path (e.g., `## Tasks / ### Evidence`).
- A conflict exists when OverrideInput defines a section with the same heading path as an existing section.
- On conflict, the entire existing section MUST be replaced by the OverrideInput section.
- Text outside any heading is treated as the "root section".