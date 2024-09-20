# zarg

A minimalist C library for standardi**z**ed handling of command line **arg**uments. 🚀

## Features

_zarg_ aims to be a fast, minimalist and efficient library designed for lightweight applications 🍃

-   short flags: single dash and single alphanumeric character
-   long flags: double dash and multiple alphanumeric characters
-   values: read strings behind flags and create an array
-   simple boolean flags
-   help dialogue generation

## Example

How you can use _zarg_.

```c
#include <stdio.h>
#include <stdlib.h>
#include <zarg.h>

int main(int argc, char *argv[])
{
    // long flag, short flag, flag that accepts a value?, description of flag
    Flag plus = { "add", 'a', true, "Add up numbers" };
    Flag encourage = { "encourage", 'e', false, "Encourage the user" };

    // argv, Flags[]: the last element in this array has to be the macro _FLAG to mark the end of the array
    if (zinit(argv, (Flag[]) {
              plus, encourage, _FLAG}
        ))
        return 0;

    int total = 0;

    char **numbers = flag_value(argv, plus);
    for (int i = 0; i < ppclen(numbers); i++)   // Do not use flag_count() here, this will inevitably lead to a memory leak.
        total += atoi(numbers[i]);
    free(numbers);              // Always call free() on the return of flag_value()

    printf("Your numbers sum up to %d!\n", total);

    if (flag_passed(argv, encourage))
        printf
            ("Great job! That's how one should pass command line arguments!\n");

    return 0;
}
```

Compile.

```
$ gcc -Wall -fstack-protector -lzarg myzarg.c -o myzarg
```

Then execute:

```
$ ./myzarg --help
./myzarg [option]

Options
--help, -h        Show this dialogue
--add, -a [value] Add up numbers
--encourage, -e   Encourage the user
```

… or

```
$ ./myzarg --add 2 -a 40
Your numbers sum up to 42!
```

If you've had a bad day, add `--encourage`!

```
$ ./myzarg --encourage -a 40 -a 2
Your numbers sum up to 42!
Great job! That's how one should pass command line arguments!
```

## Installation

### Prerequisites

-   make (_v4.4+_)
-   gcc (_v14.1+_)

### Compiling

To compile libzarg using make.

```
$ make build/libzarg.so
```

After building libzarg, execute the `install` rule with root privileges to install libzarg to your system.

```
# make install
```

## Removal

Execute the `uninstall` rule with root privileges to completely remove libzarg from your system.

```
# make uninstall
```

## Testing

Follow the installation instructions. Once finished, build the base testing binary using make.

```
$ make build/test
```

Now run the automated testing script

```
$ make test
```

… or test manually.

```
$ ./build/test --help
```

## Documentation

To better understand functions and code provided by this library, open [this site](https://konstantintutsch.com/zarg/globals.html) with your browser of choice.


If you want to view the documentation offline, use [Doxygen](https://doxygen.nl). Just run the `doc` rule with make.

```
$ make doc
```

You can now view all documentation locally. A good entry point is `./build/doc/html/globals.html`.
