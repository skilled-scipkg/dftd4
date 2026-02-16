---
name: dftd4-recipe
description: This skill should be used when users ask about recipe in dftd4; it prioritizes documentation references and then source inspection only for unresolved details.
---

# dftd4: Recipe

## High-Signal Playbook
### Route conditions
- Use this skill for stepwise workflows in official recipes, especially VASP integration (`doc/recipe/index.rst`, `doc/recipe/vasp.rst`).
- Route generic compile/install issues to `dftd4-build-and-install`.
- Route API object semantics to `dftd4-inputs-and-modeling`.

### Triage questions
- Is the target standalone `dftd4` usage or linking DFT-D4 into VASP?
- Are DFT-D4 and VASP built with the same Fortran compiler (`doc/recipe/vasp.rst`)?
- Was API compatibility mode enabled (`api_v2`)?
- Does `pkg-config --modversion dftd4` work in the VASP build environment?
- Are transitive dependencies already present on VASP link line?

### Canonical workflow
1. Build/install DFT-D4 first (Meson or CMake path).
2. For VASP linkage, enable compatibility layer (`-Dapi_v2=true` or `-DWITH_API_V2=ON`).
3. Ensure VASP uses the same Fortran compiler used for DFT-D4.
4. Verify installation visibility with `pkg-config --modversion dftd4`.
5. If needed, export module/env variables so `pkg-config`, include paths, and libraries resolve.
6. Add D4 flags/libs/includes to VASP make configuration.
7. If linking still fails, append `-lmulticharge -lmctc-lib -lmstore`.
8. Run standalone and coupled smoke calculations.

### Minimal working example
```sh
meson setup _build -Dapi_v2=true --prefix="$HOME/opt/dftd4"
meson compile -C _build
meson test -C _build api-test --print-errorlogs
meson install -C _build
pkg-config --modversion dftd4
_build/app/dftd4 --func pbe0 app/04-caffeine.xyz
```

```make
CPP_OPTIONS += -DDFTD4
LLIBS       += $(shell pkg-config --libs dftd4) -lmulticharge -lmctc-lib -lmstore
INCS        += $(shell pkg-config --cflags dftd4)
```

### Pitfalls/fixes
- DFT-D4 and VASP built with different Fortran compilers leads to ABI/link failures; rebuild with the same compiler (`doc/recipe/vasp.rst`).
- Forgetting compatibility mode (`api_v2`) breaks expected 2.5.x VASP interface behavior.
- `pkg-config` cannot locate installation; export `PKG_CONFIG_PATH` (module-file pattern in `doc/recipe/vasp.rst`).
- Linker misses transitive deps; add `-lmulticharge -lmctc-lib -lmstore` (`doc/recipe/vasp.rst`).
- Library installed but headers/libs on non-standard `lib64` vs `lib`; adjust module variables to actual install tree.

### Convergence/validation checks
- `pkg-config --modversion dftd4` succeeds in the same shell used for VASP build.
- `pkg-config --cflags --libs dftd4` returns non-empty include/lib flags.
- VASP build line contains `-DDFTD4` and DFT-D4 link flags.
- Standalone `_build/app/dftd4 --func pbe0 app/04-caffeine.xyz` and VASP-linked behavior are both reproducible.

## Scope
- Handle questions about documentation grouped under the 'recipe' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `doc/recipe/index.rst`
- `doc/recipe/vasp.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `skills/dftd4-recipe/references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `skills/dftd4-recipe/references/source_map.md` and start with the ranked source entry points.
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
- `src/dftd4/compat.f90` (`d4_calculation`, `d4par` for 2.5.x compatibility path)
- `src/dftd4/api.f90` (current API surface and symbol behavior)
- `include/dftd4.h` (public C ABI used by downstream projects)
- `meson_options.txt` (`api_v2` option and build-time toggles)
- `app/driver.f90` (runtime workflow for recipe smoke checks)
- `app/help.f90` (CLI option text synchronized with recipe usage)
- `src/dftd4/param.f90` (method parameter loading path for recipe examples)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" app include python src test`).
