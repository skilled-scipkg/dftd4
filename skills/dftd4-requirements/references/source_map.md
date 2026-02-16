# dftd4 source map: Requirements

Generated from source roots:
- `app`
- `include`
- `python`
- `src`

Use this map only after exhausting topic docs in `skills/dftd4-requirements/references/doc_map.md`.

## Fast source navigation
- `rg -n "option\(|get_option\(" meson_options.txt meson.build python/meson.build`
- `rg -n "cffi|numpy|pkg-config|pkgconfig" python/pyproject.toml python/meson.build python/ffibuilder.py`
- `rg -n "importlib|ffi|dlopen|new_structure" python/dftd4/library.py python/dftd4/interface.py`

## Suggested source entry points (requirement behavior)
- `meson_options.txt` | feature gates affecting dependency requirements.
- `meson.build` | core dependency checks and optional components.
- `python/pyproject.toml` | Python build backend and declared Python package requirements.
- `python/meson.build` | extension build requirements and link setup.
- `python/ffibuilder.py` | required C symbols exposed to Python layer.
- `python/dftd4/library.py` | runtime shared-library loading and error reporting.
- `python/dftd4/interface.py` | runtime argument/shape checks that surface dependency issues.

## Function-level behavior checks
- `python/dftd4/test_library.py` | validates library-load and API availability behavior.
- `python/dftd4/test_parameters.py` | validates parameter database loading behavior.
- `python/dftd4/test_interface.py` | validates wrapper calls and data-shape requirements.
- `python/dftd4/test_ase.py`, `python/dftd4/test_qcschema.py`, `python/dftd4/test_pyscf.py` | optional dependency behavior.

## Requirement validation commands
```sh
python -m pip install -r doc/requirements.txt
meson setup _build -Dpython=true -Dpython_version="$(command -v python3)"
meson compile -C _build
python -m pytest -q python/dftd4/test_library.py python/dftd4/test_parameters.py python/dftd4/test_interface.py
```

Validation checkpoints:
- Core Python tests pass without optional framework dependencies.
- Optional adapter tests are only enabled when matching packages are installed.
