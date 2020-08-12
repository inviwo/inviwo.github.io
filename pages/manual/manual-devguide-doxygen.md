---
title: Doxygen Guide
tags: [dev_guide, c++]
keywords: devguide, c++
summary: "How to use Doxygen docstrings in Inviwo"
sidebar: manual_sidebar
permalink: manual-devguide-doxygen.html
folder: manual
---
## Doxygen Guide

Please put API documentation into the headers and follow the guidelines described here. We use [Doxygen](https://doxygen.nl/) to automatically generate API documentation files for the project. Use docpage with markdown syntax for processors, see [below](#documenting-processors).

### Standard Documentation

#### Documenting classes:
```cpp
/**
 * \class CLASS_NAME
 *
 * \brief VERY_BRIEFLY_DESCRIBE_THE_CLASS
 *
 * DESCRIBE_THE_CLASS
 */
```

#### Documenting code blocks:
```cpp
/** \brief Brief description.
 *         Brief description continued.
 *
 *  Detailed description starts here.
 */
```

#### Documenting members:
```cpp
int var; ///< Brief description after the member
```
Document namespaces to make the namespaces and their functions appear in doxygen. Either document it once in code or add `docs/namespaces.dox` file in your module. Example:
```cpp
/**
 * \namespace inviwo::util util
 */
```

#### Example documentation for a class:
```cpp
class Test {
public:
/**
 * An enum.
 * More detailed enum description.
 */
    enum TEnum {
        TVal1, ///< enum value TVal1
        TVal2, ///< enum value TVal2
        TVal3  ///< enum value TVal3
    }
    enumVar;   ///< enum variable

    /**
     * A constructor.
     * A more elaborate description of the constructor.
     */
    Test();

    /**
     * A normal member taking two arguments and returning an integer value.
     * @param a an integer argument.
     * @param s a constant character pointer.
     * @see Test()
     * @return The test results
     */
    int testMe(int a, const char* s);
};
```

### Documenting Processors
Processor usage, inports, outports and properties, will be shown in a help widget if documentation is provided. The [processor generation tools](manual-devguide-meta.md) will add boiler plate documentation for your processor. Here is an example from the Background Processor.

```
/** \docpage{org.inviwo.Background, Background}
 * ![](org.inviwo.Background.png?classIdentifier=org.inviwo.Background)
 * Adds a background to an image.
 * The following mixing is applied
 *
 *     out.rgb = in.rgb + color.rgb * color.a * (1.0 - in.a)
 *     out.a = in.a + color.a * (1.0 - in.a)
 *
 * ### Inports
 *   * __ImageInport__ Input image.
 *
 * ### Outports
 *   * __ImageOutport__ Output image.
 *
 * ### Properties
 *   * __Style__ The are three different styles to choose from Linear gradient, uniform color,
 *     or checker board.
 *   * __Color1__ Used as the uniform color and as color 1 in the gradient and checkerboard.
 *   * __Color2__ Used as color 2 the gradient and checkerboard.
 *   * __Checker Board Size__ The size of the rectangles in the checker board.
 *   * __Switch colors__ Button to switch color 1 and 2.
 */

/**
 * \brief Adds a background to an image.
 *
 */
class IVW_MODULE_BASEGL_API Background : public Processor {
public:
    Background();
    virtual ~Background();

    virtual void initializeResources() override;
    virtual void process() override;

    virtual const ProcessorInfo getProcessorInfo() const override;
    static const ProcessorInfo processorInfo_;

private:
    void updateShaderInputs();

    enum class BlendMode {
        BackToFront,
        AlphaMixing
    };
    enum class BackgroundStyle {
        LinearHorizontal,
        LinearVertical,
        Uniform,
        CheckerBoard,
    };

    ImageInport inport_;
    ImageOutport outport_;

    TemplateOptionProperty<BackgroundStyle> backgroundStyle_;
    FloatVec4Property bgColor1_;
    FloatVec4Property bgColor2_;
    IntVec2Property checkerBoardSize_;
    ButtonProperty switchColors_;

    TemplateOptionProperty<BlendMode> blendMode_;

    Shader shader_;
    bool hadData_;
};
```

The docpage will be display inline inside of inviwo.

![Generated Processor Docs](images/manual/processorinfo.png)

The format of the docpage is very simple, the name has to match the class identifier for the processor,
one can make an optional second argument with a title if needed. The description text can use [standard Markdown syntax](https://www.doxygen.nl/manual/markdown.html). Images can be added to the data/help/images folder and then referred to in the description
or the a images folder inside a module

```
/** \docpage{<classIdentifier>}
 * Some text
 * ![caption (can be empty)](<image in data/help/images>)
 */
```

### Visual Studio integration

To make use of Visual Assist's document method feature, modify the format of its comment block by copying the below snippet into the VA Snippet for Refactor Document Method. There are separate VA Snippets for C++, C# and VB, ensure that you are under C++.

```cpp
/**
 * \brief VERY_BRIEFLY_DESCRIBE_THE_METHOD
 *
 * DESCRIBE_THE_METHOD
 *
 * @param $MethodArg$ DESCRIBE_ME
 * @return $SymbolType$ DESCRIBE_ME
 */
```

Using Visual Assist, you can add a snippet for fast commenting, trough VAssistX -> Tools -> Edit VA Snippets...

Example Snippet (just write "classdoc" somewhere in the file and hit enter to start the class commenting)

```cpp
/**
 * \class $Class_name$
 *
 * \brief VERY_BRIEFLY_DESCRIBE_THE_CLASS
 *
 * DESCRIBE_THE_CLASS
 */
```
