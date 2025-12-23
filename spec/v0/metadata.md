# Metadata

The `metadata` field allows inline definition of instruction content
directly inside `manifest.iai`.

## Purpose

Metadata exists to support:
- small projects
- quick experiments
- minimal setups

## Rules

1. Metadata content MUST be pure instruction data
2. Metadata MUST NOT contain include, merge, or override
3. Metadata is semantically equivalent to an external `.iai` file
4. Metadata and include MUST NOT coexist for the same kind

## Precedence

If both metadata and include are present:
- The Consumer MUST warn
- The Consumer MUST prioritize include
