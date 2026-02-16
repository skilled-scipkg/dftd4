# dftd4 source map: Recipe

Generated from source roots:
- `app`
- `include`
- `python`
- `src`

Use this map only after exhausting topic docs in `skills/dftd4-recipe/references/doc_map.md`.

## Fast source navigation
- `rg -n "api_v2|WITH_API_V2|compat" meson_options.txt CMakeLists.txt src/dftd4/compat.f90`
- `rg -n "d4_calculation|d4par" src/dftd4/compat.f90`
- `rg -n "new_d4_model_api|get_dispersion_api" src/dftd4/api.f90 include/dftd4.h`

## Suggested source entry points (function-level)
- `src/dftd4/compat.f90` | symbols: `d4_calculation`, `d4par` | legacy 2.5.x compatibility behavior for VASP bridges.
- `src/dftd4/api.f90` | C API wrappers used by external coupling code.
- `include/dftd4.h` | downstream ABI/API contract used in external builds.
- `meson_options.txt` | `api_v2` option enabling compatibility layer.
- `src/CMakeLists.txt` | CMake-side option wiring for API and compatibility build.
- `app/driver.f90` | standalone smoke path to validate runtime after integration build.

## Function-level behavior checks
- `test/api/example.c` | C API lifecycle sanity check comparable to external coupling flows.
- `test/api/meson.build` | CI-facing API smoke test registration.
- `app/help.f90` | verify recipe command examples match actual CLI options.

## Recipe validation commands
```sh
meson setup _build -Dapi=true -Dapi_v2=true
meson compile -C _build
meson test -C _build api-test --print-errorlogs
_build/app/dftd4 --func pbe0 app/04-caffeine.xyz
pkg-config --cflags --libs dftd4
```

Validation checkpoints:
- `api-test` passes with compatibility mode enabled.
- CLI smoke run succeeds in the same toolchain environment used for integration.
