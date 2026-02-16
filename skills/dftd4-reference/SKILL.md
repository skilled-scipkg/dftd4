---
name: dftd4-reference
description: This skill should be used when users ask about reference in dftd4; it prioritizes documentation references and then source inspection only for unresolved details.
---

# dftd4: Reference

## High-Signal Playbook
### Route conditions
- Use this skill for adapter integrations: ASE, QCSchema, and PySCF (`doc/reference/ase.rst`, `doc/reference/qcschema.rst`, `doc/reference/pyscf.rst`).
- Route core API object semantics to `dftd4-inputs-and-modeling`.
- Route installation/runtime dependency issues to `dftd4-build-and-install` and `dftd4-requirements`.

### Triage questions
- Which integration target: ASE calculator, QCSchema `run_qcschema`, or PySCF wrapper?
- Need energy only, gradient, properties, or pair-resolved outputs?
- Using D4 or D4S level/model?
- Supplying a method name or explicit `params_tweaks`?
- Are optional dependencies installed (`ase`, `qcelemental`, `pyscf`)?
- Is the input periodic and expected to emit stress/virial-like outputs?

### Canonical workflow
1. Confirm optional dependency for target adapter is available.
2. Build structure input in host framework format (ASE `Atoms`, QCSchema `AtomicInput`, PySCF `gto.M`).
3. Select method or damping tweaks and choose `d4`/`d4s`.
4. Run adapter entrypoint (`DFTD4`, `run_qcschema`, `DFTD4Dispersion`/`energy`).
5. Request optional outputs (`gradient`, `property`, `pair_resolved`) only when needed.
6. Validate key outputs against adapter tests.
7. If behavior diverges, inspect adapter source and corresponding tests.

### Minimal working example
```python
from ase.build import molecule
from dftd4.ase import DFTD4

atoms = molecule("H2O")
atoms.calc = DFTD4(method="PBE")
print(atoms.get_potential_energy())
print(atoms.get_forces().shape)
```

```python
import qcelemental as qcel
from dftd4.qcschema import run_qcschema

inp = qcel.models.AtomicInput(
    molecule={"symbols": ["O", "H", "H"],
              "geometry": [0.0, 0.0, -0.73578586109551, 1.44183152868459, 0.0, 0.36789293054775, -1.44183152868459, 0.0, 0.36789293054775]},
    driver="energy",
    model={"method": "TPSS-D4"},
)
print(run_qcschema(inp).return_result)
```

### Pitfalls/fixes
- Missing optional dependency causes immediate import failure in adapter modules; install that framework first (`python/dftd4/ase.py`, `python/dftd4/qcschema.py`, `python/dftd4/pyscf.py`).
- QCSchema `level_hint` must be `d4` or `d4s`; invalid values return structured input error (`python/dftd4/qcschema.py`, `python/dftd4/test_qcschema.py`).
- Unknown functional names return explicit input errors; validate method names before batch runs (`python/dftd4/test_qcschema.py`).
- Using both full method and conflicting manual damping tweaks causes invalid setup; choose one strategy (`python/dftd4/qcschema.py`).
- For ASE/QCSchema custom params, ensure required damping keys (`s8`, `a1`, `a2`) are present (`python/dftd4/ase.py`, `python/dftd4/qcschema.py`).
- In PySCF wrapper mode, pass SCF/CASCI-compatible objects to dispersion patching (`python/dftd4/pyscf.py`).

### Convergence/validation checks
- Reproduce adapter test-style energies for a known molecule before large runs (`python/dftd4/test_ase.py`, `python/dftd4/test_qcschema.py`, `python/dftd4/test_pyscf.py`).
- For gradients, verify output shape and expected sign conventions in host framework.
- When `pair_resolved=True` in QCSchema, verify pairwise additive and non-additive totals match total energy.
- For periodic PySCF cases, confirm periodic setup path is active and produces distinct results from molecular mode.

## Scope
- Handle questions about documentation grouped under the 'reference' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/reference/qcschema.rst`
- `doc/reference/pyscf.rst`
- `doc/reference/ase.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `skills/dftd4-reference/references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `skills/dftd4-reference/references/source_map.md` and start with the ranked source entry points.
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
- `python/dftd4/ase.py` (ASE calculator API and parameter handling)
- `python/dftd4/qcschema.py` (QCSchema input translation, options, and error mapping)
- `python/dftd4/pyscf.py` (PySCF energy/gradient patching path)
- `python/dftd4/test_ase.py` (regression energies/forces for ASE integration)
- `python/dftd4/test_qcschema.py` (error models, gradient/pairwise/property checks)
- `python/dftd4/test_pyscf.py` (PySCF reference values and PBC behavior checks)
- `python/dftd4/interface.py` (shared Python API object lifecycle used by adapters)
- `src/dftd4/reference.f90` (Fortran-side reference/property entry points)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app include python src test`).
