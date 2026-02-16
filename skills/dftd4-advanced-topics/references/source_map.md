# dftd4 source map: Advanced Topics

Generated from source roots:
- `app`
- `include`
- `python`
- `src`

Use this map only after exhausting topic docs in `skills/dftd4-advanced-topics/references/doc_map.md`.

## Fast source navigation
- `rg -n "--json|--grad|--pair-resolved|--property|--param|--func" app/help.f90 app/cli.f90`
- `rg -n "get_arguments|get_run_arguments|get_param_arguments|run_main|run_param" app/cli.f90 app/driver.f90`
- `rg -n "new_.*_api|get_dispersion_api|get_pairwise_dispersion_api|get_properties_api" src/dftd4/api.f90`

## Suggested source entry points (function-level)
- `app/help.f90` | symbols: `header`, `version`, `citation` | authoritative option and citation text.
- `app/cli.f90` | symbols: `get_arguments`, `get_run_arguments`, `get_param_arguments` | command-line parse/dispatch.
- `app/argument.f90` | symbols: `new_argument_list`, `make_argument_list` | response-file and token handling.
- `app/driver.f90` | symbols: `run_main`, `run_param` | runtime behavior behind CLI options.
- `src/dftd4/param.f90` | functional-name lookup used by `--func` and parameter modes.
- `src/dftd4/output.f90` | JSON/tagged/ascii output formatting behavior.
- `src/dftd4/api.f90` | C API wrappers and error propagation.
- `include/dftd4.h` | C API signatures and object lifecycle contract.
- `python/dftd4/interface.py` and `python/dftd4/library.py` | Python wrapper and CFFI boundary behavior.

## Function-level behavior checks
- `test/unit/test_param.f90` | functional lookup and parameter regression behavior.
- `test/unit/test_pairwise.f90` | pairwise output consistency checks.
- `app/tester.py` with `app/02-gradient.json` | CLI option-to-output regression.

## Advanced validation commands
```sh
_build/app/dftd4 --help
_build/app/dftd4 param --list
python3 app/tester.py _build/app/dftd4 app/02-gradient.json --func pbe --grad app/02-nitralin.mol
meson test -C _build param pairwise --print-errorlogs
```

Validation checkpoints:
- Manual options map to actual parser/runtime behavior.
- JSON regression run reproduces baseline values within tolerance.
