---
title: Contributing
tags: [dev_guide]
keywords: devguide
summary: "How to contribute to Inviwo"
sidebar: manual_sidebar
permalink: contributing.html
folder: manual
---
## Contributing to Inviwo
Contributions of any kind are welcome! We recommend you propose contributions in the form of a [GitHub Issue](https://github.com/inviwo/inviwo/issues/new) first, so we can discuss how to best realize your idea and help you on the way.
Also feel free to join [our Slack](https://join.slack.com/t/inviwo/shared_invite/enQtNTc2Nzc2NDQwNzIxLTRiMWM1ZWJiYjljZjkwNWE3OTk3MzYxODZlMDUyMzRmZjUzMzBiZjVhNTM3NWUyNzU1MjI4OWJjMzdkODViMzM) for discussions on the way.

### Bug Reports
1. Make one issue for each bug, do NOT put several bugs in one issue.
2. **Title** should be descriptive and (if possible) include "component"  (property,python,serialization etc).
example non-good ticket: Inviwo Crash.
3. Always include **steps to reproduce**, even if you think it is obvious, includit.
4. Are there any **warnings or errors**? Include them, it is easy just to copy paste.
5. Are there any possible **workarounds** that can be used until the bug has been fixed? If so, state them. If someone else find the same bug before it has been fixed they can also use that workaround.


### Coding Conventions
We follow the [Google coding conventions](https://google.github.io/styleguide/cppguide.html).

* Mimick the existing style of the core code
* Indents / no tabs (4 spaces)
* Const correctness
* Max 100 characters per line

It is recommended to use [ClangFormat](http://clang.llvm.org/docs/ClangFormat.html) to format code. There is a ".clang-format" file in the repo root.

### Contributing whole modules
If you are working on a large Inviwo Module and possibly have dependencies on big external libraries, we have an extra [modules repository](https://github.com/inviwo/modules).

#### Inviwo Marketplace
Additionally, there is the inviwo marketplace (see Screenshot) that allows developers to share their processors or shaders.
So after you have generated the code template for a processor by following the [guide for generating processors](https://inviwo.org/manual-devguide-meta.html), you should create a repository on [GitHub](https://github.com/) based on our [template](link-to-template).

##### Template
The template contains a workflow file for [GitHub Actions](https://github.com/features/actions). Using this workflow file, GitHub Actions can build your project automatically whenever you make a commit or manually invoke the build process. We provide an external action that retrieves all files that are necessary to build Inviwo. The workflow file in our template already invokes this action. Furthermore, the workflow file automatically creates a GitHub release after each commit and bundles the created artifacts with it.

##### Citing your contribution
[Zenodo](https://zenodo.org/) provides means to generate a DOI for GitHub Releases. GitHub provides an [easy-to-follow guide](https://guides.github.com/activities/citable-code/). After turning on the switch, Zenodo automatically generates a DOI for every new release. The last step of the guide also describes how to create a badge that always shows the latest DOI and links to it.
