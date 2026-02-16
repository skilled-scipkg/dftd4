# Evidence: dftd4-recipe

## Primary docs
- `doc/recipe/index.rst`
- `doc/recipe/vasp.rst`

## Primary source entry points
- `skills/dftd4-recipe/references/doc_map.md`
- `src/dftd4.f90`
- `include/dftd4.h`
- `src/meson.build`
- `src/CMakeLists.txt`
- `app/main.f90`
- `app/meson.build`
- `app/help.f90`
- `app/driver.f90`
- `app/CMakeLists.txt`
- `app/cli.f90`
- `app/argument.f90`
- `src/dftd4/model.f90`
- `src/dftd4/api.f90`
- `src/dftd4/version.f90`
- `src/dftd4/utils.f90`
- `src/dftd4/reference.f90`
- `src/dftd4/param.f90`
- `src/dftd4/output.f90`
- `src/dftd4/numdiff.f90`

## Extracted headings
- (none extracted)

## Executable command hints
- (none extracted)

## Warnings and pitfalls
- .. important::
- It is important to build ``dftd4`` with the same Fortran compiler you build Vasp with.
