# Evidence: dftd4-inputs-and-modeling

## Primary docs
- `doc/reference/c.rst`
- `doc/reference/python.rst`
- `doc/reference/fortran.rst`

## Primary source entry points
- `skills/dftd4-inputs-and-modeling/references/doc_map.md`
- `src/dftd4/model.f90`
- `src/dftd4/reference.f90`
- `src/dftd4/model/utils.f90`
- `src/dftd4/model/type.f90`
- `src/dftd4/model/meson.build`
- `src/dftd4/model/d4s.f90`
- `src/dftd4/model/d4.f90`
- `src/dftd4/model/CMakeLists.txt`

## Extracted headings
- (none extracted)

## Executable command hints
- Python API

## Warnings and pitfalls
- - error handlers (``dftd4_error``),
- Error handling
- Error handle class
- The library provides a light error handle type (``dftd4_error``) for storing error information
- The error handle requires only small overhead to construct and can only contain a single error.
- :returns: New allocation for error handle
- Create new error handle object
- .. c:function:: int dftd4_check_error(dftd4_error error);
- :param error: Error handle
- :returns: Current status of error handle, non-zero in case of error
- Check error handle status
- .. c:function:: void dftd4_get_error(dftd4_error error, char* buffer, const int* buffersize);
