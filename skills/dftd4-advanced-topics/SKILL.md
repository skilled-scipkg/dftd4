---
name: dftd4-advanced-topics
description: This skill should be used for lower-frequency advanced topics consolidated from one-doc skills (API index navigation and CLI manual deep dives) after checking core dftd4 skills.
---

# dftd4: Advanced Topics

## High-Signal Playbook
### Route conditions
- Use this skill for full CLI option semantics and command behavior (`man/dftd4.1.adoc`).
- Use this skill for cross-language API surface navigation (`doc/reference/index.rst`).
- Route install/build failures to `dftd4-build-and-install`.
- Route primary API usage to `dftd4-inputs-and-modeling`.

### Triage questions
- Is the question about CLI flags/subcommands (`run`, `param`) or API surface discovery?
- Do you need behavior of a specific flag (`--json`, `--grad`, `--pair-resolved`, `--param`)?
- Are you validating against CLI output, C API signatures, or Python wrappers?
- Should behavior be confirmed by test files before applying changes?

### Canonical workflow
1. Read the exact manual/API-index section first.
2. Trace option or symbol into `app/` or API wrapper files.
3. Validate behavior with the smallest matching regression test.
4. Run one concrete smoke command on repository sample inputs.

### Minimal working example
```sh
_build/app/dftd4 --help
_build/app/dftd4 param --list
_build/app/dftd4 --func pbe --json --grad app/02-nitralin.mol
```

```sh
python3 app/tester.py _build/app/dftd4 app/02-gradient.json --func pbe --grad app/02-nitralin.mol
meson test -C _build param --print-errorlogs
```

### Pitfalls/fixes
- Manual options may drift if help text and runtime behavior are changed independently; always cross-check `app/help.f90` and `app/driver.f90`.
- `--json [file]` defaults to the output file dftd4.json; scripted workflows should set explicit output names to avoid overwrite collisions.
- `--param` and `--func` interactions are order-sensitive in troubleshooting; reproduce with the exact command line from logs.

### Convergence/validation checks
- CLI help output matches documented options (`man/dftd4.1.adoc`, `app/help.f90`).
- A manual example command reproduces expected JSON keys and finite energy.
- Symbol-level API questions map to existing signatures in `include/dftd4.h` and wrappers in `src/dftd4/api.f90`.

## Scope
- Handle consolidated low-volume topics from one-doc skills.
- `doc/reference/index.rst` (API index and language-surface routing)
- `man/dftd4.1.adoc` (full CLI command/option semantics)

## Primary documentation references
- `doc/reference/index.rst`
- `man/dftd4.1.adoc`

## Workflow
- Start with the two primary references above.
- If details are missing, inspect `skills/dftd4-advanced-topics/references/doc_map.md` for full inventory.
- If behavior details remain ambiguous, inspect `skills/dftd4-advanced-topics/references/source_map.md`.
- Cite exact file paths in responses.

## Source entry points for unresolved issues
- `app/help.f90` (authoritative help text for `run`/`param` options)
- `app/cli.f90` (CLI subcommand and option parsing flow)
- `app/argument.f90` (argument tokenization and response-file support)
- `app/driver.f90` (how options map to runtime behavior)
- `src/dftd4/api.f90` (Fortran-exported C API entry points)
- `include/dftd4.h` (C API surface and lifecycle signatures)
- `python/dftd4/interface.py` (Python high-level API object wrappers)
- `python/dftd4/library.py` (CFFI binding/load boundary)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app include python src test`).
