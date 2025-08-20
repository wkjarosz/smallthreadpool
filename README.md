# smallthreadpool
A single-header, portable C++11 thread pool library.

# Why?
I needed a small, portable thread pool that also gracefully simplified to single-threaded execution when compiled with emscripten without pthread support.


# Credits
The overall design is based on the article and code by [Max Liani](https://maxliani.wordpress.com/2022/07/27/anatomy-of-a-task-scheduler/), with some ideas (for the higher-level template wrappers) taken from [Wenzel Jakob's nanothread library](https://github.com/mitsuba-renderer/nanothread).

# Usage

Just `#include "smallthreadpool.h"`.

You must `#define SMALL_THREADPOOL_IMPLEMENTATION` before including the header in *one* translation unit.

See the extensive source code doxygen comments for usage info and examples.
