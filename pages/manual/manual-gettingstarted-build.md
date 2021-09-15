---
title: Building Inviwo
tags: [getting_started, c++]
keywords: gettingstarted, c++
summary: "Learn to build Inviwo from source on your system"
sidebar: manual_sidebar
permalink: manual-gettingstarted-build.html
folder: manual
---
## How to build Inviwo

### Windows

#### Dependencies
You will need at least
- [CMake](https://cmake.org/download/) >= 3.12.0
    Also add the cmake binary to your PATH.

- [Qt5 binaries](https://qt.io/download-open-source/) >= 5.12
    Make sure you get the build for the 64 bit version for you Visual Studio version. Also add the Qt binary directory (something like `Qt/5.12.1/msvc2017_64/bin`) to your PATH.

- [Python](https://www.python.org/downloads/) (optional) is recommended in case you would like to do use Inviwo from Python, write Processors in Python, or perform batch operations. The easiest is to use the regular [Python distribution](https://www.python.org/downloads/).
{% include note.html content="
NumPy is required, `pip install numpy` or `conda install numpy` is sufficient.

**We strongly advice against using Anaconda** as Anaconda adds itself first to the PATH variable, which meanst that its Qt will be used instead of *your* Qt installed above. In case you would like to use conda, we instead recommend Miniconda as it does not include Qt. If you are forced to use Anaconda the following workarounds may make it work.
*Only if you are using Anaconda for your Python environment:*
   - Add an environment variable `CMAKE_PREFIX_PATH` and set it to your Qt dir, e.g., `Qt/5.12.1/msvc2017_64` (will ensure that CMake finds *your* Qt instead of Anaconda's).
   - Ensure that your Python environment is active before running CMake/Visual Studio. This can be done by starting the Anaconda Prompt, running `conda activate` and starting CMake/Visual Studio from the prompt.
" %}

#### Building
1. `git clone --recurse-submodules https://github.com/inviwo/inviwo`
The `--recurse-submodules` is necessary to pull dependencies.

2. Generate build pipeline (e.g. Visual Studio project): Open CMake (see [the CMake GUI tutorial](https://cmake.org/cmake/help/latest/guide/user-interaction/index.html#guide:User%20Interaction%20Guide) for more instructions on its usage), enter the source path and the preferred build directory (outside the inviwo directory!) and hit configure. You can then select the desired Inviwo modules (`IVW_MODULE_*`) and configure again. When selecting the compiler, make sure to select the correct Visual Studio version that you use on 64-bit. 32-bit is not supported.
3. (Optional) To add external Inviwo modules, add those in `IVW_EXTERNAL_MODULES` in the format of
`C:/Inviwo/otherrepo/modules;C:/mysite/myrepo/mymodules;`
 Use front slashes and no space between modules. Configure again. External modules are developed in the [inviwo modules repository](https://github.com/inviwo/modules).
4. Hit Generate and open the project in your IDE.

{% include note.html content="
When using a multi-configuration generator (like Visual Studio and most IDEs) you may want to adjust your build mode manually to `RelWithDeb`([Guide for Visual Studio](https://docs.microsoft.com/en-us/visualstudio/debugger/how-to-set-debug-and-release-configurations?view=vs-2019)), since it defaults to `Debug` at first, which has a large impact on the performance.
If you use a single-configuration generator, you can control the build mode using `CMAKE_BUILD_TYPE` in CMake.

 Unless you specifically need to debug the application, we recommend setting the build mode to `RelWithDebInfo` for good performance, while still getting reasonable stacktraces for debugging and error reporting.
" %}

#### Common Errors

##### Everything compiles but at runtime you get "failed to load QT symbols dll load errors"
Make sure that the same Qt version used for building is found when running the application. A common source of this error is that Anaconda is installed, which includes another Qt version and has added itself to the PATH environment variable. Make sure that the Qt version used for building is **before** the Anaconda path in the PATH. We have observed a similar problem with certain LaTeX distributions, so if the issue remains, try to move the LaTeX entry in your PATH behind your Qt version as well.

##### Everything compiles but at runtime you get "failed to load python.dll"
Add the path to the Python bin folder to your PATH environment variable.
You can find the path to the Python binary in Visual Studio by right clicking on the inviwo-module-python3 project and go to "Properties->Linker->Input->Additional dependencies".

##### Everything compiles but at runtime you get runtime error / Unhandled Exception in pybind11/embed.h
This may happen when the `PYTHONHOME` variable is not set or is incorrect. Check your system settings to see if it is correctly pointing to your python installation found by CMake. If you do not have the `PYTHONHOME` variable you should set it. It should point to the root folder of your python installation, e.g `C:/python37 or C:\Program Files (x86)\Microsoft Visual Studio\Shared\Anaconda3_64` (if you installed Anaconda with Visual Studio). To know which python installation inviwo uses you can check the output from the configuration pass in CMake, in the very beginning of the log it prints which python interpreter it found and will use.

### Linux

#### Dependencies
You will need at least
- [CMake](https://cmake.org/download/) >= 3.12.0
- [Qt binaries](https://qt.io/download-open-source/) >= 5.12
    Make sure you get the build for the 64 bit version of gcc or clang. Make sure to add the Qt folder to the `CMAKE_PREFIX_PATH` environment variable.
    **Example**: `export CMAKE_PREFIX_PATH=/home/user/Qt/5.13.0/gcc_x64/`
    **Note**: We highly recommend installing Qt with the official Qt installer instead of your package manager for Inviwo. While you can certainly get the versions from package managers to work, we experienced issues in the past with missing components and compiler incompatibilities.

For **Ubuntu** you can use the following commands:
```
sudo apt-get update
sudo apt-get install build-essential cmake cmake-qt-gui git freeglut3-dev xorg-dev
```
The first two commands add the Kitware APT Repo and the appropriate signing key, the third and fourth update your package manager and download the dependencies.

#### Python (optional)
On Linux the easiest way is to use the system python, which will usually be detected by CMake by default.
If you wish to use a different Python environment, e.g. from an Anaconda environment, you'll need to specify the `Python3_ROOT_DIR` in CMake (before first configure!) and set it to your conda environment.
Note that it may in some cases be necessary to run the compile or the binary from a command line with the conda environment activated.

#### Building
1. `git clone --recurse-submodules https://github.com/inviwo/inviwo`
The `--recurse-submodules` is necessary to pull dependencies.
2. Generate Build pipeline (e.g. Makefile, Ninja): Open CMake (see [the CMake GUI tutorial](https://cmake.org/cmake/help/latest/guide/user-interaction/index.html#guide:User%20Interaction%20Guide) for more instructions on its usage), enter the source path and the preferred build directory (outside the inviwo directory!) and hit configure. You can then select the desired Inviwo modules (`IVW_MODULE_*`) and configure again.
If CMake cannot find Qt, make sure you adjust your `CMAKE_PREFIX_PATH` as described above.
3. (Optional) To add external Inviwo modules, add those in `IVW_EXTERNAL_MODULES` in the format of
`C:/Inviwo/otherrepo/modules;C:/mysite/myrepo/mymodules;`
 Use front slashes and no space between modules. Configure again.
 External modules are developed in the [inviwo modules repository](https://github.com/inviwo/modules).
4. Hit Generate and open the project in your IDE.
{% include note.html content="
If you are using a single-configuration generator (like Make or Ninja) to build Inviwo, you can control the build mode with the `CMAKE_BUILD_TYPE` attribute in your CMake config.

When using a multi-configuration generator (like Visual Studio and most IDEs) you may want to adjust your build mode manually, since it probably defaults to `Debug` at first, which has a large impact on the performance.

Unless you specifically need to debug the application, we recommend setting the build mode to `RelWithDebInfo` for good performance, while still getting reasonable stacktraces for debugging and error reporting.
" %}


### Mac
TODO: do
{% include note.html content="
When using a multi-configuration generator (like Xcode) you may want to adjust your build mode manually, since it probably defaults to `Debug` at first, which has a large impact on the performance.

If you use a single-configuration generator, you can control the build mode using `CMAKE_BUILD_TYPE` in CMake.

 Unless you specifically need to debug the application, we recommend setting the build mode to `RelWithDebInfo` for good performance, while still getting reasonable stacktraces for debugging and error reporting.
" %}


### Recommended setup (Optional)

1. Build Type: Set the build mode to `RelWithDebInfo` ([See this guide for Visual Studio](https://docs.microsoft.com/en-us/visualstudio/debugger/how-to-set-debug-and-release-configurations?view=vs-2019))

2. Build with multiple cores:
    - Visual Studio: `Tools->Options->Projects and Solutions->Build and Run->maximum number of parallel project builds` We recommend 2-4 on an 8-core machine
    - Qt Creator: In `Projects->Build & Run->Build->Build Steps->Details->CMake arguments` add `-j <number of cores>`, e.g. `-j4`

3. Use multiple threads in Inviwo: In CMake set `IVW_MULTIPROCESSOR_COUNT` to the number of cores in your system.
