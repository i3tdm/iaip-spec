# IAIP Reserved Keys — v0

This document defines all reserved keys understood by the IAIP protocol
that are part of the v0 normative surface.

A Consumer:
- MUST recognize and interpret keys defined here
- MUST ignore unknown keys safely
- MUST NOT infer semantics for undocumented keys

This specification reflects the **canonical .iai reference files**.

---

### Path Semantics (Non-Executable)

All `*.path` fields in IAIP are **declarative hints only**.

- IAIP does NOT define filesystem access, creation, or validation.
- IAIP Consumers MUST NOT assume the ability to read or write paths.

Path Resolution:
- Unless explicitly stated otherwise, all paths are interpreted as **manifest-relative**.
- Consumers MAY normalize paths for display or logging purposes only.

---

## 1. Common Keys (All `.iai` files)

| Key | Type | Description |
|---|---|---|
| `kind` | string | Identifies the instruction kind |
| `name` | string | Human-readable name |

The YAML frontmatter is delimited from free-form text using `---`.

---

## 2. Manifest Keys (`kind: manifest`)

### Activation

| Key | Type | Description |
|---|---|---|
| `version` | string | IAIP spec version identifier (e.g., `"v0"`)  |
| `enabled` | list[string] | List of active kinds |
| `disabled` | list[string] | Explicit deny-list of kinds. Any kind listed here MUST be treated as inactive even if present in `enabled`. If a kind appears in both, deny overrides allow. |

- If `enabled` is present, **only** the listed kinds are considered active.
- If `version` is absent, Consumers MUST assume `v0` for compatibility.
- If `version` is present and unsupported, Consumers MUST STOP.

---

### Inclusion Structure (Per Kind)

For each enabled kind (`context`, `guardrails`, `contract`, `tasks`, `references`, `research`, `tracking`, `prompt`):

```yaml
<kind>:
  include:
    merge: []
    override: []
```

| Key                       | Type         | Description           |
| ------------------------- | ------------ | --------------------- |
| `<kind>.include.merge`    | list[string] | Files to be merged    |
| `<kind>.include.override` | list[string] | Files with precedence |

### Override Configuration (Per Kind)

A kind block MAY define an override configuration:

```yaml
<kind>:
  override:
    mode: replace_all | overlay
    algorithm: markdown_sections
```

| Key                         | Type   | Description                                                         |
| --------------------------- | ------ | ------------------------------------------------------------------- |
| `<kind>.override.mode`      | string | Override mode (`replace_all` or `overlay`)                          |
| `<kind>.override.algorithm` | string | Conflict algorithm used by `overlay` mode (v0: `markdown_sections`) |

Notes:

* If `<kind>.override` is absent, Consumers MUST assume `mode: replace_all`.
* `algorithm` is only used when `mode: overlay`.

---

### Inline Metadata (Per Kind)

A kind block MAY also define inline instructions using `metadata`.

```yaml
<kind>:
  metadata:
    # kind-specific instruction keys (same semantics as an external .iai file)
```

| Key               | Type   | Description                            |
| ----------------- | ------ | -------------------------------------- |
| `<kind>.metadata` | object | Inline instruction object for `<kind>` |


Rules:

* `metadata` and `include` MUST NOT coexist for the same kind.
* If both are present, the Consumer MUST warn and MUST prioritize `include`.
* `metadata` MUST contain only keys valid for the target kind.

---

## 3. Context Keys (`kind: context`)

| Key              | Type         | Description              |
| ---------------- | ------------ | ------------------------ |
| `scope`          | string       | Context scope (`global`) |
| `priority_paths` | list[string] | High-priority paths      |

---

## 4. Guardrails Keys (`kind: guardrails`)

| Key           | Type    | Description             |
| ------------- | ------- | ----------------------- |
| `enforcement` | string  | `hard` or `soft`        |
| `severity`    | string  | `critical` or `warning` |
| `auto_fix`    | boolean | Allow automatic fixes   |

---

## 5. Contract Keys (`kind: contract`)

| Key                   | Type    | Description          |
| --------------------- | ------- | -------------------- |
| `check_list_required` | boolean | Require checklist    |
| `validation_step`     | string  | Validation phase     |
| `compliance_report`   | string  | Output path or empty |

---

## 6. Tasks Keys (`kind: tasks`)

### Task Model

