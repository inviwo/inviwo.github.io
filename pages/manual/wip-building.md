---
title: Building Inviwo
tags: [getting_started, c++]
keywords: gettingstarted, c++
summary: "Build Inviwo from source on your system"
sidebar: manual_sidebar
permalink: wip-building.html
folder: manual

state:
    qt: 6.9.0
    gcc: 14
---

{:note:     .alert .alert-info    title="note"}
{:tip:      .alert .alert-success title="tip"}
{:warning:  .alert .alert-danger  title="warning"}



# Tools

## Git
You will need a git client to acquire the source code. We strongly recommend using a graphical client, Although a command line client  will also work.
* [Fork](https://fork.dev)
* [GitKraken](https://www.gitkraken.com/) 
* [git bash](https://gitforwindows.org/)

## CMake
You will need a recent version, we recommend using the latest version.
* [CMake](https://cmake.org/download/)

# Windows

## Compiler
We recommend that you compile Inviwo on windows using the latest version of [Visual Studio](https://visualstudio.microsoft.com/downloads/).

## Dependencies
- [Qt6 binaries](https://qt.io/download-open-source/)
  Make sure you get the build for the 64 bit version for you Visual Studio version.

- [Python](https://www.python.org/downloads/)
  Make sure you get a recent 64 bit version

- [Numpy](https://numpy.org/)
  ```sh
  pip install numpy
  ```

### Building
1. Create a base directory where you want to build Inviwo, e.g., `C:/Inviwo/`. We will call this directory `base`. At the end we will end up with a directory structure like this:
    ```
    └── base
        ├── builds
        │   └── msvc-user
        ├── inviwo
        └── vcpkg
    ```
2. Clone the Inviwo repository. In the `base` directory run:
   ```psh
   git clone https://github.com/inviwo/inviwo
   ```
3. Clone the vcpkg repository. In the `base` directory run
    ```psh
    git clone https://github.com/microsoft/vcpkg
    ```
4. Configure CMake. From the `base` directory run:
   ```psh
   cmake -S inviwo --preset msvc-user
   ```
5. Compile Inviwo. From the `base` directory run:
   ```psh
   cmake --build builds/msvc-user --config RelWithDebInfo
   ```
6. Start Inviwo
   ```psh
   ./builds/msvc-user/bin/RelWithDebInfo/inviwo.exe
   ```


# Mac

# Linux


# Notes

## External Modules

### The Modules Repo


## Python
Inviwo will not access user site-package folders. Make sure to install the packages site-wide or add your user site-package folder to the environment variable `PYTHONPATH` for example `PYTHONPATH=%appdata%\\Python\\Python311\\site-packages\`


## Another Qt Installer (aqt)
An other very fast way to install Qt is using the aqtinstall python package. Install the python package:
```sh
pip install aqtinstall
```
Then install Qt:
```sh      
aqt.exe install-qt -O C:\Qt windows desktop {{page.state.qt}} win64_msvc2022_64 --modules debug_info --archives qtbase qtsvg
```
One can optionally also install the qt sources
```sh
aqt.exe install-src -O C:\Qt windows desktop {{page.state.qt}} --archives qtbase qtsvg
```

> **Note:** This is a note callout in Jekyll.

> **Note:** You can style this block using custom CSS.
{:.note}


> This is a note using Just the Docs styling.
{: note }


> <i class="fa fa-info-circle"></i> <b>Note:</b> This is a note using Just the Docs styling.
{: tip }


