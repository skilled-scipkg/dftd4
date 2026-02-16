---
name: dftd4-general
description: This skill should be used when users ask about general in dftd4; it prioritizes documentation references and then source inspection only for unresolved details.
---

# dftd4: General

## High-Signal Playbook
### Route conditions
- Use this skill for first-contact routing, quick CLI starts, and citation/context questions (`doc/index.rst`).
- Route install/build setup to `dftd4-build-and-install`.
- Route API usage to `dftd4-inputs-and-modeling` and `dftd4-reference`.
- Route full CLI/manual option semantics to `dftd4-advanced-topics`.

### Triage questions
- Are you running the standalone CLI or embedding DFT-D4 via API?
- What structure format/input file do you have today?
- Which functional/method should parameter lookup use?
- Do you need only energy, or also gradients, properties, pair-resolved outputs, JSON?
- Is the system periodic, and should automatic periodic handling apply?
- Do you need citation output for publication reporting?

### Canonical workflow
1. Confirm binary availability and pick an input structure file.
2. Select functional/method (`--func`) for damping parameter lookup.
3. Run a single-point dispersion calculation.
4. Add optional outputs (`--grad`, `--json`, `--property`, `--pair-resolved`) as needed.
5. Validate produced files and JSON keys against expectations.
6. Route deeper needs to specialized skills.

### Minimal working example
```sh
meson compile -C _build
_build/app/dftd4 --func pbe app/01-ammonia.tmol
_build/app/dftd4 --func pbe0 --json --grad --noedisp app/04-caffeine.xyz
```

```sh
_build/app/dftd4 --property app/03-lenalidomid.gen
_build/app/dftd4 --pair-resolved --func pbe app/01-ammonia.tmol
python3 app/tester.py _build/app/dftd4 app/01-energy.json --func pbe app/01-ammonia.tmol
```

### Pitfalls/fixes
- Missing or incorrect functional choice gives wrong parameter context; verify method string before production runs.
- `.CHRG` in input directory is auto-read unless overridden with `--charge` (`man/dftd4.1.adoc`, `app/help.f90`).
- `--grad` writes to default output file unless explicit path is passed; manage file naming in scripted loops.
- Forgetting `--noedisp` can create `.EDISP` side files when JSON-only workflows are intended.
- DFT-D4 uses OpenMP shared-memory parallelism, not MPI; tune `OMP_NUM_THREADS` accordingly (`README.md`).

### Convergence/validation checks
- Baseline run prints a finite dispersion energy for known test geometry.
- Requested output artifacts are present and parseable (JSON contains an `energy` key).
- For pairwise mode, compare against `app/04-pair-analysis.json` with `app/tester.py` when possible.
- For citation workflows, verify `_build/app/dftd4 --citation` includes required references.

## Scope
- Handle questions about documentation grouped under the 'general' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/index.rst`
- `README.md`
- `man/dftd4.1.adoc`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `skills/dftd4-general/references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `skills/dftd4-general/references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `app`
- `assets/examples`

## Test references
- `test`
- `python/dftd4`

## Optional deeper inspection
- `app`
- `include`
- `python`
- `src`

## Source entry points for unresolved issues
- `app/main.f90` (CLI startup and top-level execution path)
- `app/cli.f90` (`get_arguments` and subcommand routing)
- `app/argument.f90` (argument parsing and response-file handling)
- `app/driver.f90` (`run_main`/`run_param`, output workflow, `.CHRG` handling)
- `app/help.f90` (authoritative CLI help text and option semantics)
- `src/dftd4/param.f90` (`get_functionals`, rational damping lookup)
- `src/dftd4/output.f90` (human-readable, tagged, and JSON formatting)
- `src/dftd4/version.f90` (version and citation metadata)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app include python src test`).
