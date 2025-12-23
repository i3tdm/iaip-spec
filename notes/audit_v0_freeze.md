# IAIP v0 — Freeze Audit Record

This document records the final audit state of the IAIP v0 specification
at the moment of the v0 freeze decision.

This file is **descriptive**, not normative.

---

## Executive Verdict

**READY_FOR_V0_FREEZE**

The repository is internally consistent, complete within its declared scope,
and the canonical usage strictly conforms to the specification.

---

## Blockers

None.

---

## Non-Blocking Notes

- **Empty `examples/` directory**  
  The directory exists but contains no examples.  
  This is acceptable for v0, as the `.iaip/` directory serves as the canonical
  reference implementation.

- **No validation tooling provided**  
  No schema validator, linter, or CLI is included.  
  This is explicitly stated as out of scope for v0.

---

## v0 Freeze Surface (Normative Contract)

The following elements are considered **frozen for v0** and MUST NOT change
without breaking compatibility:

- **Manifest Entry Point**  
  `manifest.iai` is the single authoritative entry point.

- **Activation Logic**  
  `enabled` is an allow-list.  
  `disabled` is a deny-list that overrides `enabled`.  
  Default behavior is inert.

- **Resolution Order**  
  Base File (`<kind>.iai`)  
  → Metadata / Include  
  → Merge  
  → Override

- **Metadata vs Include Exclusivity**  
  `metadata` and `include` cannot coexist in the same kind block.

- **Include Structure**  
  `include: { merge: [], override: [] }`

- **Determinism Guarantee**  
  Directory scanning and implicit discovery are forbidden.

---

## Spec ↔ Usage Alignment

- **Keys Coverage**: 100%  
  Every key used in `.iaip/*.iai` is defined in `spec/v0/keys.md`.

- **Schema Coverage**: Consistent  
  `manifest.iai` follows the schema defined in `spec/v0/manifest.md`.

- **Resolution Behavior**: Consistent  
  Canonical usage matches the algorithm defined in `spec/v0/resolution.md`.

---

## Path & Link Integrity

- No broken internal links detected.
- All paths are manifest-relative as defined in the specification.
- References in `.iaip/references.iai` resolve correctly.

---

## Normative Consistency

- No contradictory MUST / SHOULD / MAY statements found.
- Free-text handling and resolution algorithms are compatible.
- Absence of `<kind>.iai` is consistently treated as “no input”.

---

## Notes Hygiene

- `notes/` contains no normative rules.
- Audit answer files correctly reference the specification.
- Out-of-scope topics are explicitly documented as such.

---

## vNext Backlog (Non-Normative)

The following items are explicitly deferred beyond v0:

- Support for binary or multimodal inputs (images, audio).
- Formal schema for `compliance_report` output in the `contract` kind.
