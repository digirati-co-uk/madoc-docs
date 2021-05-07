# Capture models

## Background

A capture model is a digital document that is presented to a user as a web form. Capture models are like digital paper forms. Once created they are digitally "photocopied" and given to users to fill out. For each image that is annotated there will be a pile of these filled out forms from various users. These are consolidated into a single form.

This metaphor fits the goals of the capture model format well. When someone contributes the data is stable. Each document not only describes the data, but how to correctly edit the data - just like a physical paper form. Luckily as these are digital we can do a lot more with the data.

{% hint style="info" %}
Each image has an associated capture model for each project it is in.
{% endhint %}

## Anatomy of a capture model

A capture model is composed of 2 primary pieces of information:

* **Document** - The data and how to edit it
* **Structure** - Subsets of the document grouped together into chunks easier for filling out.

The document should mirror how you would like the data to appear at the end of this whole process \(e.g. in JSON\) and the structure will drive how users are presented with that data. The structure allows you to give users a choice of what information they want to enter, perhaps based on what they are looking at in the image. This choice will drive which fields in the document are shown. At the end of the day though, regardless of the choice they make, they are still contributing to a single document.

### Revisions

An important aspect of capture models is the revision. Each time a user fills out some fields in a document, these fields are grouped together as a revision by that user. When a revision is created it does not become part of the canonical document until it is approved in a review. How this works is described in more detail in the [Reviews](../reviews/) section.

