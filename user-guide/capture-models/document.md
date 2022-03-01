# Document

The document in a capture model contains all of the data that you want to collect from an image, including how you want to collect it when displayed as a form to the user.

## Fields

The main building block in a document is a field. A field can be made up of single or multiple values. For example, if you had a set of images each containing a single person to be identified the goal may be to have a JSON structure for each image that looks something like this:

```javascript
{
    "firstName": " ... ",
    "familyName": " ... "
}
```

If you were tasked with creating a web form to be included in a crowdsourcing project you would want it to follow some basic rules:

* 2 text boxes to be shown, one for each name
* Correct labels on the form
* Both text boxes should be single lines

In a capture model document the data and how to edit the data is stored together in the format. To achieve the above rules your document may look like this:

{% tabs %}
{% tab title="Document before data entry" %}
```javascript
{
  "firstName": [{
    "id": "15d21383-7f8e-4d7a-8f41-1b52261d0130",
    "label": "First name"
    "type": "text-field",
    "allowMultiple": false,
    "multiline": false,
    "value": ""
  }],
  "familyName": [{
    "id": "f0db952b-c511-47fa-8217-7ca0e99734e7",
    "label": "Family name"
    "type": "text-field",
    "allowMultiple": false,
    "multiline": false
    "value": ""
  }]
}
```
{% endtab %}

{% tab title="Document after data entry" %}
```javascript
{
  "firstName": [{
    "id": "15d21383-7f8e-4d7a-8f41-1b52261d0130",
    "label": "First name"
    "type": "text-field",
    "allowMultiple": false,
    "multiline": false,
    "value": "John"
  }],
  "familyName": [{
    "id": "f0db952b-c511-47fa-8217-7ca0e99734e7",
    "label": "Family name"
    "type": "text-field",
    "allowMultiple": false,
    "multiline": false
    "value": "Smith"
  }]
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
This example has some extra configuration fields that are the default and are optional but included for clarity.
{% endhint %}

This is the format in its verbose form and how it's stored and transported. However for authoring models the format can be quite difficult to quickly read and understand. Throughout this documentation the document will be described using the short hand described below.

We have also created a UI that lets you build these capture models up.

![](<../../.gitbook/assets/Screenshot 2021-05-06 at 18.13.25.png>)

### Short hand

The capture model format is a verbose format made to be verifiably correct and portable. It is not made to small or compact. However we have tools that allow you to build up capture models using a short hand syntax.

{% tabs %}
{% tab title="Short hand" %}
```javascript
{
  "name": "text-field"
}
```
{% endtab %}

{% tab title="Full format" %}
```javascript
{
  "name": [
    {
      "id": "f3c0930e-812c-4693-91fd-3cdb50951976",
      "type": "text-field",
      "label": "Name",
      "value": ""
    }
  ]
}
```
{% endtab %}

{% tab title="Output JSON" %}
```
{
  "name": ""
}
```
{% endtab %}

{% tab title="User preview" %}
![](<../../.gitbook/assets/Screenshot 2021-05-06 at 18.04.48.png>)
{% endtab %}
{% endtabs %}

The short hand allows you to describe your field quickly and generate the default values and identifiers. The short hand progressively enhances to the full format, filling in any blanks. For example you can still describes labels:

{% tabs %}
{% tab title="Short hand" %}
```javascript
{
  "name": {
    "type": "text-field",
    "label": "Full name",
    "description": "The full name of the person"
  }
}
```
{% endtab %}

{% tab title="Full format" %}
```javascript
{
  "name": [
    {
      "id": "f3c0930e-812c-4693-91fd-3cdb50951976",
      "type": "text-field",
      "label": "Full name",
      "description": "The full name of the person"
      "value": ""
    }
  ]
}
```
{% endtab %}

{% tab title="Output JSON" %}
```javascript
{
  "name": ""
}
```
{% endtab %}

{% tab title="User preview" %}
![](<../../.gitbook/assets/Screenshot 2021-05-06 at 18.09.16.png>)
{% endtab %}
{% endtabs %}

The short hand syntax may be used in these examples to describe more complex models in a compact way. If you want to create your own capture models and are using Javascript in the browser or in a Node environment you can use the helper library:

```javascript
import { captureModelShorthand } from '@capture-models/helpers';

