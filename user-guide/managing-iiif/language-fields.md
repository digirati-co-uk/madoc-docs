# Language fields

Any labels or metadata that is imported into Madoc is imported with a language associated with each value. If your content has multi-lingual labels, these will be detected. This is done by upgrading IIIF to the latest presentation version before processing.

{% tabs %}
{% tab title="Input Manifest" %}
```javascript
{
  "@id": " ... ",
  "@type": "sc:Manifest",
  "label": "My manifest",
  ...
}
```
{% endtab %}

{% tab title="Processed Manifest" %}
```javascript
{
  "id": " ... ",
  "type": "Manifest",
  "label": {
    "en": ["My manifest"]
  },
  ...
}
```
{% endtab %}
{% endtabs %}

As a result every field in your IIIF can be translated. This is built into the foundation of Madoc and its internationalisation, so wherever these labels are displayed they will be driven by the users choice of language.

{% hint style="warning" %}
We are still working on the UX of editing these language fields
{% endhint %}

