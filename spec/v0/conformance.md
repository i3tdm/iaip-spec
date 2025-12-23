# Conformance

A Consumer is IAIP-conformant (v0) if it satisfies **all MUST-level requirements**
defined in the IAIP v0 specification.

## Mandatory Capabilities

A conformant Consumer MUST:

- Load and interpret `manifest.iai` as the single entry point.
- Enforce inert-by-default behavior when `enabled` is absent.
- Implement allow/deny activation with deny precedence.
- Resolve instruction sets deterministically following `resolution.md`.
- Support semantic base files (`<kind>.iai`) for active kinds.
- Enforce `metadata` vs `include` exclusivity rules.
- Apply override replacement semantics exactly as defined.
- Preserve free-form text verbatim.
- Ignore unknown structured keys safely.
- Enforce stop-on-ambiguity and stop-on-structural-error behavior.

## Error Handling

A conformant Consumer MUST:

- Stop processing on structural errors.
- Stop processing on ambiguity.
- Surface errors to the user in a visible manner.
- Emit warnings for unsupported or invalid-but-recoverable states.

## Partial Implementations

Partial implementations:

- MUST clearly declare unsupported features.
- MUST NOT silently ignore required behavior.
- MAY operate in a restricted subset mode if explicitly documented.

## Scope Limitation

Conformance applies only to **orientation behavior**.
Execution, tooling, runtime behavior, and automation are explicitly out of scope.