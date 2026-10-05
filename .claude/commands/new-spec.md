---
description: Create a feature spec from the template
argument-hint: <id> <slug> [package]
---
Create a new spec for `$ARGUMENTS`.

1. Parse: first arg = id (e.g. `L01`), second = slug, optional third = package
   (`server` | `client` | `reviewer-core`). No package → cross-package → `specs/`.
2. Copy `specs/_template.md` to `<dir>/<id>-<slug>.md`; fill the title, `Status: draft`, `Packages`.
3. Ask me for Goal and Out of scope if they can't be inferred; leave other sections as TODO.
4. Do not write code in this command.
