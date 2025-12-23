## Backlog (v0.0.0.1 / vNext) — “Workspace Permissions / Commands”

### EPIC: Workspace governance for AI Consumers

**Goal:** Allow projects to declare **where a Consumer is allowed to read/write**, and which operations are permitted, per workspace context.

### Story 1 — Define “Workspace” concept (non-normative first)

* **Deliverable:** `notes/workspace.md`
* **Content:** definition of “workspace” (one repo can host multiple workspaces), motivation, and non-goals.
* **Acceptance:**

  * Explains why “workspace” is chosen vs “project”
  * Mentions that it’s about **Consumer permissions**, not AI execution
  * Explicitly states: *does not change IAIP v0 behavior*

### Story 2 — Propose a new Kind: `workspace.iai` (draft schema in notes)

* **Deliverable:** `notes/kind-workspace-draft.md`
* **Draft keys (example only, not spec yet):**

  * `workspace.id`
  * `paths.read_allow`, `paths.read_deny`
  * `paths.write_allow`, `paths.write_deny`
  * `ops.allow` (e.g., `read`, `write`, `create`, `delete`, `rename`)
  * `safety.protect_iaip_dir: true` (protect `.iaip/` and `spec/` by default)
* **Acceptance:**

  * Clarifies this is a **policy declaration for Consumers**
  * No execution semantics
  * Deterministic merging rules are stated (or explicitly deferred)

### Story 3 — “Commands” vs “Permissions” decision record

* **Deliverable:** `notes/adr-commands-vs-permissions.md`
* **Decision options:**

  1. `workspace.iai` as permissions/capabilities
  2. `commands.iai` as allowed command intents (still non-executable)
  3. Both (but with strict boundaries)
* **Acceptance:**

  * Clear winner picked
  * Non-goals include “no command execution”

### Story 4 — Add canonical example (without making it normative)

* **Deliverable:** `.iaip/workspace.iai` (optional) OR keep it only in `notes/`
* **Acceptance:**

  * If included in `.iaip/`, it must be **disabled** in `manifest.iai` for v0
  * README mentions it as “planned / draft” only (optional)

### Story 5 — Consumer guidance: how IDEs should respect it

* **Deliverable:** `notes/consumer-workspace-guidance.md`
* **Acceptance:**

  * Defines expected behavior: “MUST refuse writes to protected paths”
  * Defines what happens on violation: “MUST STOP and report”
  * Defines minimum viable implementation for an IDE plugin

---

## Sugestão de naming (pra não virar confusão)

* **Kind:** `workspace`
* **File:** `workspace.iai`
* **Purpose:** permissions/capabilities for Consumers
* **Avoid:** `project` (ambíguo), `commands` (parece execução), `policy` (muito genérico)