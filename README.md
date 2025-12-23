# IAIP — Instructions AI Protocol

IAIP (Instructions AI Protocol) is an open specification that defines how AI instructions
can be structured, composed, and resolved in a deterministic way.

The protocol is focused on **orientation**, not execution.  
It exists to formalize how guidance is provided to AI systems in software projects,
using files that live in the repository and can be versioned, reviewed, and audited.

---

## What this repository is

This repository contains:

- The **normative specification** of IAIP v0
- A **canonical self-hosted example** of the protocol in use
- Design notes explaining motivations, decisions, and future considerations

It does **not** contain:
- Runtime implementations
- SDKs
- Agents
- Automation logic

---

## Core idea

Instead of relying on large prompts, transient chat context, or tool-specific settings,
IAIP defines a protocol where AI instructions are:

- Explicit
- Deterministic
- Repository-based
- Tool-agnostic

An external tool (called a *Consumer*) reads these files, resolves the instructions
according to the specification, and then applies them to an AI system.

---

## Repository structure

```text
.
├─ spec/
│  └─ v0/
│     ├─ manifest.md
│     ├─ resolution.md
│     ├─ keys.md
│     ├─ kinds.md
│     ├─ file-format.md
│     ├─ scope.md
│     ├─ consumer.md
│     └─ other normative documents
│
├─ .iaip/
│  ├─ manifest.iai
│  ├─ context.iai
│  ├─ tasks.iai
│  ├─ guardrails.iai
│  ├─ prompt.iai
│  └─ other canonical examples
│
├─ notes/
│  ├─ motivation.md
│  ├─ design-decisions.md
│  ├─ philosophy.md
│  ├─ future.md
│  ├─ audit_answered_questions.md
│  └─ audit_unanswered_questions.md
│
└─ README.md
````

---

## Where to start

### 📘 Specification (normative)

If you want to **implement a Consumer** or understand the protocol precisely, start here:

```
spec/v0/
```

Everything in this directory is **normative**.
If something is not defined there, it is intentionally out of scope.

---

### 📂 Canonical usage

The `.iaip/` directory contains a **self-hosted reference implementation** of the protocol.

These files:

* Follow the specification exactly
* Serve as canonical examples
* Act as a living validation of the spec

They are not examples of “best practices”, but examples of **correct protocol usage**.

---

### 📝 Notes and rationale

The `notes/` directory contains **non-normative** documents:

* Why the protocol exists
* Design trade-offs and decisions
* Philosophical positioning
* Audit artifacts and scope boundaries
* Future considerations (non-binding)

These files help explain *why* things are the way they are, but they do not define rules.

---

## Scope and boundaries

IAIP is intentionally limited in scope.

The protocol defines:

* File structure
* Instruction composition
* Deterministic resolution
* Conflict handling
* Orientation boundaries

The protocol does **not** define:

* Execution behavior
* Business or organizational outcomes
* Tooling, SDKs, or integrations
* Performance, cost, or energy metrics
* Educational or commercial models

This separation is deliberate and enforced throughout the specification.

---

## Project status

* **Version:** v0
* **Status:** Frozen and implementation-ready
* **Backward compatibility:** Guaranteed within v0
* **Nature:** Orientation-only protocol

---

## Specification Status

IAIP v0 is **frozen** and has passed multiple independent audits.

A formal freeze audit record is available at:

- `notes/audit_v0_freeze.md`

The v0 surface is considered stable and backward-compatible.
Future changes will target vNext only.

---

## License

License file: ./LICENSE