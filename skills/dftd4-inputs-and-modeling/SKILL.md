---
name: dftd4-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in dftd4; it prioritizes documentation references and then source inspection only for unresolved details.
---

# dftd4: Inputs and Modeling

## High-Signal Playbook
### Route conditions
- Use this skill for data model setup, units, and API-level D4/D4S calculations (Fortran/C/Python).
- Route build/link/install failures to `dftd4-build-and-install`.
- Route framework adapters (ASE/QCSchema/PySCF) to `dftd4-reference`.
- Route CLI flag behavior to `dftd4-advanced-topics`.

### Triage questions
- Which API are you using: Fortran, C, or Python (`doc/reference/fortran.rst`, `doc/reference/c.rst`, `doc/reference/python.rst`)?
- Are coordinates/lattice provided in atomic units (Bohr)?
- Do you need energy only, or also gradient/virial/hessian/pairwise/properties?
- Is the system molecular or periodic (requires lattice + periodic flags)?
- Are you loading damping by method name or supplying explicit parameters?
- Do you need D4 or D4S?

### Canonical workflow
1. Build/create a structure object with atomic numbers and Bohr coordinates.
2. For periodic systems, provide lattice and periodic dimensions.
3. Create a dispersion model object (`D4` or `D4S`) from the structure.
4. Create/load damping parameters (method or explicit `s8`, `a1`, `a2`, plus related values).
5. Evaluate dispersion (`energy`; optionally `gradient`, `virial`, `pairwise`, properties).
6. Propagate/check errors after each API call.
7. Recreate structure/model when immutable structure attributes change.

### Minimal working example
```python
import numpy as np
from dftd4.interface import DispersionModel, DampingParam

numbers = np.array([8, 1, 1], dtype=int)
positions = np.array([
    [0.0, 0.0, -0.73578586109551],
    [1.44183152868459, 0.0, 0.36789293054775],
    [-1.44183152868459, 0.0, 0.36789293054775],
])  # Bohr

disp = DispersionModel(numbers, positions, model="d4")
res = disp.get_dispersion(DampingParam(method="pbe"), grad=True)
print(res["energy"], res["gradient"].shape)
```

```fortran
use dftd4, only : damping_param, get_rational_damping, get_dispersion
! See full runnable example: assets/examples/api-minimal-latest/app/main.f90
```

### Pitfalls/fixes
- API quantities are in atomic units; convert external coordinates/energies before calling (`doc/reference/fortran.rst`, `doc/reference/c.rst`).
- In Fortran, immutable structure fields (`nat`, `id`, `periodic`) require reconstructing dependent objects after change (`doc/reference/fortran.rst`).
- In C API, user must delete created opaque objects to avoid leaks (`doc/reference/c.rst`).
- Unchecked or overwritten error handles can hide failures; check and propagate immediately (`doc/reference/fortran.rst`, `doc/reference/c.rst`).
- Python `DampingParam` requires a method or a full explicit parameter set; partial initialization raises errors (`python/dftd4/interface.py`).

### Convergence/validation checks
- Verify array shapes: coordinates `[nat,3]`, gradient `[nat,3]`, pairwise arrays `[nat,nat]`.
- Cross-check a small test molecule through CLI and API for matching energy sign/magnitude.
- For periodic runs, confirm lattice and periodic flags are both supplied and output includes virial when requested.
- For pairwise mode, verify pairwise totals are consistent with reported total dispersion energy.

## Scope
- Handle questions about inputs, system setup, models, and physical parameterization.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/reference/c.rst`
- `doc/reference/python.rst`
- `doc/reference/fortran.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `skills/dftd4-inputs-and-modeling/references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `skills/dftd4-inputs-and-modeling/references/source_map.md` and start with the ranked source entry points.
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
- `src/dftd4/model.f90` (model construction and high-level model API)
- `src/dftd4/disp.f90` (energy/gradient/pairwise/property evaluation paths)
- `src/dftd4/param.f90` (functional lookup and damping parameter loading)
- `src/dftd4/cutoff.f90` (real-space cutoff semantics used in API calls)
- `src/dftd4/api.f90` (C API wrappers and error propagation boundary)
- `include/dftd4.h` (public C signatures and object lifecycle API)
- `python/dftd4/interface.py` (Python object model and validation rules)
- `assets/examples/api-minimal-latest/app/main.f90` (minimal Fortran reference workflow)
- `test/api/example.c` (minimal C object lifecycle and compute sequence)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app include python src test`).