const document = captureModelShortHand({
  name: 'text-field',
});
```

### Built-in field types

All of the field types are available in the capture model [Storybook](https://capture-models.netlify.app/?path=/story/input-types-autocomplete--with-field-wrapper) where you can preview each.

| Name              | Description                                                                                                                        |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Autocomplete**  | An autocomplete box that allows you to connect a compatible endpoint and search for values to be used as the value.                |
| **Checkbox**      | Single checkbox with an optional inline label giving you a boolean value in your document.                                         |
| **Checkbox list** | List of checkboxes, each with a label and a key. Will result in a map of the values and a boolean value.                           |
| **Dropdown**      | Fixed dropdown with labels/values for multiple values.                                                                             |
| **HTML**          | A very simple WYSIWYG for creating formatted HTML snippets.                                                                        |
| **Tagged text**   | A custom component created for one of our partners for transcribing text with tags such as super script, deletions or unclear text |
| **Text field**    | A single or multi-line text input.                                                                                                 |

Together these fields types let you build up complex data models for users to easily fill out.

Fields are written in React and can be customised and expanded. We are always looking for new field types to create. Check out a [definition](https://github.com/digirati-co-uk/capture-models/blob/master/packages/editor/src/input-types/TextField/index.ts), [component](https://github.com/digirati-co-uk/capture-models/blob/master/packages/editor/src/input-types/TextField/TextField.tsx) and [editor](https://github.com/digirati-co-uk/capture-models/blob/master/packages/editor/src/input-types/TextField/TextField.editor.tsx) on our Github.

## Entities

In a document an entity is a collection of fields grouped together. If you wanted to identify people in an image you may use an entity to model this in your document.

{% tabs %}
{% tab title="Short hand" %}
```javascript
{
  "person.firstName": "text-field",
  "person.familyName": "text-field"
}
```
{% endtab %}

{% tab title="Full model" %}
```javascript
{
  "person": [
    {
      "id": "6c278bf6-57de-4a37-accd-3eb906b2211a",
      "type": "entity",
      "label": "Person"
      "properties": {
        "firstName": [
          {
            "id": "cee51a05-1899-4258-a111-37f8d092c7a0",
            "label": "First name",
            "type": "text-field",
            "value": ""
          }
        ],
        "familyName": [
          {
            "id": "3f6be2e2-d184-4343-a059-8cbd8d1f3288",
            "label": "Family name",
            "type": "text-field",
            "value": ""
          }
        ],
      }
    }
  ]
}
```
{% endtab %}
{% endtabs %}

Grouping together properties like this allows you to create rich data structures. Combined with the `allowMultiple` option it allows for multiple set of properties to be recorded. In this example you could allow multiple on the "person" entity and the user could add the names of multiple people.

{% hint style="info" %}
It is recommended to keep your models shallow wherever possible. The model is generic enough to allow very deeply nested and complex structures, however these become much more complex to construct and manage.
{% endhint %}

## Selectors

When annotating it is common for information to not only be associated with the whole image, but instead a smaller region of that image. Each field can have an associated "selector" to narrow down the area of the image being targeted.

Currently we only have a box selector that allows users to associate a field, or entity, with a boxed region on the image.

## Revisions

When a user submits their contribution to an images capture model, the information that was there previously is not removed. Instead the revised field sits alongside the original:

```javascript
{
  "name": [
    {
      "id": "f3c0930e-812c-4693-91fd-3cdb50951976",
      "type": "text-field",
      "label": "Name",
      "value": ""
    },
    {
      "id": "288d244a-e5c8-4d38-ba2a-1c78c4a363de",
      "type": "text-field",
      "label": "Name",
      "value": "John smith", // The value entered by the user.
      "revises": "f3c0930e-812c-4693-91fd-3cdb50951976", // The field it revises
      "revisionId": "49f30cbf-d285-4218-87ba-1dba84afaa0f" // ID for their work
    }
  ]
}
```

This model is described in more detail in the developer documentation. This plays a role in Madoc when a user loads up a capture model for an image. They will only see published fields and their own revisions and not other users submissions that have not been accepted.
