---
title: Create your own property
tags: [dev_guide, c++]
keywords: devguide, c++
summary: "Learn to create your own C++ properties for use in Inviwo"
sidebar: manual_sidebar
permalink: manual-devguide-build-property.html
folder: manual
---
# Build your own Property
This guide will walk you through the creation of a new property.

## Necessary files and changes
Similar to the ["Build your own Processor" guide](manual-devguide-build-processor), we recommend using Inviwo-meta to create the necessary files for a property. The following describes the necessary files and changes:
- Create source and header files in `<module-dir>/src/properties/`
- Register the property in its module. Open the `<module-dir>/src/<module-name>module.cpp` and add the following line to its constructor: `registerProperty<MyNewProperty>()`
- Add the new property's header and source to the modules `CMakeLists.txt` (`<module-dir>/CMakeLists.txt`)

Inviwo-meta does currently not have a special argument to create properties. That means you need to use the standard file creation default arguments. This will create the header and source and will add them to CMake. You will need to register the property in the module manually.

<!-- ## TODO: Composite Property

## TODO: Serialization -->
