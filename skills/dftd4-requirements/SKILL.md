---
name: dftd4-requirements
description: This skill should be used when users ask about requirements in dftd4; it prioritizes documentation references and then source inspection only for unresolved details.
---

# dftd4: Requirements

## High-Signal Playbook
### Route conditions
- Use this skill for dependency scope, environment prerequisites, and optional Python/docs test stacks.
- Route compiler/build-system setup to `dftd4-build-and-install` (`doc/recipe/installation.rst`).
- Route adapter-specific runtime behavior to `dftd4-reference`.

### Triage questions
- Target artifact: CLI/library, C API consumer, Python extension, docs build, or integration tests?
- Are you using conda/mamba or a pip+source build workflow?
- Do you need optional adapter tests (`ase`, `qcelemental`, `pyscf`)?
- Is Python 3 selected explicitly for Meson Python builds?
- Must downstream tools discover `dftd4` through `pkg-config`?

### Canonical workflow
1. Identify the target deliverable (core binary/lib vs Python/docs/test extras).
2. Install baseline runtime/build toolchain from installation docs.
3. For docs/Python layers, install `doc/requirements.txt` and Python build deps.
4. Configure build options for required artifacts (`python`, `api`, `api_v2` where needed).
5. Build and install to a known prefix.
6. Verify discovery (`pkg-config`) and Python import paths.
7. Enable optional framework deps only when those adapters are needed.

### Minimal working example
```sh
python -m pip install -r doc/requirements.txt
meson setup _build -Dpython=true -Dpython_version="$(command -v python3)"
meson compile -C _build
meson test -C _build dftd4 param --print-errorlogs
python -m pytest -q python/dftd4/test_library.py python/dftd4/test_parameters.py
```

### Pitfalls/fixes
- `doc/requirements.txt` is documentation-focused; Python runtime/build paths also need CFFI/Numpy-enabled extension build context.
- Optional adapter imports fail unless their extra dependency is installed (`ase`, `qcelemental`, `pyscf`).
- Expecting Python extension from CMake main build path causes confusion; use the Meson/python workflow.
- Python interpreter mismatch in Meson config can bind extension to an unintended interpreter; set `-Dpython_version=...`.
- Downstream editable Python builds can fail if `pkg-config` cannot find installed `dftd4`.

### Convergence/validation checks
- `pkg-config --modversion dftd4` resolves in the active environment after install.
- `python -c "import dftd4"` succeeds with the expected interpreter.
- For enabled adapters, import smoke tests pass (`import dftd4.ase`, `import dftd4.qcschema`, `import dftd4.pyscf`).
- Selected test subset runs cleanly before broader production use.

## Scope
- Handle questions about documentation grouped under the 'requirements' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/requirements.txt`
- `doc/recipe/installation.rst`
- `README.md`
- `python/README.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `skills/dftd4-requirements/references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `skills/dftd4-requirements/references/source_map.md` and start with the ranked source entry points.
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
- `meson_options.txt` (option-level requirement gates: API/Python/OpenMP/LAPACK)
- `python/meson.build` (Python extension build wiring and pkg-config use)
- `python/ffibuilder.py` (CFFI binding generation and symbol exposure)
- `python/dftd4/library.py` (shared-library loading and API availability checks)
- `python/dftd4/interface.py` (runtime validation and array/shape/unit expectations)
- `python/dftd4/test_ase.py` (optional ASE dependency behavior)
- `python/dftd4/test_qcschema.py` (optional QCSchema dependency behavior)
- `python/dftd4/test_pyscf.py` (optional PySCF dependency behavior)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app include python src test`).
