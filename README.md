# SmallThreadPool
A single-header, portable C++11 thread pool library.

# Why?
I needed a small, portable thread pool that also gracefully simplified to single-threaded execution when used with 0 threads or compiled with emscripten without pthread support. Also, SmallThreadPool is small -- about 1000 lines of heavily doxygen-commented code.


# Credits
The overall design is based on the article and code by [Max Liani](https://maxliani.wordpress.com/2022/07/27/anatomy-of-a-task-scheduler/), with some ideas (for the higher-level template wrappers) taken from [Wenzel Jakob's nanothread library](https://github.com/mitsuba-renderer/nanothread).

# Usage

Just
```cpp
#include "smallthreadpool.h"
```
to get started.

In *one* translation unit (source file), you must `#define SMALL_THREADPOOL_IMPLEMENTATION` before including the header.

One of the core constructs is the parallel for-loop. Here's an example of how to use it:
```cpp
#include <spdlog/spdlog.h>
#define SMALL_THREADPOOL_IMPLEMENTATION
#include <smallthreadpool.h>

int main(int, char **) {
    int result[100];

    // Call the provided lambda function 100 times with blocks of size 1
    stp::parallel_for(
        blocked_range<uint32_t>(0   // begin,
                                100 // end,
                                1   // block_size
                                ),

        // The callback is allowed to be a stateful lambda function
        [&](uint32_t begin, uint32_t end, int unit_index, int thread_index) {
            for (uint32_t i = begin; i != end; ++i) {
                spdlog::info("Worker thread {} is starting to process work unit {}\n",
                             pool_thread_id(), i);

                // Write to variables defined in the caller's frame
                result[i] = i;
            }
        }
    );
}
```

There are a number of other constructs provided by `smallthreadpool`, including a `do_async` function to perform a single computation asynchronously on the thread pool, a `Barrier` thread-coordination mechanism class, and a `for_each_thread` function to execute a function once for each thread in the thread pool. See the extensive source code doxygen comments for more usage info and examples.
