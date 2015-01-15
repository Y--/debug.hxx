# debug

> tiny, C++11 header-only debug utility inspired to TJ Holowaychuk node and go debug libraries. Tested with clang.

## Installation

Just drop this file wherever you want and include it.

## Usage

With `debug` you simply invoke `Debug("your_module_name")` to generate your debug function. The debug function will log to cerr the string passed as parameter by prefixing it with the module name. By default, module names are printed in colors. You can disable this with the env variable `DEBUG_COLORS=no`.

## Example

Assume you have two c++ modules:

```c++
#include "debug.hxx"

auto debug = Debug("test");

void secondfunction() {
  debug("This is just another function");
}
```

and

```c++
#include <iostream>
#include "debug.hxx"

auto debug = Debug("main");

using namespace std;

extern void secondfunction();

int main(int argc, char const *argv[])
{
  debug("this is a main`s message");
  secondfunction();
  return 0;
}
```

You use the the __DEBUG__ environment variable to enable these based on a regular expression. Here are some examples:

![](https://dl.dropboxusercontent.com/u/5867765/images/debug_hxx.png)

## Legacy npm debug behavior

You can separate module names with commas `,`. These are automatically converted to a regex or:

```
DEBUG=test,main ...
```

is equal to

```
DEBUG=test\|main 
```

## Wrapper for default values

A default `debugm` macro wraps the lambda generated by `Debug` to include file and line number. Assume the following `main.cpp`

```c++
#include <iostream>
#include "debug.hxx"

using namespace std;

extern void secondfunction();

int main(int argc, char const *argv[])
{
        debugm("this is main");
        return 0;
}
```

then:

```bash
> DEBUG_COLORS=no DEBUG=* ./bin/test2 
 test/test3.cpp:10 this is main
```

## Time output

To enable time measurements between debug messages associated with
the same namespace (or the same line of code when using `debugm`), use `DEBUG_TIME=yes`:

```c++
/* example.cpp */
#include <iostream>
#include "debug.hxx"
#include <unistd.h>

using namespace std;

int main(int argc, char const *argv[])
{
    for(int i=0; i<4; i++) {
        debugm("debug printed.");
        sleep(1);
    }
        return 0;
}
```

Run it with: 

```
> DEBUG_COLORS=no DEBUG=* DEBUG_TIME=yes $srcdir/../bin/test6

 test/test6.cpp:10 debug printed. +0ms
 test/test6.cpp:10 debug printed. +1002ms
 test/test6.cpp:10 debug printed. +2003ms
 test/test6.cpp:10 debug printed. +3005ms
```



## Conventions

If you're using this in one or more of your libraries, you _should_ use the name of your library so that developers may toggle debugging as desired without guessing names. 

## Tests

```
make clean && make test
```

## Authors

- Vittorio Zaccaria

## Previous work

- TJ Holowaychuk
- Nathan Rajlich


## License

(The MIT License)

