libarg3format
=============

[![Build Status](http://img.shields.io/travis/deadcoda/arg3format.svg)](https://travis-ci.org/deadcoda/arg3format)
[![Coverage Status](https://coveralls.io/repos/deadcoda/arg3format/badge.svg?branch=master&service=github)](https://coveralls.io/github/deadcoda/arg3format?branch=master)
[![License](http://img.shields.io/:license-mit-blue.svg)](http://deadcoda.mit-license.org)

A c++11 class for formatting strings.

Examples
--------

```c++

cout << format("{0} walked up {1} miles and saw {2}.", "A bear", 20, "an eagle");

```

using argument methods in different order

```c++
format f("{2} saw {0} {1}!");

f.args(20, "eagles"); // inserts {0} and {1}

f.args("A bear"); // inserts {2}

string str = f; // will equal "A bear saw 20 eagles!"
```

specify type formatting:

```c++
format f("{0:f2}", 123.45278);

f.str() == "123.45"; // floating point with precision 2

format f("{0:X}", 10);

f.str() == "0A"; // uppercase hexidecimal

```

or:

```c++
format f("{0,8} h2", "h1");

f.str() == "      h1 h2"; // right aligned spacing

format f("{0,-8:f2} cm", 23.45213);

f.str() == "23.45   cm"; // left aligned spacing

```

the class will throw *invalid_argument* exception on errors:

```c++

format f("{2} arg"); // throws an exception. no 0 or 1 specifier


format f("{0}", "test", "two"); // throws an exception. missing specifier

```

Building
--------

After cloning run the following command to initialize submodules:

```bash

git submodule update --init --recursive
```

you can use [cmake](https://cmake.org) to generate for the build system of your choice.

```bash
mkdir debug; cd debug; 
cmake -DCMAKE_BUILD_TYPE=Debug ..
make
make test
```

a homebrew release example:
```bash
mkdir release; cd release
cmake -DCMAKE_BUILD_TYPE=Release $(cd ..;brew diy --version=0.2.0)
make
make install
brew link arg3json
```

options supported are:

    -DCODE_COVERAGE=ON   :   enable code coverage using lcov
    -DMEMORY_CHECK=ON    :   enable valgrind memory checking on tests


Coding Style
------------

- class/struct/method names are all lower case with underscores separating words
- non public members are camel case with and underscore at end of the name
- macros, enums and constants are all upper case with underscores seperating words
- braces on a new line


formatting
----------

I borrowed the feel of [composite formating](http://msdn.microsoft.com/en-us/library/txafckwd.aspx) from c#.

basic structure I use here is:

    +------------------> opening tag
    |    +-------------> width seperator
    |    |    +--------> format seperator
    |    |    |
    {  0 , -8 : F 2 } -> closing tag

       |   |    | |
       |   |    | +----> formating argument, variable length, reserved for future use
       |   |    +------> type of formating, see below
       |   +-----------> width of argument, integer, optional negative sign indicating left align
       +---------------> argument index, integer


format types
------------

- **x**: lower case hexidecimal
- **X**: upper case hexidecimal
- **o**: octal
- **f**: floating point values
- **e**: scientific floating point
- **E**: uppercase scientific floating point

some examples are:

```
{0,-8}  // left align value with a width of 8
{1,4:X} // align right a hexidecmal value with a width of 4
{2:E}   // scientific floating point
```

