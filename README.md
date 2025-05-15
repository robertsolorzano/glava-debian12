## Compatibility Fixes for Debian 12 (GCC 12+)

This fork includes a few changes to get GLava building and running cleanly on Debian 12 with modern GCC. The original project had issues due to outdated header usage and missing `extern` declarations that broke the build on stricter compilers.

### Files Changed

#### `glfft/glfft_gl_interface.hpp`

  ```cpp
  extern "C" {
      #include <stdlib.h>
      #include <string.h>
      #include <error.h>
      #include <errno.h>
      #include <stdio.h>
  }

  //Add these C++ headers:
  #include <cstdio>
  #include <cerrno>
  #include <cstdarg>
  #include <stdexcept>
```

#### `glfft/glfft_gl_interface.cpp`

* Add these headers:

  ```cpp
  #include <cstdio>
  #include <cstdarg> 
  ```

#### `glfft/glfft_common.hpp`

* Add missing include for exception support:

  ```cpp
  #include <stdexcept>
  ```

#### `glava/glava.h`

* Fixed function pointer declarations to include `extern`:

  ```c
  __attribute__((noreturn, visibility("default"))) extern void (*glava_abort)(void);
  __attribute__((noreturn, visibility("default"))) extern void (*glava_return)(void);
  ```

Without `extern`, these were generating linker issues.

---

### Summary

If you ran into the same build issues I did this should save you some time. This repo includes the all files needed to get GLava compiled and running on Debian 12 with nothing else changed.

Original project: [GLava on GitHub](https://github.com/jarcode-foss/glava)

---
