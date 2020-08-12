---
title: Frequently Asked Questions
tags: [dev_guide]
keywords: devguide
summary: "Frequently asked questions"
sidebar: manual_sidebar
permalink: manual-devguide-faq.html
folder: manual
---

## Frequently Asked Questions


This page is intended to provide some more detailed information on different aspects when developing in Inviwo. Typical examples include dealing with images and meshes and how to create and access them in a proper fashion.

* [Images](#images)
* [Meshes](#meshes)
* [Metadata](#metadata)

### Images

#### Convert raw data into an `inviwo::Image`

This can be used for all supported data types, here for float vec4:
```cpp
auto layer = std::make_shared<LayerRAMPrecision<vec4>>(imgDim, LayerType::Color);
auto data = layer->getDataTyped(); // data will be a vec4*
...
// write data
...
auto outImage =  std::make_shared<Image>(std::make_shared<Layer>(layer));
outport_.setData(outImage);
```

#### Convert QImage to an `inviwo::Image`

```cpp
QImage img(":/icons/inviwo_dark.png");

size2_t dim(img.width(), img.height());
// need to use QRgb since pixel format depends on underlying platform
auto imgData = reinterpret_cast<const QRgb *>(img.bits());
auto data = new glm::u8vec4[dim.x * dim.y];
// copy pixel values from QImage
for (std::size_t i=0; i < dim.x * dim.y; ++i) {
    auto pixel = imgData[i];
    data[i] ={ qRed(pixel), qGreen(pixel), qBlue(pixel), qAlpha(pixel) };
}
// create layer representation using the image data
auto layerRAM = std::make_shared<LayerRAMPrecision<glm::u8vec4>>(data, dim, LayerType::Color);
// Create a layer with the layer representation, and an image with the layer.
auto ivwImage = std::make_shared<Image>(std::make_shared<Layer>(layerRAM));
```


### Meshes

Meshes used in Inviwo can be created and manipulated in different ways.
1. Using `inviwo::Mesh` directly
    **Advantages:** full flexibility, e.g. custom datatypes like `vec2` instead of `vec4`, and no additional buffers like texture coordinates.
2. Using `inviwo::BasicMesh`
    **Advantages:** predefined buffers for position (`vec3`), normal (`vec3`), texture coordinate (`vec3`), and color (`vec4`). Provides some primitives (ellipse, cylinder, sphere, arrow, ...).
3. Using `inviwo::SimpleMesh`
    **Advantages:** predefined buffers for position (`vec3`), texture coordinate (`vec3`), and color (`vec4`). Basic primitivs in `SimpleMeshCreator`, i.e. rectangle, plane, sphere, ...

Mesh manipulation is possible via member functions of `Mesh`, `SimpleMesh`, or `BasicMesh`, e.g. `append()`, `addBuffer()`, `addVertex()`, `setVertex()`, etc. In addition, the RAM representations of the individual buffers can accessed as well.

**Note:** appending one mesh to another will merge all buffers, i.e. the number and types of buffers must match. In addition, the new mesh will only have a single transformation matrix.

#### Using `inviwo::Mesh`
```cpp
std::shared_ptr<Mesh> generateAxisMesh3D(const AxisProperty& property, const vec3& startPos,
                                         const vec3& endPos) {
    auto mesh = std::make_shared<Mesh>(DrawType::Lines, ConnectivityType::None);

    auto posBuffer = std::make_shared<Buffer<vec3>>(2u, BufferUsage::Static);
    auto colBuffer = std::make_shared<Buffer<vec4>>(2u, BufferUsage::Static);
    auto& vertices = posBuffer->getEditableRAMRepresentation()->getDataContainer();
    auto& colors = colBuffer->getEditableRAMRepresentation()->getDataContainer();

    std::fill(colors.begin(), colors.end(), property.color_.get());

    vertices[0] = startPos;
    vertices[1] = endPos;

    mesh->addBuffer(BufferType::PositionAttrib, posBuffer);
    mesh->addBuffer(BufferType::ColorAttrib, colBuffer);

    mesh->addIndicies(Mesh::MeshInfo(DrawType::Lines, ConnectivityType::None),
                      inviwo::util::makeIndexBuffer({0, 1}));

    return mesh;
}
```

#### Using `inviwo::BasicMesh`
```cpp
std::shared_ptr<Mesh> getMaximaMesh(const std::vector<ivec3>& positions,
                                    const size3_t& volDim) {
    if (positions.empty()) {
        return std::make_shared<BasicMesh>();
    }
    std::shared_ptr<BasicMesh> mesh = std::make_shared<BasicMesh>();

    std::vector<BasicMesh::Vertex> vertices;
    vertices.reserve(positions.size());
    for (auto& elem : positions) {
        auto color = isInterface(elem) ? colorMaxSurface : colorMax3D;
        vertices.push_back({elem, vec3(0.0f, 1.0f, 0.0f), elem, color});
    }

    mesh->addVertices(vertices);

    auto ind = mesh->addIndexBuffer(DrawType::Points, ConnectivityType::None);
    std::vector<std::uint32_t> indices;
    indices.resize(positions.size());
    std::iota(indices.begin(), indices.end(), 0);
    ind->append(indices);

    // update mesh matrices
    glm::mat4 modelMatrix, worldMatrix;
    ...

    mesh->setModelMatrix(modelMatrix);
    mesh->setWorldMatrix(worldMatrix);

    return mesh;
}
```

#### Using `inviwo::SimpleMesh`
Example taken from `AxisAlignedCutPlane`
```cpp
void AxisAlignedCutPlane::createBoundingBox() {
    boundingBoxMesh_ = util::make_unique<SimpleMesh>(DrawType::Lines, ConnectivityType::Strip);
    boundingBoxMesh_->addVertex(vec3(0, 0, 0), vec3(0, 0, 0), boundingBoxColor_.get());
    boundingBoxMesh_->addVertex(vec3(0, 0, 1), vec3(0, 0, 1), boundingBoxColor_.get());
    boundingBoxMesh_->addVertex(vec3(0, 1, 0), vec3(0, 1, 0), boundingBoxColor_.get());
    boundingBoxMesh_->addVertex(vec3(0, 1, 1), vec3(0, 1, 1), boundingBoxColor_.get());
    boundingBoxMesh_->addVertex(vec3(1, 0, 0), vec3(1, 0, 0), boundingBoxColor_.get());
    boundingBoxMesh_->addVertex(vec3(1, 0, 1), vec3(1, 0, 1), boundingBoxColor_.get());
    boundingBoxMesh_->addVertex(vec3(1, 1, 0), vec3(1, 1, 0), boundingBoxColor_.get());
    boundingBoxMesh_->addVertex(vec3(1, 1, 1), vec3(1, 1, 1), boundingBoxColor_.get());

    boundingBoxMesh_->addIndices(1, 0, 2, 3, 7, 5, 4, 6, 2, 0, 4, 5, 1, 3, 7, 6);
}
```

### Metadata

**Best practice:**\\
Separate meta data from the Data and use a port for the meta data.
See [Data Structures & Memory Representations](manual-devguide-data-memory.html) for more details on how the Data object works.


**Alternatives:**\\
Make a copy of the data and edit the copy. A solid solution but the data will be duplicated.


**Not recommended:**\\
This will work but you are not supposed to edit input data, it is an ugly hack:
```cpp
    auto input = inport_.getData();
    auto image = const_cast<Image*>(input.get());
    auto data = image->createMetaData<FloatMetaData>("data");

    outport_.setData(inport_.getData());
```

The Image constructor takes a `shared_ptr<Layer>` so this code would create another shared pointer, which has the same pointer as the inport's data. The pointer would be deleted twice, once per shared pointer:
```cpp
    auto layer = inport.getData()->getColorLayer();
    auto image = std::make_shared<Image>(layer);
    auto data = image->createMetaData<FloatMetaData>("data");
    outport.setData(image);
```
Why not to use: Sharing data between images can be problematic since you do not have control over the last edited representation anymore. One piece of code can edit the layer but it won't be known by the the other.
