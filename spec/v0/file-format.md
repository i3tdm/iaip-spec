# File Format

IAIP instruction files use the `.iai` extension.

## Structure

An `.iai` file consists of:

1. Optional structured fields (YAML-compatible)
2. Optional free-form text content

Example structure:

---
key: value
another_key:
  nested: true
---

Free Text Format Declaration

Free text is plain-text by default.

Authors MAY optionally declare the intended structural format of a free text block
by placing a format declaration at the very beginning of the text.

The declaration uses the following form:

---
---<format>---
---

Where <format> is an opaque identifier (e.g., md, plain).

This declaration is a hint only.
It does not mandate semantic interpretation, execution, or validation.

Consumers MAY use the declared format solely to select an appropriate structural
resolution algorithm (if applicable).

If no format is declared, Consumers MUST treat the content as plain-text.

## Parsing Rules

- Structured fields MUST be parsed before free text
- Free text MUST be preserved verbatim
- Consumers MUST ignore unknown structured keys
- Consumers MUST NOT infer meaning from formatting inside free text

## Encoding

- UTF-8
- LF line endings recommended
