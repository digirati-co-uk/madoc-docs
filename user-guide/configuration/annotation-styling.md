# Annotation styling

In Madoc there are different places where you can see annotations. Since Madoc is both an authoring platform for annotations, and a presentation platform it can be useful to distinguish between different types of annotations.

### Presenting annotations

When you view a canvas on Madoc, you will see a sidebar with links to view annotations that have been made on. From here you can see the annotations in a few different ways.

* **Annotations** - simple annotations, similar to Mirador, with some text + a target
* **Transcriptions** - text based representation
* **Document** - complex JSON-LD documents attached to the canvas - usually output of crowd sourcing.
* **Submissions** - when crowd sourcing, your submissions will appear here.

By default none of these annotations will be visible before you open the left hand panel. You may choose to display one or more of these by default. Note this may slow down your pages.&#x20;

{% hint style="warning" %}
Some annotations may appear in both the "Annotations" and "Document" panel. If you show both at the same time, they may overlap.
{% endhint %}

### Editing annotations

When you are editing a deeply nested capture model you may be working with many selectors, such as in [OCR correction](../ocr/adding-ocr-correction-to-capture-model.md). Whenever you navigate a nested model, we will maintain 4 "buckets" of annotations. These can move around as you move around. They are used to show/hide the annotations, or style them.

* **Top level selector** - If there is a single top level selector, like when adding a single poem - this might be the outline of the poem.
* **Current level selectors** - if you were editing a single "word" in OCR correction, this would be the word.
* **Adjacent selectors** - if you were editing a single "word", this would be the rest of the words in the line.
* **Hidden selectors** - everything else

We render hidden selectors, but without a style. It's not recommended to add style to these. If there are classifications of selectors that do not fit this list, let us know!

When you are editing annotations, there are some other types of annotations you may want users to see from the submission panel (mentioned above):

* Previous accepted submissions from users.
* Your previous submissions on the image

The complication here is they may be more than one per submission, and they may be in a hierarchy. This variability makes a one-size-fits-all unlikely.&#x20;

### Customisations

You can customise the 8 types of annotations described above (with regions)

* Annotations panel
* Document panel
* Submission panel
* Current submission
  * Top level
  * Current level
  * Adjacent
  * Hidden

In the admin interface you will see customisation options for each of these types. By default all of the panel annotations are only visible when you hover over the annotations/item in the panel. This styling is shared with "active" styling (i.e. when the mouse is down). The following options are available for _each_ type of annotation:

* Background colour (colour + opacity)
* Border (weight + colour)
* Hover styles (border + background colour overrides)
* Active styles (border + background colour overrides, active = pressing in this context)
* Interactive (checkbox - will it respond to hovers)
* Hidden (checkbox - will be always hidden)

When you are editing those options you will see a preview of what your annotation will look like. You can choose a canvas from your project to test.&#x20;

Once you have created your colour theme in the admin, you can apply it to one of your projects from the configuration page.&#x20;

**\[steps TBC]**

