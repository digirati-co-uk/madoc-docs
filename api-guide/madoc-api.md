# Madoc API

{% api-method method="get" host="" path="/api/madoc/projects/:project\_id/personal-notes/:resource\_id" %}
{% api-method-summary %}
Current users notes for an object
{% endapi-method-summary %}

{% api-method-description %}
View any notes made by the user in a project on a specific resource
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="resource\_id" type="number" required=true %}
Numeric identifier of canvas
{% endapi-method-parameter %}

{% api-method-parameter name="project\_id" type="string" required=true %}
Numeric identifier or slug of project
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
You will always be returned a note if the user is able to make a note. If no note exists in the database, an empty note will be returned.
{% endapi-method-response-example-description %}

```
{
  "note": "A note made by a user"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="put" host="" path="/api/madoc/projects/:project\_id/personal-notes/:resource\_id" %}
{% api-method-summary %}
Update users note for an object
{% endapi-method-summary %}

{% api-method-description %}
Update the contents of a users note
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="project\_id" type="string" required=true %}
Numeric identifier or slug of a project
{% endapi-method-parameter %}

{% api-method-parameter name="resource\_id" type="number" required=true %}
Numeric identifier of canvas
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="note" type="string" required=true %}
The updated note
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
No body will be returned after creation.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

