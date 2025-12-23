# Instruction Kinds (v0)

IAIP categorizes instructions into semantic kinds.
Kinds express **intent**, not behavior.
Some kinds (such as prompt) may define interaction or communication rules, which do not imply execution or automation.

Each instruction file represents exactly one kind.

---

## Defined Kinds

### `manifest`
Controls activation and resolution of instruction kinds.

### `context`
Describes architecture, structure, and relevant project areas.

### `guardrails`
Defines prohibitions, constraints, and safety rules.

### `contract`
Defines acceptance criteria and compliance conditions.

### `tasks`
Defines how tasks may be structured and recorded.

### `references`
Defines authoritative external sources.

### `research`
Defines investigation limits and exploration rules.

### `tracking`
Defines auditability, history, and evidence tracking.

### `prompt`
Defines communication style and interaction constraints for the Consumer.

---

## Kind Rules

- Each file MUST declare exactly one kind
- Kinds are resolved independently
- Kinds define orientation only