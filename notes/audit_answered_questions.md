# Audit Notes: Answered Questions

The following questions have been answered based on a strict audit of the `iaip-spec` repository (v0) content (`spec/v0`, `notes`, `README.md`).

## Technical Architecture & Integration

**1. AI Researcher (Interpretability):**
*   **Methodology:** The protocol enforces **Determinism**. `spec/v0/resolution.md` defines a strict algorithm where two Consumers must produce the exact same instruction set. Implicit "directory scanning" is forbidden; every instruction must have a traceable origin in `manifest.iai`. `notes/philosophy.md` reinforces the core philosophy: "Determinism > Intelligence."

**2. Software Engineer (Edge Cases):**
*   **Handling:** The protocol mandates a **Stop-on-Ambiguity** rule. `spec/v0/resolution.md` states that if a conflict (e.g., in an overlay) cannot be reliably resolved, the Consumer "MUST STOP and surface the ambiguity."

**3. Tech Enthusiast (IDE Integration):**
*   **Integration:** Integration is handled via the **Consumer** concept (`spec/v0/consumer.md`). The protocol is "Tool-Agnostic" (`README.md`), allowing any IDE to act as a Consumer that reads `manifest.iai` and resolves instructions before the session begins.

**16. DevOps Engineer (CI/CD):**
*   **Integration:** `spec/v0/consumer.md` lists "Validation tools" and "Static analyzers" as valid Consumers, implying the protocol allows CI pipelines to "lint" AI instructions or verify conformance before deployment.

## Ethics & Safeguards

**4. Ethics Specialist (Bias Safeguards):**
*   **Safeguards:** The protocol enables safeguards through the **`guardrails` kind** (`spec/v0/keys.md`). This allows defining strict rules with metadata like `enforcement: hard` and `severity: critical`. It provides the mandatory "Law" layer to enforce ethical rules defined by the user.

**9. Security Expert (Vulnerabilities):**
*   **Mitigation:** The **`guardrails` kind** allows "hard" enforcement of security patterns. Additionally, the `prompt` kind has a `behavior.do_not_execute` flag (`spec/v0/keys.md`) to prevent the AI from running potentially unsafe code.

**28. Policy Maker (Compliance):**
*   **Compliance:** The **`references` kind** (`spec/v0/keys.md`) supports `policy` keys (`prefer`, `allow`, `deny`) and source citations. This creates a traceable link between code instructions and regulatory texts (e.g., EU AI Act).

## User Experience & Collaboration

**6. Curious Student (Beginner Steps):**
*   **Simplicity:** The protocol uses **"Semantic Base Files"** (`spec/v0/manifest.md`). A beginner can simply create a `context.iai` in their project root. The system recognizes it automatically without complex configuration.

**8. Project Manager (Collaboration):**
*   **Adaptation:** The protocol supports scale via **Manifest Composition** (`spec/v0/manifest.md`). It uses `include.merge` and `include.override` lists, allowing teams to share a global policy file merged into every project's manifest.

**11. UX Designer (User Experience):**
*   **UX Support:** The **`prompt` kind** allows configuring `style.tone`. The `tasks` kind (`spec/v0/keys.md`) enables a `task_model` where fields like "UX Review" can be made mandatory for every generated task.

**25. Psychologist (Cognitive Load):**
*   **Mitigation:** `notes/motivation.md` explicitly mentions the goal of reducing the "tiredness" (*cansativo*) of constantly re-explaining rules to the AI. By "offloading" memory to `.iai` files, the developer's cognitive load is reduced.

## Validation & Quality

**7. Data Scientist (Validation):**
*   **Validation:** The **`contract` kind** (`spec/v0/keys.md`) supports `validation_step` and `compliance_report` keys, allowing formal validation phases to be required.

**21. Quality Assurance Tester (QA):**
*   **Verification:** The **`contract` kind** includes `check_list_required: boolean`. This forces the AI to output tasks that adhere to a specific checklist (Definition of Done) before completion.

## Adaptation & Philosophy

**10. Hobbyist Programmer (Personal Projects):**
*   **Customizability:** The protocol is **"Inert-by-Default"** (`spec/v0/manifest.md`). If you don't enable a feature, it doesn't exist. This modularity makes it lightweight for simple projects.

**13. Linguist (Language Variations):**
*   **Handling:** `spec/v0/file-format.md` preserves **"Free-Form Text"** verbatim. This supports any dialect. The `prompt` kind also has a `style.language` key.

**17. Philosopher (Philosophy):**
*   **Philosophy:** `notes/philosophy.md` states the core philosophy is **"Determinism > Intelligence."** It defines "Orientation" as passing necessary data to minimize hallucinations, placing the human as the architect of rules the AI must obey.

**23. Historian (Historical Context):**
*   **Context:** `notes/motivation.md` contextualizes the protocol as a solution to the "Technical Frustration" of the current AI wave, addressing the problem of being a "Hostage to Platform" (chat interfaces).

**27. Game Designer (Procedural Generation):**
*   **Adaptability:** The **`tasks` kind** allows defining a custom schema (`task_model`). A designer can require fields like "Seed" and "Constraints" for every task, standardizing procedural inputs.

**30. Futurist (Evolution):**
*   **Evolution:** `notes/future.md` states the protocol will **never** be an execution agent but will evolve to support **MCP (Model Context Protocol)** and governance tools.
