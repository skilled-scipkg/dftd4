# dftd4 source map: Inputs and Modeling

Generated from source roots:
- `app`
- `include`
- `python`
- `src`

Use this map only after exhausting topic docs in `skills/dftd4-inputs-and-modeling/references/doc_map.md`.

## Fast source navigation
- `rg -n "new_d4_model|new_d4s_model|new_dispersion_model" src/dftd4/model*.f90 src/dftd4/model/*.f90`
- `rg -n "get_dispersion|get_properties|get_pairwise_dispersion" src/dftd4/disp.f90 src/dftd4/api.f90`
- `rg -n "class Structure|class DampingParam|class DispersionModel" python/dftd4/interface.py`

## Suggested source entry points (function-level)
- `src/dftd4/model.f90` | symbol: `new_dispersion_model` | high-level model factory.
- `src/dftd4/model/d4.f90` | symbols: `new_d4_model`, `weight_references`, `get_atomic_c6` | D4-specific model behavior.
- `src/dftd4/model/d4s.f90` | symbols: `new_d4s_model`, `weight_references`, `get_atomic_c6` | D4S-specific model behavior.
- `src/dftd4/disp.f90` | symbols: `get_dispersion`, `get_properties`, `get_pairwise_dispersion` | main compute kernels.
- `src/dftd4/numdiff.f90` | symbol: `get_dispersion_hessian` | numerical Hessian path.
- `src/dftd4/cutoff.f90` | symbol: `get_lattice_points_cutoff` | periodic cutoff translation generation.
- `src/dftd4/param.f90` | damping-parameter load and functional-name resolution.
- `src/dftd4/api.f90` | C API wrappers for structure/model/parameter lifecycle and evaluation.
- `include/dftd4.h` | public C signatures and ownership contract.
- `python/dftd4/interface.py` | Python-side validation and object wrappers over C API.

## Function-level behavior checks
- `test/unit/test_model.f90` | validates reference-weight and polarizability paths.
- `test/unit/test_dftd4.f90` | validates energies/gradients for named functionals.
- `test/unit/test_pairwise.f90` | validates pairwise decomposition vs total energy.
- `test/unit/test_periodic.f90` | validates periodic handling and virial behavior.
- `python/dftd4/test_interface.py` | validates Python array shape/unit behavior and API calls.

## Simulation and validation commands
```sh
meson compile -C _build
meson test -C _build model dftd4 pairwise periodic api-test --print-errorlogs
_build/app/dftd4 --func pbe0 --json app/04-caffeine.xyz
```

Validation checkpoints:
- Targeted suites pass for model, dispersion, pairwise, and periodic behavior.
- CLI JSON output exists and includes finite `energy`.
