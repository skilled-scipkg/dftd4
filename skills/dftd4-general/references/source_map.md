# dftd4 source map: General

Generated from source roots:
- `app`
- `include`
- `python`
- `src`

Use this map only after exhausting topic docs in `skills/dftd4-general/references/doc_map.md`.

## Fast source navigation
- `rg -n "<symbol_or_keyword>" app include python src test`
- `rg -n "get_arguments|run_main|get_rational_damping|json_results" app src`
- `rg -n "--json|--grad|--pair-resolved|--property" app/help.f90 app/cli.f90`

## Suggested source entry points (function-level)
- `app/main.f90` | symbol: `dftd4_app` | CLI startup and top-level error flow.
- `app/cli.f90` | symbols: `get_arguments`, `get_run_arguments`, `get_param_arguments` | option parsing and dispatch.
- `app/driver.f90` | symbols: `run_main`, `run_param` | execution workflow for run/param modes.
- `app/help.f90` | symbols: `header`, `version`, `citation` | authoritative user-facing option text.
- `src/dftd4/param.f90` | symbols: `get_functionals`, `get_rational_damping_name`, `get_rational_damping_id` | functional lookup behavior.
- `src/dftd4/output.f90` | symbols: `ascii_results`, `json_results`, `ascii_pairwise` | emitted text/JSON schema behavior.
- `src/dftd4/version.f90` | version and citation metadata used by CLI output.

## Function-level behavior checks
- `test/unit/test_param.f90` | validates parameter parsing and functional aliases.
- `test/unit/test_pairwise.f90` | validates pairwise decomposition consistency.
- `app/tester.py` + `app/01-energy.json` + `app/02-gradient.json` | end-to-end CLI JSON regression harness.

## Simulation smoke commands
```sh
meson compile -C _build
python3 app/tester.py _build/app/dftd4 app/01-energy.json --func pbe app/01-ammonia.tmol
python3 app/tester.py _build/app/dftd4 app/02-gradient.json --func pbe --grad app/02-nitralin.mol
python3 app/tester.py _build/app/dftd4 app/04-pair-analysis.json --func pbe0 --pair-resolved app/04-caffeine.xyz
```

Validation checkpoints:
- All commands exit with status 0.
- JSON regression commands report no missing keys and no numeric mismatches.
