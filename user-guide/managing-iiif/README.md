# Managing IIIF

![](../../.gitbook/assets/image%20%284%29.png)

The primary building block for content in Madoc is IIIF \(International Image Interoperability Framework™\). IIIF is a set of open standards for standardising access to images and collections of images. There are 3 primary building blocks that make up IIIF.

### Jump to section

* \*\*\*\*[**Creating IIIF collections**](collections.md#creating-a-new-collection)\*\*\*\*
* \*\*\*\*[**Importing IIIF collections**](collections.md#importing-an-existing-collection)\*\*\*\*
* \*\*\*\*[**Importing IIIF Manifests**](manifests.md#importing-an-existing-manifest)\*\*\*\*
* \*\*\*\*[**Managing canvases**](canvases.md)\*\*\*\*

#### Canvas

The first is the canvas. A canvas is a digital representation of an individual part of an object. This might be a single page in a book or a single photograph. Canvases are the building blocks for IIIF and aim to represent their counterparts as accurately as possible.

The most common form a canvas takes is a static or deep zoom image. 

From the IIIF Specification:

> A virtual container that represents a particular view of the object and has content resources associated with it or with parts of it. The Canvas provides a frame of reference for the layout of the content, both spatially and temporally. The concept of a Canvas is borrowed from standards like PDF and HTML, or applications like Photoshop and PowerPoint, where an initially blank display surface has images, video, text and other content “painted” on to it by Annotations, collected in Annotation Pages.

#### Manifest

A manifest will be made up of one or more canvases. For example, a book make contain a single canvas for each of the pages of a book, including the front and back covers. The manifest contains the structural information to bind the book together. Ranges of canvases could be constructed to represent chapters or a table of contents. 

From the IIIF Specification:

> A description of the structure and properties of the compound object. It carries information needed for the client to present the content to the user, such as a title and other descriptive information about the object or the intellectual work that it conveys. Each Manifest usually describes how to present a single compound object such as a book, a statue or a music album.

#### Collection

A IIIF collection is a set of one or more manifests \(and/or other collections\) and provides an organisational tool for grouping together objects.

From the IIIF Specification:

> An ordered list of Manifests, and/or further Collections. Collections allow Manifests and child Collections to be grouped in a hierarchical structure for presentation, which can be for generating navigation, showing dynamic results from a search, or providing fixed sets of related resources for any other purpose.



