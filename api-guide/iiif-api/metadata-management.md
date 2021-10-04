---
description: Get and update metadata and descriptive properties in IIIF.
---

# Metadata management

IIIF Metadata and descriptive properties are stored in Madoc individually by the property they are assigned, language they are in and the position they appear if there are more than one.

When a piece of IIIF is imported into a site 2 copies of the metadata is stored:

* **Canonical metadata** - does not change, used when subsequently imported into a different site
* **Site metadata** - can be changed, used when displaying on a specific site

This allows the same resource to be imported into multiple sites with the original metadata, even if the metadata is changed on a particular site. The data model contains columns for future work to support more complex syncing for these use-cases.

These rows are transformed into IIIF-compliant language fields when requesting specific IIIF resources. For managing and editing the fields, this expanded format is used to avoid ambiguity.

### Data model

{% tabs %}
{% tab title="Label" %}
```javascript
{
  "id": 1,
  "key": "label",
  "value": "Scottish Bridges",
  "language": "none",
  "source": "iiif",
  "resource_id": 1,
  "site_id": null,
  "readonly": false,
  "edited": false,
  "auto_update": true,
  "data": null
}
```
{% endtab %}

{% tab title="Metadata label" %}
```javascript
{
  "id": 4,
  "key": "metadata.0.label",
  "value": "Title",
  "language": "none",
  "source": "iiif",
  "resource_id": 2,
  "site_id": null,
  "readonly": false,
  "edited": false,
  "auto_update": true,
  "data": null
}
```
{% endtab %}

{% tab title="Metadata value" %}
```javascript
{
  "id": 5,
  "key": "metadata.0.value",
  "value": "Forth Bridge illustrations 1886-1887",
  "language": "none",
  "source": "iiif",
  "resource_id": 2,
  "site_id": null,
  "readonly": false,
  "edited": false,
  "auto_update": true,
  "data": null
}
```
{% endtab %}
{% endtabs %}

| **Column** | **Description** |
| :--- | :--- |
| **id** | Numeric ID for the individual Metadata value |
| **key** | Dot notation for property in the IIIF \(e.g. `metadata.0.label` \) |
| **value** | The value of the metadata item |
| **language** | Language code as described in the [IIIF Specification](https://iiif.io/api/presentation/3.0/#44-language-of-property-values) |
| **source** | Where this value was sourced from, usually always `iiif` . In future could be used to distinguish different sources of metadata. |
| **resource\_id** | The numeric ID for the IIIF resource this metadata will be attached to |
| **site\_id** | Metadata is per-site, so each site will copy the metadata when importing. If this metadata is changed, it will not be reflected on other sites. If this is `null` then the data is the canonical \(from import\). This allows for some reverting features in the future. |
| **readonly** | Currently unused, but when set will prevent manually updating this value. Intended to be used when metadata has been derived from another source. |
| **auto\_update** | When this is set, if the canonical \(site\_id = null\) is updated, then this will also be updated.  |
| **edited** | This indicates that the value has been updated in Madoc. Usually sets `auto_update` to false once updated. |
| **data** | Unstructured JSON field, could be used to store useful information about the field. Currently unused. |



### API Endpoints

The following API endpoints are currently only accessible from a JWT with a `site.admin` scope. 

* Metadata keys
* Metadata values
* Update metadata

The reading endpoints for IIIF 



{% api-method method="get" host="" path="/api/madoc/iiif/metadata-keys" %}
{% api-method-summary %}
Metadata keys
{% endapi-method-summary %}

{% api-method-description %}
For a given resource id \(IIIF\) it will return a list of unique labels in the metadata grouped by language along with how many instances of the label exists. This is used for configuring search, showing the most popular facets.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="jwt scope" type="string" required=true %}
site.admin
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
List of unique labels and number of occurrences.
{% endapi-method-response-example-description %}

