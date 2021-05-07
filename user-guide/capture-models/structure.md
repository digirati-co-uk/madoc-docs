# Structure

The structure of the capture model aims to make it easier to contribute values to the document values by splitting down the fields into chunks. For example, you may be transcribing a document while also identifying people, place names and allowing people to write comments or notes. Instead of presenting the user with one giant form with all of these values, you can use a structure to split these up into a set of choices:

* **What would you like to Annotate?**
  * Transcribe the text
  * **Identify entity**
    * Person
    * Place
  * Write a note

The user could click through these options and only see the fields they need to see. Some of these may overlap, for example you may have the notes under the transcription. This is all supported when setting up the structure.![](blob:https://digirati.atlassian.net/4cb19e91-fd75-43ea-8637-9c975818b3b3#media-blob-url=true&id=dd0a4d33-283c-41f8-bc9a-770829c2f86a&collection=contentId-1476296713&contextId=1476296713&mimeType=image%2Fpng&name=Screenshot%202019-12-10%20at%2016.57.31.png&size=45419&width=2592&height=2378)

The structure itself is a nested set of JSON objects. There are 2 types of objects, **choices** and **models**:

### **Choices**

A choice is simply a group of other choices and models displayed as selectable options to the user. In our example above "What would you like to Annotate" and "Identify entity" are both choices. Choices contain display information, such as labels and descriptions to the user that they will see when selecting their choice.![](blob:https://digirati.atlassian.net/b95bcf06-7dd4-4f55-a8cd-2a521a6d18ad#media-blob-url=true&id=0e6ec0eb-5b8b-4493-a065-82d082ec0306&collection=contentId-1476296713&contextId=1476296713&mimeType=image%2Fpng&name=Screenshot%202019-12-10%20at%2016.58.00.png&size=92121&width=2592&height=2058)

### Models

A model is a form, from a users perspective. The model contains a list of the fields from the **document** that should be shown in the form.

## Examples

Default single choice.

{% tabs %}
{% tab title="Structure" %}
```javascript
{
  "id": "9c2c6558-703d-4276-ac44-01c78e66ecef",
  "type": "model",
  "label": "Default",
  "fields": [
    "firstName",
    "familyName"
  ]
}
```
{% endtab %}

{% tab title="Model short hand" %}
```javascript
{
  "firstName": "text-field",
  "familyName": "text-field"
}
```
{% endtab %}
{% endtabs %}

Splitting the same document into 2 choices.

{% tabs %}
{% tab title="Structure" %}
```javascript
{
  "id": "db314a02-b857-430a-98dd-0f49d12dd978",
  "type": "choice",
  "label": "Choose something",
  "items": [
    {
      "id": "9c2c6558-703d-4276-ac44-01c78e66ecef",
      "type": "model",
      "label": "First name",
      "description": "Fill out the first name",
      "fields": [
        "firstName"
      ]
    },
    {
      "id": "c46f3fcb-6ecc-46b2-8bee-5adf1c874da4",
      "type": "model",
      "label": "Family name",
      "description": "Fill out the family name",
      "fields": [
        "familyName"
      ]
    }
  ]
}
```
{% endtab %}

{% tab title="User interface" %}
![](../../.gitbook/assets/screenshot-2021-05-07-at-11.08.44.png)
{% endtab %}
{% endtabs %}

This is not a realistic example as these two fields would be better inside of a single contribution.

