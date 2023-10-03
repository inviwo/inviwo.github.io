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

Inviwo uses CMake as build system and supports building on Windows with Visual Studio, MacOS with XCode, and Linux with Clang or GCC. We require the the compiler supports C++20. In most cases the latest version of AppleClang is the compiler with the least support so that dictates mostly what we require.
The source code is hosted on [GitHub](https://github.com/inviwo/inviwo), and we recommend using [git](https://git-scm.com/) to clone the repository, since you can then easily update to the latest version, and you also get the needed git sub-modules.

To acquire the dependencies, you can either use [vcpkg](https://github.com/microsoft/vcpkg), the bundled git sub-modules, or your system package manager. On Windows and MacOS we recommend vcpkg, on linux we recommend the system package manger primarily.  

#### Git
You will need a git client to acquire the source code. We strongly recommend using a graphical client such as [Fork](https://fork.dev) or [GitKraken](https://www.gitkraken.com/). Although a command line client such as [git bash](https://gitforwindows.org/) will also work.

#### CMake
You will need a recent version [CMake](https://cmake.org/download/), we recommend using the latest version.

### Windows

#### Compiler
We recommend that you compile Inviwo on windows using a recent version [Visual Studio](https://visualstudio.microsoft.com/downloads/) (2022 or later).

{% include note.html content="
Inviwo cannot be compiled with Visual Studio 2022 17.6.x due to a compiler [regression](https://developercommunity.visualstudio.com/t/Regression-176:-parameter-pack-expansio/10372131). Please upgrade to version 17.7.0 or later
" %}

#### Dependencies

- [Qt binaries](https://qt.io/download-open-source/) >= 6.
    Make sure you get the build for the 64 bit version for you Visual Studio version.
  
    An other very fast way to install Qt is using the aqtinstall python package. Install the python package:
  
      pip install aqtinstall

    Then install Qt:
      
      aqt.exe install-qt -O C:\Qt windows desktop 6.5.2 win64_msvc2019_64 --modules debug_info --archives qtbase qtsvg

    One can optionally also install the qt sources
  
      aqt.exe install-src -O C:\Qt windows desktop 6.6.0 --archives qtbase qtsvg


- [Python](https://www.python.org/downloads/) 
    is recommended in case you would like to use Inviwo from Python, write Processors in Python, or perform batch operations. The easiest is to use the regular [Python distribution](https://www.python.org/downloads/).
    One can disable python by turning off `IVW_ENABLE_PYTHON`

    {% include note.html content="NumPy is required, `pip install numpy` or `conda install numpy` is sufficient." %}

    {% include note.html content="Inviwo will not access user site-package folders. Make sure to install the packages site-wide or add your user site-package folder to the environment variable `PYTHONPATH` for example `PYTHONPATH=%appdata%\\Python\\Python311\\site-packages\`" %}

    {% include note.html content="**We strongly advice against using Anaconda** as Anaconda adds itself first to the PATH variable, which means that its Qt will be used instead of *your* Qt installed above. In case you would like to use conda, we instead recommend Miniconda as it does not include Qt. If you are forced to use Anaconda the following workarounds may make it work.
    *Only if you are using Anaconda for your Python environment:*
       + Add an environment variable `CMAKE_PREFIX_PATH` and set it to your Qt dir, e.g., `Qt/6.5.0/msvc2019_64` (will ensure that CMake finds *your* Qt instead of Anaconda's).
       + Ensure that your Python environment is active before running CMake/Visual Studio. This can be done by starting the Anaconda Prompt, running `conda activate` and starting CMake/Visual Studio from the prompt.
    " %}

- [Vcpkg](https://github.com/vcpkg)
    For all the other dependencies we recommend using vcpkg together with the manifest file in the Inviwo repository. This will ensure that you get the correct versions of the dependencies.
    And make it very easy to update the dependencies, and add new libraries as needed.

#### Building
1. Create a base directory where you want to build Inviwo, e.g., `C:/Inviwo/`. We will call this directory `base`. At the end we will end up with a directory structure like this:
    ```
    └── base
        ├── builds
        │   └── inviwo-vcpkg-dynamic
        ├── inviwo
        ├── modules
        └── vcpkg
    ```

2. Clone the Inviwo repository. In the `base` directory run:
    git clone --recurse-submodules https://github.com/inviwo/inviwo
The `--recurse-submodules` is necessary to pull dependencies.

3. Clone the vcpkg repository In the `base` directory run:
    git clone https://github.com/microsoft/vcpkg

4. Optional clone the Inviwo modules repository. In the `base` directory run:
    git clone --recurse-submodules https://github.com/inviwo/modules

5. Generate build system (e.g. Visual Studio project):
    Using the CMake GUI
    + Open CMake (see [the CMake GUI tutorial](https://cmake.org/cmake/help/latest/guide/user-interaction/index.html#guide:User%20Interaction%20Guide) for more instructions on its usage)
    + Enter the path to the Inviwo directory (e.g., `C:/Inviwo/inviwo`) in `Where is the source code`
    + Select a `Preset` either "MSVC User configuration" or "MSVC Developer configuration"
    + Press `Configure`
    + Optionally modify the configuration, You can then select the desired Inviwo modules (`IVW_MODULE_*`) and configure again. To add external Inviwo modules, add those in `IVW_EXTERNAL_MODULES` in the format of `C:/Inviwo/module/molvis;C:/mysite/myrepo/mymodules;`
    + Press `Generate`
    + Press `Open Project` to open the project in Visual Studio
    
    Or using the command line
    + From the `base` directory run:
        cmake -S inviwo --preset msvc-user
    + Any extra options can be past to CMake using `-D` for example to add external modules:
        cmake -S inviwo --preset msvc-user -DIVW_EXTERNAL_MODULES="C:/Inviwo/module/molvis;C:/mysite/myrepo/mymodules;"
      or extra modules
        cmake -S inviwo --preset msvc-user -DIVW_MODULE_SOMEMODULE=ON
    + Finally open the solution in MSVC by running:
        start builds/msvc-user/inviwo-projects.sln

{% include note.html content="
If you computer becomes unresponsive while building you can reduce the number of parallel build projects/cores:
    - Number of cores per project: In CMake set `IVW_MULTIPROCESSOR_COUNT` (the number of cl.exe processes per project).
    - Visual Studio: `Tools->Options->Projects and Solutions->Build and Run->maximum number of parallel project builds`
    - Qt Creator: In `Projects->Build & Run->Build->Build Steps->Details->CMake arguments` add `-j <number of cores>`, e.g. `-j4`
" %}

#### Common Errors

##### Everything compiles but at runtime you get "failed to load QT symbols dll load errors"
Make sure that the same Qt version used for building is found when running the application. A common source of this error is that Anaconda is installed, which includes another Qt version and has added itself to the PATH environment variable. Make sure that the Qt version used for building is **before** the Anaconda path in the PATH. We have observed a similar problem with certain LaTeX distributions, so if the issue remains, try to move the LaTeX entry in your PATH behind your Qt version as well.

##### Everything compiles but at runtime you get "failed to load python.dll"
Add the path to the Python bin folder to your PATH environment variable.
You can find the path to the Python binary in Visual Studio by right clicking on the inviwo-module-python3 project and go to "Properties->Linker->Input->Additional dependencies".

##### Everything compiles but at runtime you get runtime error / Unhandled Exception in pybind11/embed.h. For example "Unhandled exception at 0x00007FFE786A284E (ucrtbase.dll) in Inviwo.exe: Fatal program exit requested"
This may happen when the `PYTHONHOME` variable is not set or is incorrect. Check your system settings to see if it is correctly pointing to your python installation found by CMake. If you do not have the `PYTHONHOME` variable you should set it. It should point to the root folder of your python installation, e.g `C:/python37 or C:\Program Files (x86)\Microsoft Visual Studio\Shared\Anaconda3_64` (if you installed Anaconda with Visual Studio). To know which python installation inviwo uses you can check the output from the configuration pass in CMake, in the very beginning of the log it prints which python interpreter it found and will use.


### Mac

#### Compiler
We recommend that you compile Inviwo using the latest version of XCode.
We require C++20 support from the compiler.

#### Dependencies
You will need at least (we recommend using latest versions)
- [Qt binaries](https://qt.io/download-open-source/) >= 6.
    We recommend installing Qt using [brew](https://brew.sh) `brew install qt`

- [Python](https://www.python.org/downloads/) is recommended in case you would like to do use Inviwo from Python, write Processors in Python, or perform batch operations. See further (important!) instructions about Python for Mac below. Python can also easily be install using `brew install python` 
{% include note.html content="NumPy is required, `pip install numpy` is sufficient." %}

- [Vcpkg](https://github.com/vcpkg)
    For all the other dependencies we recommend using vcpkg together with the manifest file in the Inviwo repository. This will ensure that you get the correct versions of the dependencies.
    And make it very easy to update the dependencies, and add new libraries as needed.

#### Python (optional)
Ensuring that the correct Python is found can be a bit complicated (read more about [brew Python here](https://docs.brew.sh/Homebrew-and-Python)). If you do not care about using different Python versions it is probably easiest to simply install Python 3 via brew:
```
brew install python3
pip3 install numpy
```

If you wish to use a different Python environment, e.g. from an Miniconda environment, you'll need to activate the conda environment from the Terminal and run cmake/XCode from there:
```
brew install --cask miniconda
conda init "$(basename "${SHELL}")"
conda install numpy
```
These three lines install miniconda, sets up the Terminal to use conda, and installs numpy to the active conda environment. **We strongly advice against using Anaconda** as Anaconda adds itself first to the PATH variable, which means that its Qt will be used instead of *your* Qt installed above. In case you would like to use conda, we instead recommend Miniconda as it does not include Qt. 
Note that CMake may not be able to find the appropriate Python environment unless it has been started from a command line with the conda environment activated:
```
conda activate
cmake-gui
```

#### Building
1. Create a base directory where you want to build Inviwo, e.g., `~/Inviwo/`. We will call this directory `base`. At the end we will end up with a directory structure like this:
    ```
    └── base
        ├── builds
        │   └── inviwo-vcpkg-dynamic
        ├── inviwo
        ├── modules
        └── vcpkg
    ```

2. Clone the Inviwo repository. In the `base` directory run:
    git clone --recurse-submodules https://github.com/inviwo/inviwo
The `--recurse-submodules` is necessary to pull dependencies.

3. Clone the vcpkg repository In the `base` directory run:
    git clone https://github.com/microsoft/vcpkg

4. Optional clone the Inviwo modules repository. In the `base` directory run:
    git clone --recurse-submodules https://github.com/inviwo/modules

5. Generate build system (e.g. XCode project):
    Using the CMake GUI
    + Open CMake (see [the CMake GUI tutorial](https://cmake.org/cmake/help/latest/guide/user-interaction/index.html#guide:User%20Interaction%20Guide) for more instructions on its usage)
    + Enter the path to the Inviwo directory (e.g., `~/Inviwo/inviwo`) in `Where is the source code`
    + Select a `Preset` either "XCode User configuration" or "XCode Developer configuration"
    + Press `Configure`
    + Optionally modify the configuration, You can then select the desired Inviwo modules (`IVW_MODULE_*`) and configure again. To add external Inviwo modules, add those in `IVW_EXTERNAL_MODULES` in the format of `~/Inviwo/module/molvis;C:/mysite/myrepo/mymodules;`
    + Press `Generate`
    + Press `Open Project` to open the project in XCode
    
    Or using the command line
    + From the `base` directory run:
        cmake -S inviwo --preset xcode-user
    + Any extra options can be past to CMake using `-D` for example to add external modules:
        cmake -S inviwo --preset xcode-user -DIVW_EXTERNAL_MODULES="~/Inviwo/module/molvis;~/mysite/myrepo/mymodules;"
      or extra modules
        cmake -S inviwo --preset xcode-user -DIVW_MODULE_SOMEMODULE=ON
    + Finally open the solution in MSVC by running:
        open builds/xcode-user/inviwo-projects.xcodeproj


### Linux

#### Compiler
We recommend that you compile Inviwo using a recent version of Clang or GCC
We require C++20 support from the compiler.

#### Dependencies
- [Qt binaries](https://qt.io/download-open-source/) >= 6.
    Make sure you get the build for the 64 bit version of gcc or clang. Make sure to add the Qt folder to the `CMAKE_PREFIX_PATH` environment variable.
    **Example**: `export CMAKE_PREFIX_PATH=/home/user/Qt/6.5.0/gcc_x64/`

For **Ubuntu** you can use the following commands:

```
sudo apt-get update
sudo apt install \
     build-essential git ninja-build gcc-12 g++-12 \
     cmake extra-cmake-modules cmake-qt-gui \
     python3 python3-pip python3-numpy python3-h5py python3-pybind11 python3-scipy python3-regex pybind11-json-dev \
     qt6-base-dev qt6-tools-dev qt6-tools-dev libqt6svg6-dev \
     libglew-dev freeglut3-dev xorg-dev \
     openexr zlib1g zlib1g-dev libjpeg-dev libtiff-dev libtirpc-dev \
     libhdf5-dev libpng-dev libglu1-mesa-dev libxrandr-dev libxinerama-dev libxcursor-dev \
     googletest libgtest-dev libglm-dev nlohmann-json3-dev libfmt-dev libglew-dev \
     libfreetype-dev libassimp-dev cimg-dev libnifti-dev libznz-dev libopenexr-dev libtclap-dev
```

#### Python (optional)
On Linux the easiest way is to use the system python, which will usually be detected by CMake by default.
If you wish to use a different Python environment, e.g. from an Anaconda environment, you'll need to specify the `Python3_ROOT_DIR` in CMake (before first configure!) and set it to your conda environment.
Note that it may in some cases be necessary to run the compile or the binary from a command line with the conda environment activated.

#### Building
1. Create a base directory where you want to build Inviwo, e.g., `~/Inviwo/`. We will call this directory `base`. At the end we will end up with a directory structure like this:
    ```
    └── base
        ├── builds
        │   └── inviwo-vcpkg-dynamic
        ├── inviwo
        ├── modules
        └── vcpkg
    ```

2. Clone the Inviwo repository. In the `base` directory run:
    git clone --recurse-submodules https://github.com/inviwo/inviwo
The `--recurse-submodules` is necessary to pull dependencies.

3. Clone the vcpkg repository In the `base` directory run:
    git clone https://github.com/microsoft/vcpkg

4. Optional clone the Inviwo modules repository. In the `base` directory run:
    git clone --recurse-submodules https://github.com/inviwo/modules

5. Generate build system (e.g. Ninja project):
    Using the CMake GUI
    + Open CMake (see [the CMake GUI tutorial](https://cmake.org/cmake/help/latest/guide/user-interaction/index.html#guide:User%20Interaction%20Guide) for more instructions on its usage)
    + Enter the path to the Inviwo directory (e.g., `~/Inviwo/inviwo`) in `Where is the source code`
    + Select a `Preset` either "Ninja User configuration using apt" or "Ninja Developer configuration using apt"
    + Press `Configure`
    + Optionally modify the configuration, You can then select the desired Inviwo modules (`IVW_MODULE_*`) and configure again. To add external Inviwo modules, add those in `IVW_EXTERNAL_MODULES` in the format of `~/Inviwo/module/molvis;C:/mysite/myrepo/mymodules;`
    + Press `Generate`
    + Compile the project by running `ninja` in the build folder `~/Inviwo/builds/xcode-user`
    
    Or using the command line
    + From the `base` directory run:
        cmake -S inviwo --preset ninja-user-apt
    + Any extra options can be past to CMake using `-D` for example to add external modules:
        cmake -S inviwo --preset ninja-user-apt -DIVW_EXTERNAL_MODULES="~/Inviwo/module/molvis;~/mysite/myrepo/mymodules;"
      or extra modules
        cmake -S inviwo --preset ninja-user-apt -DIVW_MODULE_SOMEMODULE=ON
    + Finally compile inviwo by running `ninja` in the build folder `~/Inviwo/builds/xcode-user`

### Comments

{% include tip.html content="
Unless you specifically need to debug the application, we recommend setting the build mode to `RelWithDebInfo` for good performance, while still getting reasonable stacktraces for debugging and error reporting.

When using a multi-configuration generator (like Visual Studio, XCode, and most IDEs) you may want to adjust your build mode manually to `RelWithDebInfo`([Guide for Visual Studio](https://docs.microsoft.com/en-us/visualstudio/debugger/how-to-set-debug-and-release-configurations?view=vs-2019)), since it defaults to `Debug` at first, which has a large impact on the performance.
If you use a single-configuration generator, you can control the build mode using `CMAKE_BUILD_TYPE` in CMake.
" %}
