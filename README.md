# Inviwo Website
This document gives a brief overview of how this website works.
For detailed issues, consult [the documentation of its template](https://idratherbewriting.com/documentation-theme-jekyll/).
## Single important files:
- **_config.yml**: Set the website title, GitHub repo, local build settings.
    - Also set Markdown and linting libraries
    - Setup defaults for page layouts, sidebars, topnavs
    - Define sidebars
- **index.md**: The landing page
- **_data/topnav.yml**: Configure top navigation bar
- **_data/sidebars/*.yml**: Configure side bars (documentation is all in manual)
- **css/customstyles.css**: Add custom styles here

## Adding new posts
In the `pages/` directory there are subfolders for groups of pages. The hand-written documentation files and manuals go into `pages/manual/`, API docs in `pages/cppapi/` and `pages/pyapi/` respectively.
To create a new post, add a new Markdown file to the appropriate folder.
Each post has a header, similar to the following:
```
---
title: Create your own property
tags: [dev_guide, c++]
keywords: devguide, c++
summary: "Learn to create your own C++ properties for use in Inviwo"
sidebar: manual_sidebar
permalink: manual-devguide-build-property.html
folder: manual
---
```
From the listed fields the `title`, `folder` and `permalink` field are necessary. Adding a `sidebar` here determines which sidebar is shown next to the page (`none` is also an option).

Below this header you can write normal Markdown which is compilant with [Kramdown](https://kramdown.gettalong.org/index.html). You can also embed HTML and [Liquid HTML](https://shopify.github.io/liquid/basics/introduction/), which can be used in conjunction with includes (see `_includes/`) to get around most Markdown restrictions (like embedding videos)

In order to reach the newly created post somehow, you should add it to a sidebar by adding an entry to `_data/sidebars/your_sidebar.yml` consisting of `title`, `output` and `url`. Compare with existing sidebars to get the hierarchy right.
Note that the `sidebar` header field in your new Markdown post only determines which sidebar is shown, not which this page is shown in.

Similarly you could add your new page to the top navigation bar by adding a new entry in `_data/topnav.yml` with `title` and `url`.

### Adding images and media
You can put your images into the `images/` folder, preferrably in a suitable sub folder and use them directly in your Markdown (address them using `images/...`).

Additional media like `.pdfs` can be put in `media/` to be made downloadable through a link to `/media/.../file.pdf`

### References
You can reference markdown headings using links like so: `[link name](#heading)`. You can also get a direct link to a heading by clicking the link symbol next to your heading on your served website.
Similarly you can link to all internal pages by referencing the `permalink` that you set in their headers. E.g `[link name](manual-gettingstarted-ui.html)`. You can also combine this with the Markdown heading to scroll down to a heading automatically (e.g. `[link name](manual-gettingstarted-ui.html#processors)`).


## Adding new templates
You can make arbitrarily complex HTML templates inside `_includes/` and embed them into Markdown using Liquid's include tags: `{% include template.html param="..." %}`. You can access the given parameters in the template using `{{include.param}}`.
