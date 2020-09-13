---
layout: post
title: "Header files names in C, C++, and POSIX"
tag: cpsc351
---

A student in [CPSC 351](https://sites.google.com/view/cpsc-351/fall-2020) asks:

> I'm trying to use the `<errno>` header file, and I get the following error:
> ```shell-session
> $ c++ test.cpp
> test.cpp:3:10: fatal error: errno: No such file or directory
>     3 | #include <errno>
>       |          ^~~~~~~
> compilation terminated.
> $
> ```

## The short answer

Use this instead:
```c++
#include <cerrno>
```

## The long, complicated answer

If you're programming in C, or including a C header file in a C++ program, you need to include the file extension `.h`.  So you'll see in the documentation for [`errno(3)`](https://man7.org/linux/man-pages/man3/errno.3.html) that you need to do:
```c
#include <errno.h>
```
The C++ convention is to not include the `.h` extension. But since it's a C header file,
```c++
#include <errno>
```
won't work.

You could add the `.h`, but you shouldn't. `errno` is part of the C standard library (which you can tell because it's in section 3 of the manual). Since C++ was originally based on C, there are versions of all the C standard library header files included in the C++ standard library, but the convention in that library is to drop the `.h` suffix and prefix the filename with the letter `c` to show that it came from the C standard library. So you should use:
```c++
#include <cerrno>
```
to get `errno`. You can see this convention at work in the [sample code](https://gist.github.com/ProfAvery/590490757ffb8a02458999ee4c271e54), where I wrote:
```c++
#include <cstdlib>
```
to bring in the constants `EXIT_SUCCESS` and `EXIT_FAILURE`. If I were programming in C I'd do this instead:
```c
#include <stdlib.h>
```

### What about header files from other libraries?

Given what I've just said, you may be wondering why the sample code has lines like this:
```c++
#include <unistd.h>
```
instead of something like this:
```c++
#include <cunistd>
```
That's because `<unistd.h>` isn't part of the C or C++ standard library. It's part of the POSIX API, which includes many (but not all) Linux [system calls](https://man7.org/linux/man-pages/man2/intro.2.html). So it's not in either the C or C++ standard libraries, and since [UNIX programmers tend to use C](http://www.catb.org/~esr/writings/taoup/html/ch14s04.html#c_language), there's only a single version of the header, and it ends in `.h`.

