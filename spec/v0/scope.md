# Scope

IAIP defines how AI systems are **oriented**, not how they act.

IAIP is inert by default. Instruction files have no effect unless explicitly activated by the manifest.

## In Scope

IAIP defines:
- Instruction file structure
- Instruction categorization (kinds)
- Deterministic resolution rules
- Consumer behavior requirements
- Error handling and stopping conditions

## Out of Scope

IAIP explicitly does NOT define:
- Agent architectures
- Tool execution
- Task execution
- Memory implementations
- Model selection
- Prompt engineering techniques
- Runtime orchestration

Any system implementing execution logic is considered a **Consumer**, not part of IAIP.

## Design Constraint

IAIP MUST remain:
- Tool-agnostic
- Model-agnostic
- Platform-agnostic

## Inert by Default

IAIP is inert by default: instruction files have no effect unless a Consumer is explicitly configured to load a `manifest.iai`.
