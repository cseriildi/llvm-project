### **Summary**

This PR adds more symbol-header mappings that `clang-include-cleaner`, `clangd` and `clang-tidy` use to detect symbols that are used but not directly included ([following the include-what-you-use model](https://clangd.llvm.org/guides/include-cleaner)).

* **Add mappings for missing POSIX symbols** that are currently not handled. These mappings are placed in new dedicated files to avoid mixing non-standard elements with the ISO C/C++ standard library mappings.

  * Addresses and **closes multiple open issues**, including:
    * Closes https://github.com/llvm/llvm-project/issues/64336 – `timeval`
    * Closes https://github.com/llvm/llvm-project/issues/76567 – `strsignal`
    * Closes https://github.com/llvm/llvm-project/issues/89844 & closes https://github.com/llvm/llvm-project/issues/120830 – `signal`
    * Closes https://github.com/llvm/llvm-project/issues/134818 – `errno`

* **Update the existing generated mapping files** using [`gen_std.py`](https://github.com/llvm/llvm-project/blob/main/clang/tools/include-mapping/gen_std.py) and [this archive](https://en.cppreference.com/w/Cppreference%253AArchives.html).

* Move symbols that were previously detected by the Python script but now not anymore to `*SpecialSymbolMap.inc`, as these mappings are still needed.

Example diagnostic message from `clangd`:<br>
`No header providing "pthread_t" is directly included (fixes available) - clangd(missing-includes)`

---

### **Questions**

We would appreciate input on the following points to make sure we’re building in the right direction:

1. **Is the existing proposal in https://github.com/llvm/llvm-project/pull/66089 still planned to be completed?**

   * It appears to introduce a more general solution that aligns closely with the logic of [IWYU](https://github.com/include-what-you-use/include-what-you-use/blob/master/iwyu_include_picker.cc#L346) and could potentially cover both standard and POSIX symbols in a unified way. Basically, it maps private headers to public headers, instead of symbols to public headers. This might simplify future contributions and extensions.

2. **Is it appropriate to maintain POSIX mappings in separate files**, or should we integrate them into the existing mapping files despite them not being part of the standard library?

    * Is it ok to map some POSIX symbols (e.g. strsignal) also to C++ headers (<cstring>), or should they map to C headers (<string.h>) only?

3. Regarding `gen_std.py`: **Should an issue be opened to look into why the script finds less and less symbols?**

   * We noticed the script removed several entries that are still needed. Based on previous commits, it seems this is typically resolved by moving such entries to the *SpecialSymbolMap.inc files, so we did the same.

Thanks in advance for your time and feedback!

