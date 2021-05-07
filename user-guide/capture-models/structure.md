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

#### **Choices**

A choice is simply a group of other choices and models displayed as selectable options to the user. In our example above "What would you like to Annotate" and "Identify entity" are both choices. Choices contain display information, such as labels and descriptions to the user that they will see when selecting their choice.![](blob:https://digirati.atlassian.net/b95bcf06-7dd4-4f55-a8cd-2a521a6d18ad#media-blob-url=true&id=0e6ec0eb-5b8b-4493-a065-82d082ec0306&collection=contentId-1476296713&contextId=1476296713&mimeType=image%2Fpng&name=Screenshot%202019-12-10%20at%2016.58.00.png&size=92121&width=2592&height=2058)

#### Models

A model is a form, from a users perspective. The model contains a list of the fields from the **document** that should be shown in the form.

