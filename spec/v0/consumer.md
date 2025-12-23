# Consumer Behavior

A Consumer is any system that reads and interprets IAIP instruction files.

Examples include:
- IDE integrations
- Agent runners
- CLIs
- Validation tools
- Static analyzers

## Core Principles

A Consumer MUST treat IAIP as:
- Orientation-only
- Non-executable
- Deterministic
- Repository-based

## Mandatory Behavior

A Consumer MUST:

- Respect resolution rules defined in `resolution.md`.
- Respect activation logic defined in `manifest.md`.
- Preserve free-form text verbatim.
- Avoid inference beyond explicit instructions.
- Enforce stopping conditions on ambiguity or invalid structure.
- Validate cross-kind consistency of loaded files.

## Warnings and Diagnostics

A Consumer SHOULD:

- Emit warnings for invalid-but-recoverable states.
- Surface the origin of instructions when making decisions.
- Make conflicts and ambiguities visible to the user.

## Optional Behavior

A Consumer MAY:

- Cache resolved instruction sets.
- Provide debugging or audit output.
- Offer tooling support (linting, visualization, validation).

These optional behaviors MUST NOT affect resolution semantics.