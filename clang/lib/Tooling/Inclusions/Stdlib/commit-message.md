[include-cleaner] Add POSIX symbol mappings and update generated include maps

- Add missing POSIX symbol-to-header mappings (e.g., `timeval`, `strsignal`, `signal`, `errno`) to new dedicated mapping files to avoid mixing with standard library mappings.
- Update generated `*SymbolMap.inc` files using `gen_std.py` and the latest cppreference archive.
- Move removed but still-needed entries to `*SpecialSymbolMap.inc` files to retain coverage.
- Closes several related issues: #64336, #76567, #89844, #120830, #134818.

Co-authored-by: itislu <itislu.git@gmail.com>
