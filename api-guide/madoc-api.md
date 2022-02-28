# Madoc API

{% swagger baseUrl="" path="/api/madoc/projects/:project_id/personal-notes/:resource_id" method="get" summary="Current users notes for an object" %}
{% swagger-description %}
View any notes made by the user in a project on a specific resource
{% endswagger-description %}

{% swagger-parameter in="path" name="resource_id" type="number" %}
Numeric identifier of canvas
{% endswagger-parameter %}

{% swagger-parameter in="path" name="project_id" type="string" %}
Numeric identifier or slug of project
{% endswagger-parameter %}

{% swagger-response status="200" description="You will always be returned a note if the user is able to make a note. If no note exists in the database, an empty note will be returned." %}
```
{
  "note": "A note made by a user"
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger baseUrl="" path="/api/madoc/projects/:project_id/personal-notes/:resource_id" method="put" summary="Update users note for an object" %}
{% swagger-description %}
Update the contents of a users note
{% endswagger-description %}

{% swagger-parameter in="path" name="project_id" type="string" %}
Numeric identifier or slug of a project
{% endswagger-parameter %}

{% swagger-parameter in="path" name="resource_id" type="number" %}
Numeric identifier of canvas
{% endswagger-parameter %}

{% swagger-parameter in="body" name="note" type="string" %}
The updated note
{% endswagger-parameter %}

{% swagger-response status="201" description="No body will be returned after creation." %}
```
```
{% endswagger-response %}
{% endswagger %}