| Key                   | Type         | Description         |
| --------------------- | ------------ | ------------------- |
| `task_model.fields`   | list[string] | Allowed task fields |
| `task_model.required` | list[string] | Required fields     |

---

### Output

| Key           | Type   | Description                 |
| ------------- | ------ | --------------------------- |
| `output.mode` | string | `none`, `inline`, or `file` |
| `output.path` | string | Output path                 |
| `output.format` | string | `md`, `yaml`, or `jsonl` |
---

### Workflow

| Key                      | Type         | Description    |
| ------------------------ | ------------ | -------------- |
| `workflow.states`        | list[string] | Allowed states |
| `workflow.default_state` | string       | Default state  |

---

### Planning

| Key                           | Type    | Description        |
| ----------------------------- | ------- | ------------------ |
| `planning.prefer_small_tasks` | boolean | Prefer small tasks |
| `planning.max_steps_per_task` | integer | Step limit         |

---

## 7. References Keys (`kind: references`)

### Policy

| Key      | Type   | Description                  |
| -------- | ------ | ---------------------------- |
| `policy` | string | `prefer`, `allow`, or `deny` |

---

### Sources

| Key       | Type         | Description    |
| --------- | ------------ | -------------- |
| `sources` | list[object] | Reference list |

For each source:

| Key                   | Type         | Description                         |
| --------------------- | ------------ | ----------------------------------- |
| `sources[].id`        | string       | Identifier                          |
| `sources[].type`      | string       | `url`, `doc`, `rfc`, `repo`, `file` |
| `sources[].value`     | string       | Location                            |
| `sources[].authority` | string       | `primary`, `secondary`, `community` |
| `sources[].use_for`   | list[string] | Usage categories                    |
| `sources[].scope`     | string       | `global`, `module:<x>`, `path:<y>`  |

---

## 8. Research Keys (`kind: research`)

### Limits

| Key                        | Type    | Description       |
| -------------------------- | ------- | ----------------- |
| `limits.max_iterations`    | integer | Iteration cap     |
| `limits.stop_on_ambiguity` | boolean | Stop if ambiguous |

---

### Logging

| Key               | Type    | Description           |
| ----------------- | ------- | --------------------- |
| `logging.enabled` | boolean | Enable logging        |
| `logging.path`    | string  | Log path              |
| `logging.format`  | string  | `md`, `yaml`, `jsonl` |

---

### Evidence

| Key                    | Type    | Description      |
| ---------------------- | ------- | ---------------- |
| `evidence.required`    | boolean | Require evidence |
| `evidence.min_sources` | integer | Minimum sources  |

---

## 9. Tracking Keys (`kind: tracking`)

The tracking kind contains **three explicit top-level sections**.

### Memory

| Key               | Type    | Description   |
| ----------------- | ------- | ------------- |
| `memory.enabled`  | boolean | Enable memory |
| `memory.path`     | string  | Storage path  |
| `memory.max_size` | integer | Size limit    |

---

### History

| Key                | Type    | Description    |
| ------------------ | ------- | -------------- |
| `history.enabled`  | boolean | Enable history |
| `history.path`     | string  | Storage path   |
| `history.max_size` | integer | Size limit     |

---

### Audit

| Key                    | Type    | Description            |
| ---------------------- | ------- | ---------------------- |
| `audit.enabled`        | boolean | Enable audit           |
| `audit.path`           | string  | Audit path             |
| `audit.claim_evidence` | boolean | Require claim→evidence |

---

## 10. Prompt Keys (`kind: prompt`)

### Style

| Key               | Type   | Description             |
| ----------------- | ------ | ----------------------- |
| `style.tone`      | string | Communication tone      |
| `style.language`  | string | Language                |
| `style.verbosity` | string | `low`, `medium`, `high` |

---

### Format

| Key               | Type         | Description       |
| ----------------- | ------------ | ----------------- |
| `format.prefer`   | string       | Preferred format  |
| `format.sections` | list[string] | Standard sections |

---

### Behavior

| Key                                   | Type    | Description       |
| ------------------------------------- | ------- | ----------------- |
| `behavior.no_hallucination`           | boolean | Prevent invention |
| `behavior.ask_when_ambiguous`         | boolean | Ask on ambiguity  |
| `behavior.cite_sources_when_possible` | boolean | Prefer citations  |
| `behavior.do_not_execute`             | boolean | No execution      |

