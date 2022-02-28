# Importing attached OCR

When importing IIIF content, by default Madoc will search for attached OCR and attempt to import and convert it ready for correction. Any incompatible OCR will still be detected by Madoc and can be used if Madoc supports it in the future.

### Supported formats

OCR must be attached onto individual "Canvas" elements in your IIIF. They are found on the `seeAlso` property.&#x20;

The rules roughly follow the [Mirador Text overlay](https://github.com/dbmdz/mirador-textoverlay) plugin

* Line-level annotations with either one of:
  * a `motivation` that is `supplementing` (IIIF v3)
  * a resource that has a `@type` that is `cnt:contentAsText` (IIIF v2)
* A per-canvas `seeAlso` entry pointing to the ALTO or hOCR OCR markup for the page with either:
  * A `format` that is `application/xml+alto` or `text/vnd.hocr+html`
  * A `profile` starting with `http://www.loc.gov/standards/alto/`, `http://kba.cloud/hocr-spec`, `http://kba.github.io/hocr-spec/` or `https://github.com/kba/hocr-spec/blob/master/hocr-spec.md`

#### Mets Alto

Madoc supports OCR conforming to the Alto specification published by the Library of Congress. ([https://www.loc.gov/standards/alto/](https://www.loc.gov/standards/alto/))

```json5
{
  "id": "https://example.org/canvas-1",
  "type": "Canvas",
  "seeAlso": [
    {
      "id": "https://example.org/path/to/alto.xml",
      "motivation": "supplementing",
      "format": "application/xml+alto",
      "profile": "http://www.loc.gov/standards/alto/v3/alto.xsd" 
    }
  ]
}
```

The format **must** be either:

* none - left out completely
* Exactly match `application/xml`
* Exactly match `application/xml+alto`

The motivation should be `supplementing` although we do not check for this specifically to maximise compatibility (specifically with items converted from Presentation v2)

The profile **must** be exactly one of the following:

* `http://www.loc.gov/standards/alto/v3/alto.xsd`
* `http://www.loc.gov/standards/alto/v4/alto.xsd`

{% hint style="danger" %}
The links to the specification are considered canonical using the "http" protocol. Profiles starting with "https" **will not be detected** by Madoc.
{% endhint %}

#### hOCR

Madoc also supports hOCR, an open standard for data representation for formatted texts. It is a common output of popular OCR tools such as Tesseract. The specification can be found here: [http://kba.cloud/hocr-spec/1.2/](http://kba.cloud/hocr-spec/1.2/) or started with [Tesseract](https://tesseract-ocr.github.io/tessdoc/FAQ.html).

```json5
{
  "id": "https://example.org/canvas-1",
  "type": "Canvas",
  "seeAlso": [
    {
      "id": "https://example.org/path/to/hocr.html",
      "motivation": "supplementing",
      "format": "text/vnd.hocr+html",
      "profile": "http://kba.cloud/hocr-spec/1.2/" 
    }
  ]
}
```

The format **must** be `text/vnd.hocr+html` if provided.

The profile **must** start with one of the following:

* [http://kba.cloud/hocr-spec](http://kba.cloud/hocr-spec)
* [http://kba.github.io/hocr-spec/](http://kba.github.io/hocr-spec/)
* [https://github.com/kba/hocr-spec/blob/master/hocr-spec.md](https://github.com/kba/hocr-spec/blob/master/hocr-spec.md)

These are commonly followed by a version (e.g. http://kba.cloud/hocr-spec/1.2/).&#x20;

{% hint style="danger" %}
The links to the specification are considered canonical using the "http" protocol. Profiles starting with "https" **will not be detected** by Madoc.
{% endhint %}



### Importing in Madoc

When you import a Manifest into Madoc, you will see the following additional sections following your list of canvases. This indicates that Madoc has searched for OCR to import. It does not mean that it found OCR.

![The task page when importing a Manifest](<../../.gitbook/assets/Screenshot 2022-02-28 at 20.35.00.png>)



If Madoc finds compatible OCR, following the link above will show a screen showing the import static of each page of your Manifest.

![](<../../.gitbook/assets/Screenshot 2022-02-28 at 20.36.51.png>)

When viewing a Manifest in the Admin, you can see the status of the OCR and the formats that have been detected.

![](<../../.gitbook/assets/Screenshot 2022-02-28 at 20.45.31.png>)

When viewing each individual Canvas you can see both the original OCR document and the converted capture model that can be used as the basis for corrections.

![](<../../.gitbook/assets/Screenshot 2022-02-28 at 20.46.43.png>)

{% hint style="info" %}
If you choose "Save to madoc" you can store a local copy of the hOCR or Alto XML to Madoc. You will be able to edit as plaintext and paste in a new XML document. We don't offer the ability to add new linking properties at the moment. This functionality is also available through the Madoc API.
{% endhint %}

If you click on the "OCR Capture model data" link you will see a preview of what the capture model will look like to your contributors.

![This a read-only preview of your capture model generated from your OCR.](<../../.gitbook/assets/Screenshot 2022-02-28 at 20.49.22.png>)

This can be useful for validating a few pages after importing from a new source.

{% hint style="warning" %}
This preview UI uses an older version of the capture model UI and may not be completely representative of the UI seen by users, but should give a good indication that all of the data has been imported correctly.
{% endhint %}
