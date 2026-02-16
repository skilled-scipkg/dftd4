# dftd4 source map: Reference

Generated from source roots:
- `app`
- `include`
- `python`
- `src`

Use this map only after exhausting topic docs in `skills/dftd4-reference/references/doc_map.md`.

## Fast source navigation
- `rg -n "class DFTD4|run_qcschema|class DFTD4Dispersion|def energy|def grad" python/dftd4`
- `rg -n "pair_resolved|params_tweaks|level_hint|model" python/dftd4/qcschema.py python/dftd4/ase.py`
- `rg -n "get_dispersion|get_pairwise_dispersion|get_properties" python/dftd4/interface.py src/dftd4/api.f90`

## Suggested source entry points (function-level)
- `python/dftd4/ase.py` | symbol: `class DFTD4` | ASE calculator integration and parameter translation.
- `python/dftd4/qcschema.py` | symbol: `run_qcschema` | QCSchema request parsing and result shaping.
- `python/dftd4/pyscf.py` | symbols: `class DFTD4Dispersion`, `energy`, `grad` | PySCF hooks.
- `python/dftd4/interface.py` | symbols: `Structure`, `DampingParam`, `DispersionModel` | shared Python object model.
- `python/dftd4/library.py` | CFFI-level bridge to exported C API.
- `src/dftd4/api.f90` | Fortran implementation behind C/Python adapter calls.
- `include/dftd4.h` | C API signatures used by Python bindings.

## Function-level behavior checks
- `python/dftd4/test_ase.py` | ASE energy/force regression checks.
- `python/dftd4/test_qcschema.py` | QCSchema energy/gradient/pairwise/property and error-path checks.
- `python/dftd4/test_pyscf.py` | PySCF molecular and PBC behavior checks.
- `python/dftd4/test_interface.py` | shared interface behavior used by all adapters.

## Adapter validation commands
```sh
python -m pytest -q python/dftd4/test_interface.py
python -m pytest -q python/dftd4/test_qcschema.py
python -m pytest -q python/dftd4/test_ase.py python/dftd4/test_pyscf.py
```

Validation checkpoints:
- No import/runtime errors for enabled adapters.
- Reported energies are finite and agree with test references.
