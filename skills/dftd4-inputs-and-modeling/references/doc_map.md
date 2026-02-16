# dftd4 documentation map: Inputs and Modeling

Generated from documentation roots:
- `doc`
- `man`
- `app`
- `assets/examples`
- `test`
- `python/dftd4`

Total docs grouped in this topic: 8

## API documentation
- `doc/reference/fortran.rst` | Fortran object model, structure semantics, and error handling.
- `doc/reference/c.rst` | C API object lifecycle, function signatures, and memory ownership.
- `doc/reference/python.rst` | Python wrapper classes and usage patterns.

## Runnable API examples
- `assets/examples/api-minimal-latest/app/main.f90` | Minimal Fortran D4 energy workflow.
- `assets/examples/api-read-coord/app/main.f90` | Fortran structure-from-file workflow.
- `test/api/example.c` | C API example covering energy, gradient, pairwise, and updates.

## Validation references
- `test/unit/test_dftd4.f90` | energy/gradient regression suite.
- `test/unit/test_periodic.f90` | periodic-system and virial/gradient checks.
