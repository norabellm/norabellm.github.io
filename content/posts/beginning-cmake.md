---
title: "Beginning CMake : Quick Getting Started with CMake"
date: 2023-09-29T22:31:20+07:00
draft: false
tags: ['cmake', 'c++']
---

CMake is build system intented for organize C/C++ project. Not only saving me from write long
 compiler command to build program, it also become project definition. Cmake handle dependencies, build, compiler selection, testing and many others. If you familiar with GNU Make, this may the niche version of it that specialize to C/C++ code.

CMake works by compiling `CMakeList.txt` (Your project definition) into native build system such 
Makefile, ninja, meson or other. So, you do not have to worry when one of the tool not exist in 
your environment.

This post is about my quick note using CMake such creating then building project and keeping dependencies in C/C++ project. To install CMake, you could use your OS package manager or [grab the binary here](https://cmake.org/download/).

## Making first project

Everything in CMake is defined in `CMakeLists.txt` file, yes it `.txt`. You only need make it at the root of your project directory.

Lets create folder structure like this:

```txt
+ first-project
    +- src
        - init.cc
    - CMakeLists.txt
```

Put these code to `init.cc`, so we can see the output.

```cc
#include <iostream>

int main()
{
    std::cout << "Hello, UwU" << std::endl;
    return 0;
}

```

Now, pour some text into the `CMakeList.txt`,

```cmake
cmake_minimum_required(VERSION 3.11)

project(First-Project)
add_executable(First-Exe src/init.cc)
```

Explanation:

- `cmake_minimum_required`, stating minimal CMake version required to run the project, in this case is `3.11`.
- `project`, declaring your project name, your could change `First-Project` with any name you like.
- `add_executable`, this part where you specify your source and header file path to produce binary. Each file sparated by whitespace, such `First-Exe src/init.cc src/init.hh`. `First-Exe` will become your executable output name and you can change it as you like.

### Build the CMake

The best pratice of build using CMake is making a directory inside project to run build proses. 
Let say the directory is `build` and we enter that folder as working directory (`cd build`). 

`build` folder needed to store result of compilation while keeping   source code not poluted by build artifacts in compilation phase.

Here our project structure look like:

```txt
+ first-project
    +- build
    +- src
        - init.cc
    - CMakeLists.txt
```

After creating the build folder and `cd`-ing, then run `cmake ../`. These step CMake will compiling the `CMakeLists.txt` into instruction script for build system that available in your system such GNU make or ninja that already supplied with. 

If you run in Linux ecosystem, your probably ended using GNU make. If Mac your probably use ninja. Lets assume you using Linux then you need run `make` after CMake finish build receipt for you.

After finish buliding, your will found the `First-Exe` binary in `build` folder.

### Adding new file

Let say you want to add new file your project. Here an example, you want to add folder in `utils` with that contain `utils.hh` and `utils.cc` file. 

```txt
+ first-project
    +- build
    +- src
        +- utils
            - utils.cc
            - utils.hh
        - init.cc
    - CMakeLists.txt
```

In `CMakeLists.txt` you should add the new file path in `add_executable` section.

```cmake
cmake_minimum_required(VERSION 3.11)

project(First-Project)
add_executable(First-Exe 

    src/utils/utils.hh
    src/utils/utils.cc

    src/init.cc
)
```

At `init.cc`, you could include the header file like this:

```cc
#include <iostream>
#include "utils/utils.hh"

int main()
{
    std::cout << "Hello, UwU" << std::endl;
    return 0;
}
```

## Dependencies

There serveral way to import library to CMake. There four easy way to do that, using `find_package` command, importing another CMake project, convert source code into CMake project or use Vcpkg.

Keep in mind, CMake always build the shared object of imported library for linking proccess and it include header files.

### find_package

This is the simple way to import dependencies. CMake will check path listed in `CMAKE_MODULE_PATH` environment variable then match each directory to find your library. 

There an advantage of this method. You can install dependencies using your OS package manager (such, libXX or libXX-devel) then use `find_package` command to use it. 

This way quite fast if you lazy, but in my opinion it quite dirty to polute OS library and 
less portable when you need compile at another machine with different package manager consensus.

```cmake
cmake_minimum_required(VERSION 3.11)

project(First-Project)

find_package(CURL Required)

add_executable(First-Exe 

    src/utils/utils.hh
    src/utils/utils.cc

    src/init.cc
)

target_link_libraries(First-Exe PUBLIC CURL)
```

Let me explain code above. I add `find_package(CURL)` to import `curl` library that installed (`libcurl` for shared object and `libcurl-dev` for inculde header files) and `target_link_libraries(First-Exe PUBLIC CURL)` to linking the shared object against `First-Exe` object in order making complete executable that resolve the `curl` shared library.

Then include it into your source code -- in this case is `init.cc`,

```cc
#include <iostream>
#include "utils/utils.hh"
#include <curl/curl.h>

...
```

> P.S: include using `<>` meaning resolve file from outside project such `curl` or etc. include using `""` meaning resolve file from your local project directory such `utils/utils.hh` that exist inside `src` folder along with the `init.cc`.

After that do the build,

```
$ cd build
$ cmake ../
$ make
$ ./First-Exe
```

### Importing another CMake project

Importing another CMake project almost like adding file to `add_executable` command. To do this, you
should grab the source code you want import to your project. For convinient, create `vendor` directory then put the source code of CMake project you want to import.

For example, let assume you had `machineid` folder that contain CMake project of machineid library 
inside `vendor` directory.

```txt
+ first-project
    +- build
    +- src
        +- utils
            - utils.cc
            - utils.hh
        - init.cc
    +- vendor
        +- machineid
            - ....
            - CMakeLists.txt
    - CMakeLists.txt
```

Make sure the source code directory contain `CMakelists.txt` file. Then at 
`CMakeLists.txt` in root folder we add `add_subdirectory` command.

```cmake
cmake_minimum_required(VERSION 3.11)

project(First-Project)

add_subdirectory(vendor/machineid)

add_executable(First-Exe 

    src/utils/utils.hh
    src/utils/utils.cc

    src/init.cc
)

target_link_libraries(First-Exe PUBLIC machineid)
```

`add_subdirectory` argument is path of the library source code.

The last folder name at `add_subdirectory` will be name of project that will used for linking program. So, I add `machineid` to `target_link_libraries` for linking aginst `First-Exe` object code.

### Import the source

There are two ways:

- Compile the project and install it to system, then use `find_package` command to import the library into your project.

- If the source code is not using CMake, you need to convert it. This section is quite big and I never done it. I suggest you to read this, [Converting Existing System to CMake](https://cmake.org/cmake/help/book/mastering-cmake/chapter/Converting%20Existing%20Systems%20To%20CMake.html).

### Combine Vcpkg with CMake

This most clean and most convinient way to add or even manage dependencies. Vcpkg handle 
how you obtain the library you need and build it then CMake just use from it.

Vcpkg is package manager for handle C/C++ libraries, it almost same like conan or `pip` in Python.

Installing package in Vcpkg is quite simple, just `vcpkg install <your library>`. You could explore available package at [https://vcpkg.io/en/packages](https://vcpkg.io/en/packages).

After installing the package, then we need import it to our project by `find_package` command.

Here a case, I want to install `curl` library and import it to my project.

Look at instruction of how to do it:

- Install from vcpkg, run `vcpkg install curl`

- Put the `find_package` to `CMakeLists.txt`.

```cmake
cmake_minimum_required(VERSION 3.11)

project(First-Project)

find_package(CURL Required)

add_executable(First-Exe 

    src/utils/utils.hh
    src/utils/utils.cc

    src/init.cc
)

target_link_libraries(First-Exe PUBLIC CURL)
```

- Then, move to `build` folder, and run these:

```sh
cmake ../ -DCMAKE_TOOLCHAIN_FILE=[path to vcpkg]/scripts/buildsystems/vcpkg.cmake 
```

- Finally, build the binary, using `make`.

`-DCMAKE_TOOLCHAIN_FILE=[path to vcpkg]/scripts/buildsystems/vcpkg.cmake` is argument for connecting
CMake with Vcpkg package consensus so `find_project` be able to resolve the library. 

`[path to vcpkg]` is your vcpkg installation path.

## Conclusion

The step above is my workflow when dealing with CMake or C/C++ in general and my current knowledge about CMake. 

If you're looking advance example of `CMakeLists.txt`, you could visit [Screenplay/CMakeLists.txt](https://gitlab.com/kelteseth/ScreenPlay/-/blob/master/CMakeLists.txt).

There interesting topic I not covered, such testing, cross-compilation and so forth. 

### Acknowledge

Thanks to [Elias Steurer](https://kelteseth.com/) for [additional correction](https://news.ycombinator.com/item?id=37713833).