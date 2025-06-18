---
title: Building Inviwo
tags: [getting_started, c++]
keywords: gettingstarted, c++
summary: "Build Inviwo from source on your system"
sidebar: manual_sidebar
permalink: manual-gettingstarted-build.html
folder: manual

state:
    qt: 6.9.1
    gcc: 14
---

{:note:     .alert .alert-info    title="note"}
{:tip:      .alert .alert-success title="tip"}
{:warning:  .alert .alert-danger  title="warning"}



## Tools

### Git
You will need a git client to acquire the source code, and while we strongly recommend using a graphical client, a command line client will work as well.
* [Fork](https://fork.dev)
* [GitKraken](https://www.gitkraken.com/) 
* [git bash](https://gitforwindows.org/)

### CMake
You will need a recent version, we recommend using the latest version.
* [CMake](https://cmake.org/download/)

## Windows

### Compiler
We recommend that you compile Inviwo on Windows using the latest version of [Visual Studio](https://visualstudio.microsoft.com/downloads/).

### Dependencies
- [Qt6 binaries](https://qt.io/download-open-source/)
  Make sure you get the build for the 64 bit version for your Visual Studio version.

- [Python](https://www.python.org/downloads/)
  Make sure you get a recent 64 bit version

- [Numpy](https://numpy.org/)
  ```shell
  pip install numpy
  ```

### Building
1. Create a base directory where you want to build Inviwo, e.g., `C:/inviwo-project/`. At the end you should end up with a directory structure like this:
    ```
    └── inviwo-project
        ├── builds
        │   └── msvc-user
        ├── inviwo
        └── vcpkg
    ```
2. Clone the Inviwo repository. In the `inviwo-project` directory run:
   ```powershell
   git clone https://github.com/inviwo/inviwo
   ```
3. Clone the vcpkg repository. In the `inviwo-project` directory run
    ```powershell
    git clone https://github.com/microsoft/vcpkg
    ```
4. Configure CMake. From the `inviwo-project` directory run:
   ```powershell
   cmake -S inviwo --preset msvc-user
   ```
5. Compile Inviwo. From the `inviwo-project` directory run:
   ```powershell
   cmake --build builds/msvc-user --config RelWithDebInfo
   ```
6. Start Inviwo
   ```powershell
   ./builds/msvc-user/bin/RelWithDebInfo/inviwo.exe
   ```

When you gotten this far you might want to customize your build with a different [cmake preset](#cmake-presets) or by adding more inviwo modules from the [inviwo modules repo](#the-modules-repo) or by creating your own [module](manual-devguide-meta.html) and [processor](manual-devguide-build-processor.html).

## Macos

### Compiler
We recommend that you compile Inviwo using the latest version of XCode from the Apple AppStore.

### Dependencies
 We recommend installing the dependencies using [brew](https://brew.sh).

- [Qt6 binaries](https://qt.io/download-open-source/)
- [Python](https://www.python.org/downloads/)
- [Numpy](https://numpy.org/)
```shell
brew install cmake qt python3 numpy
```

### Building
1. Create a base directory where you want to build Inviwo, e.g., `~/inviwo-project/`. At the end we will end up with a directory structure like this:
    ```
    └── inviwo-project
        ├── builds
        │   └── xcode-user
        ├── inviwo
        └── vcpkg
    ```
2. Clone the Inviwo repository. In the `inviwo-project` directory run:
   ```shell
   git clone https://github.com/inviwo/inviwo
   ```
3. Clone the vcpkg repository. In the `inviwo-project` directory run
    ```shell
    git clone https://github.com/microsoft/vcpkg
    ```
4. Configure CMake. From the `inviwo-project` directory run:
   ```shell
   cmake -S inviwo --preset xcode-user
   ```
5. Compile Inviwo. From the `inviwo-project` directory run:
   ```shell
   cmake --build builds/xcode-user --config RelWithDebInfo
   ```
6. Start Inviwo
   ```shell
   open ./builds/xcode-user/bin/RelWithDebInfo/inviwo.app
   ```
When you gotten this far you might want to customize your build with a different [cmake preset](#cmake-presets) or by adding more inviwo modules from the [inviwo modules repo](#the-modules-repo) or by creating your own [module](manual-devguide-meta.html) and [processor](manual-devguide-build-processor.html).

## Linux

### Compiler
We recommend that you compile Inviwo using a recent version of Clang or GCC.
We require C++23 support from the compiler.

### Dependencies
- [Qt6 binaries](https://qt.io/download-open-source/)
- [Python](https://www.python.org/downloads/)
- [Numpy](https://numpy.org/)

 We recommend installing the dependencies using your systems package manager.
```shell
sudo apt-get update
sudo apt install \
     build-essential git ninja-build curl gcc-{{page.state.gcc}} g++-{{page.state.gcc}} \
     cmake python3 python3-pip python3-numpy \
     qt6-base-dev qt6-tools-dev qt6-tools-dev libqt6svg6-dev \
     bison flex libxt-doc
```

### Building
1. Create a base directory where you want to build Inviwo, e.g., `~/inviwo-project/`. At the end we will end up with a directory structure like this:
    ```
    └── inviwo-project
        ├── builds
        │   └── ninja-user
        ├── inviwo
        └── vcpkg
    ```
2. Clone the Inviwo repository. In the `inviwo-project` directory run:
   ```shell
   git clone https://github.com/inviwo/inviwo
   ```
3. Clone the vcpkg repository. In the `inviwo-project` directory run
    ```shell
    git clone https://github.com/microsoft/vcpkg
    ```
4. Configure CMake. From the `inviwo-project` directory run:
   ```shell
   cmake -S inviwo --preset ninja-user -DVCPKG_TARGET_TRIPLET=x64-linux-dynamic -DVCPKG_HOST_TRIPLET=x64-linux-dynamic
   ```
5. Compile Inviwo. From the `inviwo-project` directory run:
   ```shell
   cmake --build builds/ninja-user
   ```
6. Start Inviwo
   ```shell
   ./builds/ninja-user/bin/inviwo
   ```
   
When you gotten this far you might want to customize your build with a different [cmake preset](#cmake-presets) or by adding more inviwo modules from the [inviwo modules repo](#the-modules-repo) or by creating your own [module](manual-devguide-meta.html) and [processor](manual-devguide-build-processor.html).

## Notes

### CMake Presets

Inviwo uses [**CMake Presets**](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html) to make setup and configuration easier. 
Out of the box, Inviwo provides the following presets:
* **msvc-user**: MSVC User configuration
* **msvc-developer**: MSVC Developer configuration
* **msvc-developer-modules**: MSVC Developer configuration with Modules
* **xcode-user**: Xcode User configuration
* **xcode-developer**: Xcode Developer configuration
* **xcode-developer-modules**: Xcode Developer configuration with Modules
* **ninja-user**: Ninja User configuration
* **ninja-developer**: Ninja Developer configuration
* **ninja-developer-modules**: Ninja Developer configuration with Modules

#### Preset Building Blocks.
The Presets above are composed from a set of building blocks

##### Base Config
* **user**: This is the base configuration for using the Inviwo editor. This will only build the inviwo application and is best when you only want to run inviwo. 
* **developer**: This is the base configuration for developing inviwo. This will build inviwo and all tests etc. It will also enable various asserts and profiling tools to make developing easier. 

##### Vcpkg Config
* **vcpkg**: This sets the cmake toolchain file to the one provided by vcpkg. This assumes that the vcpkg repo is next to the inviwo repo. 
* **vcpkg-cache-read**: This enables reading of cached vcpkg binary artifacts to speed up building dependencies. See binary caching below for more detail. 
* **vcpkg-cache-write**: This also enables writing to the vcpkg cache. This requires setting the environment variable `VCPKG_CACHE_TOKEN` to a valid value.

##### Modules Config
* **modules**: This adds the [inviwo modules repo](https://github.com/inviwo/modules) to `IVW_EXTERNAL_MODULES` variable
* **modules-vcpkg**: This adds additional vcpkg overlay ports needed for module dependencies.

##### Generator Config
* **msvc**: This sets the cmake generator to Visual Studio, and defines the vcpkg triplet `x64-windows`.  
* **xcode**: This sets the cmake generator to Xcode, and defines the vcpkg triplet `arm64-osx-dynamic`.
* **ninja**: This sets the cmake generator to Ninja, and you need to specify the vcpkg triplet manually. For example on linux you might pass `-DVCPKG_TARGET_TRIPLET=x64-linux-dynamic -DVCPKG_HOST_TRIPLET=x64-linux-dynamic` on the command line.

##### Build Config
* **build**: This sets the build directory to `builds/<preset name>`.

#### Custom Presets
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
        "VCPKG_CACHE_TOKEN": "<my token>"
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
The cmake variable `VCPKG_MANIFEST_FEATURES` can be used to specify additional vcpkg dependencies to install not needed by the default modules. HDF5 for data storage, FFmpeg for multimedia processing, Graphviz for graph visualization, SGCT for multi-display setups, TTK for topology toolkit, and VTK for the visualization toolkit.
The `hdf5` and `ffmpeg` features are enabled by default and used by the `IVW_MODULE_HDF5` and `IVW_MODULE_FFMPEG` modules, but can be disabled by setting `VCPKG_MANIFEST_NO_DEFAULT_FEATURES` to `ON`. The features are defined in `inviwo-project/ìnviwo/vcpkg.json`.

### Vcpkg Binary Caching
Build all the dependencies using vcpkg can be time consuming, hence we provide a binary caching server (`https://jenkins.inviwo.org`) to speed up the process. To enable the cache simply inherit your cmake preset from `vcpkg-cache-read`. This is already done for the default presets. This will configure the `VCPKG_BINARY_SOURCES` appropriately. To also write to the cache one can use `vcpkg-cache-write` but that will require you to ask the inviwo maintainers for a token to be provided as an environment variable `VCPKG_CACHE_TOKEN`.

Vcpkg will automatically hash the abi version of the dependencies to avoid any incompatibilities. This means it is easy to get cache misses if there are slight differences in the setups. We use `VCPKG_INSTALL_OPTIONS=--x-abi-tools-use-exact-versions` and on windows we also set `VCPKG_FEATURE_FLAGS=-compilertracking` to reduce the chance of cache misses. 
It is also important to have the same version of the vcpkg tool itself. To ensure that, one should make sure that the `vcpkg` repo is checked out to the same commit as is defined as the baseline in the `vcpkg.json` file. Then bootstrap vcpkg to acquire the corresponding vcpkg executable. This can be done as follows, From the `inviwo-project/vcpkg` directory run:

- On Windows (using powershell):
  ```powershell
  git reset --hard ((get-content ..\inviwo\vcpkg.json | ConvertFrom-Json).'vcpkg-configuration'.'default-registry'.'baseline')
  ./bootstrap-vcpkg.bat
  ```
- On macOS/Linux (requires that you have jq installed):
  ```bash
  git reset --hard `jq -r ".[\"vcpkg-configuration\"].[\"default-registry\"].baseline" ../inviwo/vcpkg.json`
  ./bootstrap-vcpkg.sh
  ```

### External Modules
Inviwo supports adding additional `Inviwo Modules`. An `Inviwo Module` is a self-contained package of functionality, such as processors, data formats, or utilities, that extends the capabilities of the Inviwo framework. For more details, see the [Inviwo Modules Documentation](https://inviwo.org/documentation/modules/). 

This is achieved by adding directories of modules to the cmake variable `IVW_EXTERNAL_MODULES`. Each subfolder in the given directory will then be added as an `Inviwo Module` to CMake which can be enabled by setting `IVW_MODULE_MYMODULE` to `ON`.
For example given a directory `C:/inviwo-project/my_inviwo_modules` with a subfolder `mymodule` we can register it like this
```shell
cmake -S inviwo --preset msvc-user -DIVW_EXTERNAL_MODULES=C:/my_inviwo_modules -DIVW_MODULE_MYMODULE=ON
```

Additional paths can be added to `IVW_EXTERNAL_MODULES` by separating them using a semicolon `;`.

### The Modules Repo
The [inviwo modules repo](https://github.com/inviwo/modules) provides a large set of additional module the are grouped into the following categories:
 * vectorvis: Modules for visualizing vector fields and related data.
 * infovis: Modules for creating and exploring information visualizations.
 * medvis: Modules focused on medical data visualization and analysis.
 * misc: Miscellaneous modules that do not fit into other categories.
 * molvis: Modules for visualizing molecular structures and simulations.
 * tensorvis: Modules for visualizing tensor fields and their properties.
 * topovis: Modules for applying and visualizing topological methods.

The modules can be enabled by first cloning the modules repo in the `inviwo-project` directory
```shell
git clone https://github.com/inviwo/modules
``` 
And then using one of the `*-modules` presets. Or by adding the them to the `IVW_EXTERNAL_MODULES` cmake variable.

### Python
Python enables you to use Inviwo from Python, write Processors in Python, or perform batch operations. The easiest way is to use the regular [Python distribution](https://www.python.org/downloads/).
If you are sure you don't want Python it can be disabled in cmake by turning off `IVW_ENABLE_PYTHON`

Inviwo will not access user site-package folders. Make sure to install the packages site-wide or add your user site-package folder to the environment variable `PYTHONPATH`. 

For example:
- On Windows: `PYTHONPATH=%appdata%\\Python\\Python311\\site-packages\`
- On macOS: `PYTHONPATH=~/Library/Python/3.x/lib/python/site-packages`
- On Linux: `PYTHONPATH=~/.local/lib/python3.11/site-packages`

Inviwo will also look at the `VIRTUAL_ENV` when starting the python interpreter, and use that if it is set.


### Another Qt Installer (aqt)
Another very fast way to install Qt is using the aqtinstall python package. Install the python package:
```shell
pip install aqtinstall
```
Then install Qt:
```shell      
aqt.exe install-qt -O C:\Qt windows desktop {{page.state.qt}} win64_msvc2022_64 --modules debug_info --archives qtbase qtsvg
```
One can optionally also install the qt sources.
```shell
aqt.exe install-src -O C:\Qt windows desktop {{page.state.qt}} --archives qtbase qtsvg
```

To help cmake find Qt it can be helpful to set the `CMAKE_PREFIX_PATH` to the Qt root directory. A preset like the following can be helpful:
```json
    {
      "name": "qt",
      "hidden": true,
      "cacheVariables": {
        "CMAKE_PREFIX_PATH" :   { "type": "PATH", "value": "C:/Qt/{{page.state.qt}}/msvc2022_64"}
      }
    }
```

### Updating Master
To update Inviwo to the latest version, use your graphical git client to pull the latest master of the Inviwo repository and, optionally, the Inviwo modules repository. Alternatively, run the following commands in the `inviwo-project` directory:
```bash
cd inviwo
git pull
cd ../modules
git pull
cd ../vcpkg
git pull
```
Make sure to set vcpkg to the correct baseline, see [Vcpkg Binary Caching](#vcpkg-binary-caching). Then rerun CMake and build.