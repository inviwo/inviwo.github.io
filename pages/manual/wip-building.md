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



## Tools

### Git
You will need a git client to acquire the source code. We strongly recommend using a graphical client, Although a command line client  will also work.
* [Fork](https://fork.dev)
* [GitKraken](https://www.gitkraken.com/) 
* [git bash](https://gitforwindows.org/)

### CMake
You will need a recent version, we recommend using the latest version.
* [CMake](https://cmake.org/download/)

## Windows

### Compiler
We recommend that you compile Inviwo on windows using the latest version of [Visual Studio](https://visualstudio.microsoft.com/downloads/).

### Dependencies
- [Qt6 binaries](https://qt.io/download-open-source/)
  Make sure you get the build for the 64 bit version for you Visual Studio version.

- [Python](https://www.python.org/downloads/)
  Make sure you get a recent 64 bit version

- [Numpy](https://numpy.org/)
  ```bash
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
   ```bash
   git clone https://github.com/inviwo/inviwo
   ```
3. Clone the vcpkg repository. In the `base` directory run
    ```bash
    git clone https://github.com/microsoft/vcpkg
    ```
4. Configure CMake. From the `base` directory run:
   ```bash
   cmake -S inviwo --preset msvc-user
   ```
5. Compile Inviwo. From the `base` directory run:
   ```bash
   cmake --build builds/msvc-user --config RelWithDebInfo
   ```
6. Start Inviwo
   ```bash
   ./builds/msvc-user/bin/RelWithDebInfo/inviwo.exe
   ```

## Macos

### Compiler
We recommend that you compile Inviwo using the latest version of XCode from the Apple AppStore.

### Dependencies
- [Qt6 binaries](https://qt.io/download-open-source/)
- [Python](https://www.python.org/downloads/)
- [Numpy](https://numpy.org/)

 We recommend installing the dependencies using [brew](https://brew.sh)
```bash
brew install cmake qt python3 numpy
```

### Building
1. Create a base directory where you want to build Inviwo, e.g., `~/inviwo/`. We will call this directory `base`. At the end we will end up with a directory structure like this:
    ```
    └── base
        ├── builds
        │   └── xcode-user
        ├── inviwo
        └── vcpkg
    ```
2. Clone the Inviwo repository. In the `base` directory run:
   ```bash
   git clone https://github.com/inviwo/inviwo
   ```
3. Clone the vcpkg repository. In the `base` directory run
    ```bash
    git clone https://github.com/microsoft/vcpkg
    ```
4. Configure CMake. From the `base` directory run:
   ```bash
   cmake -S inviwo --preset xcode-user
   ```
5. Compile Inviwo. From the `base` directory run:
   ```bash
   cmake --build builds/xcode-user --config RelWithDebInfo
   ```
6. Start Inviwo
   ```bash
   open ./builds/xcode-user/bin/RelWithDebInfo/inviwo.app
   ```

## Linux

### Compiler
We recommend that you compile Inviwo using a recent version of Clang or GCC
We require C++23 support from the compiler.

### Dependencies
- [Qt6 binaries](https://qt.io/download-open-source/)
- [Python](https://www.python.org/downloads/)
- [Numpy](https://numpy.org/)

 We recommend installing the dependencies using you systems package manager
```bash
sudo apt-get update
sudo apt install \
     build-essential git ninja-build gcc-{{page.state.gcc}} g++-{{page.state.gcc}} \
     cmake python3 python3-pip python3-numpy \
     qt6-base-dev qt6-tools-dev qt6-tools-dev libqt6svg6-dev 
```

### Building
1. Create a base directory where you want to build Inviwo, e.g., `~/inviwo/`. We will call this directory `base`. At the end we will end up with a directory structure like this:
    ```
    └── base
        ├── builds
        │   └── xcode-user
        ├── inviwo
        └── vcpkg
    ```
2. Clone the Inviwo repository. In the `base` directory run:
   ```bash
   git clone https://github.com/inviwo/inviwo
   ```
3. Clone the vcpkg repository. In the `base` directory run
    ```bash
    git clone https://github.com/microsoft/vcpkg
    ```
4. Configure CMake. From the `base` directory run:
   ```bash
   cmake -S inviwo --preset ninja-user
   ```
5. Compile Inviwo. From the `base` directory run:
   ```bash
   cmake --build builds/ninja-user --config RelWithDebInfo
   ```
6. Start Inviwo
   ```bash
   ./builds/ninja-user/bin/RelWithDebInfo/inviwo
   ```

## Notes

### CMake Presets

Inviwo uses [**CMake Presets**](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html) to make setup and configuration easier. 
Out out the box inviwo provided the following presets:
* **msvc-user**: MSVC User configuration
* **msvc-developer**: MSVC Developer configuration
* **msvc-developer-modules**: MSVC Developer configuration with Modules
* **xcode-user**: Xcode User configuration
* **xcode-developer**: Xcode Developer configuration
* **xcode-developer-modules**: Xcode Developer configuration with Modules
* **ninja-user**: Ninja User configuration
* **ninja-developer**: Ninja Developer configuration
* **ninja-developer-modules**: Ninja Developer configuration with Modules

These are composed of a set of building blocks:
* **user**: This is the base configuration for using the Inviwo editor
* **developer**: This is the base configuration for developing new inviwo modules

* **vcpkg**: This sets the cmake toolchain file to the one provided by vcpkg. This assumes that the vcpkg repo is next to the inviwo repo. 
* **vcpkg-cache-read**: This enables reading of cached vcpkg binary artifacts to speed up building dependencies. See binary caching below for more detail. 
* **vcpkg-cache-write**: This also enables writing to the vcpkg cache. This requires setting the environment variable `VCPKG_CACHE_TOKEN` to a valid value.

* **modules**: This adds the [inviwo modules repo](https://github.com/inviwo/modules) to `IVW_EXTERNAL_MODULES` variable
* **modules-vcpkg**: This add extra vcpkg overlay ports needed for module dependencies.

* **build**: This sets the build directory to `builds/<preset name>`.

* **msvc**: This sets the cmake generator to Visual Studio, and defines the default vcpkg triplet.
* **xcode**: This sets the cmake generator to Xcode, and defines the default vcpkg triplet.
* **ninja**: This sets the cmake generator to Ninja, and defines the default vcpkg triplet.

You can easily compose your own preset in your `CMakeUserPresets.json` file, for example:
```json
{
  "version": 9,
  "cmakeMinimumRequired": { "major": 3, "minor": 30, "patch": 0 },
  "configurePresets": [
    {
      "name": "custom-msvc-dev",
      "displayName": "MSVC Custom configuration",
      "inherits" : ["msvc", "developer", "vcpkg", "vcpkg-cache-read", "build", "modules", "modules-vcpkg"],
      "environment": {
        "VCPKG_CACHE_TOKEN": "<my token>",
      },
      "cacheVariables": {
        "VCPKG_MANIFEST_NO_DEFAULT_FEATURES": { "type": "BOOL", "value": "ON" },
        "VCPKG_MANIFEST_FEATURES" :    "hdf5;ffmpeg;graphviz;sgct;ttk;vtk",
        "IVW_ENABLE_TRACY":            { "type": "BOOL", "value": "OFF" },
        "IVW_ENABLE_OPENMP":           { "type": "BOOL", "value": "ON" },
        "IVW_MODULE_TTK":              { "type": "BOOL", "value": "ON" },
        "IVW_MODULE_VTK":              { "type": "BOOL", "value": "ON" },
      }
    }
  ]
}
```

### External Modules
Inviwo supports adding additional `Inviwo Module`s. This is achieved by adding directories of modules to the cmake variable `IVW_EXTERNAL_MODULES`. 
Each subfolder in the given directory will then be added as an `Inviwo Module` to CMake which can be enabled by setting `IVW_MODULE_MYMODULE` to `ON`.
For example given a directory `C:/my_inviwo_modules` with a subfolder `mymodule` we can register it like this
```bash
cmake -S inviwo --preset msvc-user -DIVW_EXTERNAL_MODULES=C:/my_inviwo_modules -DIVW_MODULE_MYMODULE=ON
```

Additional paths can be added to `IVW_EXTERNAL_MODULES` by separating them using a semicolon `;`.

### The Modules Repo
The [inviwo modules repo](https://github.com/inviwo/modules) provides a large set of additional module the are groped it the following categories:
* vectorvis: Vector Visualization
* infovis: Information Visualization
* medvis: Medical Visualization
* misc: Miscellaneous
* molvis: Molecular Visualizations
* tensorvis: Tensor Field Visualization. 
* topovis: Modules for topological methods and topology visualization.

The can be enabled by cloning the repo in the `base` directory
```bash
git clone https://github.com/inviwo/modules
``` 
And then using one of the `*-modules` presets. Or by adding the them to the `IVW_EXTERNAL_MODULES` cmake variable.


### Python
Python enables you to use Inviwo from Python, write Processors in Python, or perform batch operations. The easiest is to use the regular [Python distribution](https://www.python.org/downloads/).
If you are sure you don't want python it can be disabled in cmake by turning off `IVW_ENABLE_PYTHON`

Inviwo will not access user site-package folders. Make sure to install the packages site-wide or add your user site-package folder to the environment variable `PYTHONPATH` for example `PYTHONPATH=%appdata%\\Python\\Python311\\site-packages\`


### Another Qt Installer (aqt)
An other very fast way to install Qt is using the aqtinstall python package. Install the python package:
```bash
pip install aqtinstall
```
Then install Qt:
```bash      
aqt.exe install-qt -O C:\Qt windows desktop {{page.state.qt}} win64_msvc2022_64 --modules debug_info --archives qtbase qtsvg
```
One can optionally also install the qt sources
```bash
aqt.exe install-src -O C:\Qt windows desktop {{page.state.qt}} --archives qtbase qtsvg
```