```javascript
{
  "metadata": [
    {
      "label": "Title",
      "total_items": 101,
      "language": "none"
    },
    {
      "label": "Full conditions of use",
      "total_items": 37,
      "language": "none"
    },
    {
      "label": "Collection",
      "total_items": 30,
      "language": "none"
    },
    {
      "label": "Provenance",
      "total_items": 30,
      "language": "none"
    },
    {
      "label": "Barcode",
      "total_items": 30,
      "language": "none"
    },
    {
      "label": "Edition",
      "total_items": 30,
      "language": "none"
    }
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="" path="/api/madoc/iiif/metadata-values" %}
{% api-method-summary %}
Metadata values
{% endapi-method-summary %}

{% api-method-description %}
For a given resource id \(IIIF\) it will return a unique list of values from inside the metadata along side how many instances of the value exists. This is used for configuring search to fix certain facets into the search sidebar.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="jwt scope" type="string" required=true %}
site.admin
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="label" type="string" required=true %}
Which metadata label should be filtered \(e.g. "Title"\)
{% endapi-method-parameter %}

{% api-method-parameter name="page" type="integer" required=false %}
Page of results
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns all of the values for the given Metadata key/label. Ordered by frequency and paged.
{% endapi-method-response-example-description %}

```javascript
{
  "page": 1,
  "values": [
    {
      "total_items": 30,
      "language": "none",
      "value": "You have permission to make copies of this work under the <a target=\"_top\" href=\"http://creativecommons.org/licenses/by/4.0/\">Creative Commons Attribution 4.0 International Licence</a> unless otherwise stated."
    },
    {
      "total_items": 4,
      "language": "none",
      "value": "You have permission to make copies of this work under the <a target=\"_top\" href=\"http://creativecommons.org/licenses/by/4.0/\">Creative Commons Attribution 4.0 International Licence</a> unless otherwise stated."
    },
    {
      "total_items": 2,
      "language": "none",
      "value": "You have permission to make copies of this work under the <a target=\"_top\" href=\"http://creativecommons.org/licenses/by/4.0/\">Creative Commons Attribution 4.0 International Licence</a> unless otherwise stated."
    },
    {
      "total_items": 1,
      "language": "none",
      "value": "You have permission to make copies of this work under the <a target=\"_top\" href=\"http://creativecommons.org/licenses/by/4.0/\">Creative Commons Attribution 4.0 International Licence</a> unless otherwise stated."
    }
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="" path="/api/madoc/iiif/canvases/:canvas\_id/metadata" %}
{% api-method-summary %}
Canvas metadata
{% endapi-method-summary %}

{% api-method-description %}
Returns all of the descriptive properties and metadata for a canvas in a flat format \(id, key, value, language, source\). This format removes ambiguity with lists of fields and different languages, assigning an identifier to each.   
  
This allows single values to be updated without any notion of position in the IIIF-JSON. For example “update the second dutch value in the metadata list where the label is Description” is prone to error.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="canvas\_id" type="integer" required=true %}
Numeric ID for the canvas
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="jwt scope" type="string" required=true %}
site.read
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns all of the metadata in a flat list \(see data model\)
{% endapi-method-response-example-description %}

```javascript
{
  "fields": [
    {
      "id": 1892,
      "key": "label",
      "value": "Front cover",
      "language": "en",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    }
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="" path="/api/madoc/iiif/manifests/:manifest\_id/metadata" %}
{% api-method-summary %}
Manifest metadata
{% endapi-method-summary %}

{% api-method-description %}
Returns all descriptive properties and metadata for a manifest in a flat format.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="manifest\_id" type="integer" required=false %}
Numeric ID for the manifest
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="jwt scope" type="string" required=true %}
site.read
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "fields": [
    {
      "id": 1087,
      "key": "label",
      "value": "Aberdeenshire",
      "language": "none",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    },
    {
      "id": 1088,
      "key": "metadata.0.label",
      "value": "Title",
      "language": "none",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    },
    {
      "id": 1089,
      "key": "metadata.0.value",
      "value": "Aberdeenshire",
      "language": "none",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    },
    {
      "id": 1090,
      "key": "metadata.1.label",
      "value": "Place in text",
      "language": "none",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    },
    {
      "id": 1091,
      "key": "metadata.1.value",
      "value": "Aberdeenshire",
      "language": "none",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    },
    {
      "id": 1092,
      "key": "metadata.2.label",
      "value": "Place depicted",
      "language": "none",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    },
    {
      "id": 1093,
      "key": "metadata.2.value",
      "value": "Aberdeen",
      "language": "none",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    },
    {
      "id": 1094,
      "key": "metadata.3.value",
      "value": "<a href=\"https://digital.nls.uk/97134287\">View in our digital gallery</a>",
      "language": "none",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    },
    {
      "id": 1095,
      "key": "metadata.4.label",
      "value": "Full conditions of use",
      "language": "none",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    },
    {
      "id": 1096,
      "key": "metadata.4.value",
      "value": "You have permission to make copies of this work under the <a target=\"_top\" href=\"http://creativecommons.org/licenses/by/4.0/\">Creative Commons Attribution 4.0 International Licence</a> unless otherwise stated.",
      "language": "none",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    },
    {
      "id": 1097,
      "key": "requiredStatement.label",
      "value": "Attribution",
      "language": "none",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    },
    {
      "id": 1098,
      "key": "requiredStatement.value",
      "value": "National Library of Scotland<br/>License: <a target=\"_top\" href=\"http://creativecommons.org/licenses/by/4.0/\">CC BY 4.0</a>",
      "language": "none",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    }
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="" path="/api/madoc/iiif/collections/:collection\_id/metadata" %}
{% api-method-summary %}
Collection metadata
{% endapi-method-summary %}

{% api-method-description %}
Returns all descriptive properties and metadata for a manifest in a flat format.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="collection\_id" type="string" required=false %}
Numeric ID for the collection
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="jwt scope" type="string" required=true %}
site.read
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "fields": [
    {
      "id": 1892,
      "key": "label",
      "value": "My collection",
      "language": "en",
      "source": "iiif",
      "edited": false,
      "auto_update": true,
      "readonly": false,
      "data": null
    }
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

