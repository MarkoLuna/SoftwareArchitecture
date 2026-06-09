# AGENTS.md — Software Architecture Documentation

## What this repo is

A pure **Markdown knowledge base** of software architecture patterns, principles, and best practices. No executable code, no build system, no CI/CD, no tests, no scripts.

## Structure

- **29 topic directories** — each has a `README.md` entry point with a `## 🗺️ Table of Contents`. Example: `clean-code/README.md`, `solid-principles/README.md`.
- **Deep-dive sub-articles** — additional `.md` files within topic dirs (e.g., `api-design/idempotency.md`, `resilience-patterns/rate-limiting.md`).
- **Root reference files** — `glossary.md` (198 lines, 6 categories), `case-studies.md` (1018 lines: Netflix, Uber, Airbnb, Spotify, Amazon).
- **`TODO.md`** — project roadmap with `[X]`/`[ ]` checkboxes.
- **`AGENTS.md`** — this file.

## Entry point quirks

- **`cloud-native/`** has no `README.md`; its entry point is `cloud-native/kubernetes.md`.
- **`language-guides/`** has no `README.md`; just `java-upgrades.md` and `go-upgrades.md`.
- Every other topic dir has a `README.md` as the entry point.

## Content conventions

- **Mermaid diagrams** (`graph TD`, `flowchart`, `sequenceDiagram`, `stateDiagram-v2`, `classDiagram`) — used in nearly every README.
- **Multi-language code examples** — Java, Go, Python inside Markdown code blocks (not source files). Some modules (e.g., `solid-principles/`) use collapsible `<details>` per language.
- **Comparison tables** — almost every module ends with a trade-off matrix.
- **Cross-references** — modules link to each other. Use `> [!TIP]` or `> [!NOTE]` callouts.

## Working here

- All changes are **plain Markdown edits**. No language toolchain required.
- To preview Mermaid diagrams: render on GitHub or use a Mermaid-compatible editor.
- `case-studies.md` includes a documentation template for adding new case studies — follow that pattern.
- Commit messages should match the documentation-only nature (e.g., "Add rate-limiting deep-dive", "Fix typo in clean-code/README.md").

## What to do when contributing

1. Read the relevant topic `README.md` first for existing structure and style.
2. Follow the emoji title, ToC, Mermaid diagrams, comparison table conventions.
3. For new sub-articles, add cross-references in the parent `README.md`.
4. Update `glossary.md` if new terms are introduced.
5. Mark items in `TODO.md` as `[X]` when completed.
