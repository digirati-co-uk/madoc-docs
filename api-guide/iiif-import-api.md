# IIIF Import API

{% api-method method="post" host="" path="/api/madoc/iiif/import/manifest" %}
{% api-method-summary %}
Import Manifest
{% endapi-method-summary %}

{% api-method-description %}
Import a valid IIIF 2 or 3 manifest into madoc.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="manifest" type="string" required=true %}
URL of manifest to import
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
A task from the Tasks API is returned. This can be polled to get status of the import task. Canvases on the manifest will be added as subtasks
{% endapi-method-response-example-description %}

```javascript
{
  "id": "8ecff060-a90d-438e-876c-2a598562e891",
  "type": "madoc-manifest-import",
  "status": 1,
  "status_text": "accepted",
  "state": {
    "resourceId": "..."
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

