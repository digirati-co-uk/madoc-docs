# IIIF Import API

{% swagger baseUrl="" path="/api/madoc/iiif/import/manifest" method="post" summary="Import Manifest" %}
{% swagger-description %}
Import a valid IIIF 2 or 3 manifest into madoc.
{% endswagger-description %}

{% swagger-parameter in="body" name="manifest" type="string" %}
URL of manifest to import
{% endswagger-parameter %}

{% swagger-response status="200" description="A task from the Tasks API is returned. This can be polled to get status of the import task. Canvases on the manifest will be added as subtasks" %}
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
{% endswagger-response %}
{% endswagger %}
