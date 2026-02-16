---
name: dftd4-index
description: This skill should be used when users ask how to use dftd4 and the correct generated documentation skill must be selected before going deeper into source code.
---

# dftd4 Skills Index

## Route the request
- Classify the request into one of the generated topic skills listed below.
- Prefer workflow-level guidance first; switch to function-level tracing only when behavior is unclear.

## Generated topic skills
- `dftd4-build-and-install`: Build and Install (build, installation, compilation, and environment setup)
- `dftd4-inputs-and-modeling`: Inputs and Modeling (inputs, system setup, models, and physical parameterization)
- `dftd4-reference`: Reference (ASE, QCSchema, and PySCF integration usage)
- `dftd4-recipe`: Recipe (stepwise workflows, especially VASP integration)
- `dftd4-general`: General (first-contact CLI starts and project context)
- `dftd4-requirements`: Requirements (dependency and environment prerequisites)
- `dftd4-advanced-topics`: Advanced Topics (API index navigation and full CLI manual deep dives)

## Documentation-first inputs
- `doc`
- `man`

## Tutorials and examples roots
- `app`
- `assets/examples`

## Test roots for behavior checks
- `test`
- `python/dftd4`

## Escalate only when needed
- Start from topic skill primary references.
- If those references are insufficient, inspect the topic doc map (for example: `skills/dftd4-general/references/doc_map.md`).
- If documentation still leaves ambiguity, inspect the topic source map (for example: `skills/dftd4-general/references/source_map.md`) and trace symbols from there.
- Use targeted symbol search while inspecting source (for example: `rg -n "<symbol_or_keyword>" app include python src test`).

## Source directories for deeper inspection
- `app`
- `include`
- `python`
- `src`

## Fast simulation startup from repository
1. `meson setup _build`
2. `meson compile -C _build`
3. `_build/app/dftd4 --func pbe app/01-ammonia.tmol`
4. Optional JSON regression check: `python3 app/tester.py _build/app/dftd4 app/01-energy.json --func pbe app/01-ammonia.tmol`
