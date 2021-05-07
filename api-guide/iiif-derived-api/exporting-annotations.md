---
description: There are a few ways to export annotations.
---

# Exporting annotations

When a user contributes to an image, the target of that contribution is a canvas and the output is stored as a capture model. Each project has a capture model that is used as a template for each canvas. A canvas may be in multiple projects and as a result have multiple capture models associated with it.

The endpoint for exporting annotations and models is here:

```text
/s/default/madoc/api/canvases/[numeric_canvas_id]/models
```

This endpoint supports multiple query parameters.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Query param</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>format</code>
      </td>
      <td style="text-align:left">
        <p>The format that should be returned. Default: <code>capture-model</code>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <p><b>Valid values:</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;</p>
        <ul>
          <li><code>capture-model</code>
          </li>
          <li><code>capture-model-with-pages</code>
          </li>
          <li><code>open-annotation</code>
          </li>
          <li><code>w3c-annotation</code>
          </li>
          <li><code>json</code>
          </li>
          <li><code>w3c-annotation-pages</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>project_id</code>
      </td>
      <td style="text-align:left">The project slug or numeric identifier. This will filter to only show
        contributions from one project.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>derived_form</code>
      </td>
      <td style="text-align:left">If you know the ID of the capture model, you can use this to specify exactly
        which capture model should be used when filtering. This will query for
        the <code>derived_from</code> property on the capture model.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>selectors</code>
      </td>
      <td style="text-align:left">This is a boolean value. If set to <code>true</code> then only annotations
        that have selectors will be returned. This only applies to annotation formats.
        This can be useful if you want to display bounding boxes on an image and
        don&apos;t care about items that do not have regions defined.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>model_id</code>
      </td>
      <td style="text-align:left">Override the capture model that is fetched to provide annotations. (unlikely
        to be required in normal use of the API)</td>
    </tr>
  </tbody>
</table>

## **Example requests**

These examples are long and consist of a few contributions from a few projects.

### Returning full published capture models

```javascript
{
  "models": [
    {
      "id": "254a73c7-2a18-4ede-b541-e6a43eecac78",
      "structure": {
        "id": "16e4789c-6866-4271-bdfd-ee5220e9ffeb",
        "type": "choice",
        "label": "First project",
        "items": [
          {
            "id": "9c2c6558-703d-4276-ac44-01c78e66ecef",
            "type": "model",
            "label": "Default",
            "fields": [
              "regionOfInterest"
            ]
          }
        ]
      },
      "document": {
        "id": "805d9f20-8688-4ced-b494-5ae896cb7939",
        "type": "entity",
        "label": "Untitled document",
        "properties": {}
      },
      "target": [
        {
          "id": "urn:madoc:collection:1",
          "type": "Collection"
        },
        {
          "id": "urn:madoc:manifest:2",
          "type": "Manifest"
        },
        {
          "id": "urn:madoc:canvas:5",
          "type": "Canvas"
        }
      ],
      "profile": null,
      "derivedFrom": "f5302b73-bbcd-4453-8c1a-b3750eec6878",
      "revisions": [
        {
          "structureId": "9c2c6558-703d-4276-ac44-01c78e66ecef",
          "approved": true,
          "label": "Default",
          "id": "a14b8970-adfb-441d-8e08-5ceb12a09f0d",
          "fields": [
            "regionOfInterest"
          ],
          "status": "accepted",
          "revises": null,
          "authors": [
            "urn:madoc:user:1"
          ]
        }
      ],
      "contributors": {
        "urn:madoc:user:1": {
          "id": "urn:madoc:user:1",
          "type": "Person",
          "name": "Madoc TS"
        }
      }
    },
    {
      "id": "55702332-e035-43b1-83d9-f740b20d0c99",
      "structure": {
        "id": "b13f4147-b1a7-4aa6-8db5-e88732611f2b",
        "type": "choice",
        "label": "Multiple entities test",
        "items": [
          {
            "id": "90407ab0-208a-4c08-9a08-8af80f2b3e6f",
            "type": "model",
            "label": "Single choice",
            "fields": [
              "something",
              [
                "entity-test",
                [
                  "name",
                  "description"
                ]
              ]
            ]
          }
        ]
      },
      "document": {
        "id": "07c017ea-7172-4c5d-8763-10d8d86f22af",
        "type": "entity",
        "label": "Untitled document",
        "properties": {
          "entity-test": [
            {
              "id": "f48511c3-826a-41b5-83a0-a6f74e8a641c",
              "type": "entity",
              "label": "Single entity",
              "pluralLabel": "Multiple entities",
              "description": "A short description of the entity",
              "allowMultiple": true,
              "labelledBy": "name",
              "properties": {
                "name": [
                  {
                    "id": "9958b81c-1f1d-4e0d-b798-719e756e517b",
                    "type": "text-field",
                    "label": "Name",
                    "value": ""
                  }
                ],
                "description": [
                  {
                    "id": "88987a11-17f9-4ef3-bbc4-3510a74e1635",
                    "type": "text-field",
                    "label": "Description",
                    "value": "",
                    "minLines": 5,
                    "multiline": true
                  }
                ]
              },
              "selector": {
                "id": "a05c459c-2216-4f0d-b22e-d86b952ea6f1",
                "type": "box-selector",
                "state": null
              }
            }
          ],
          "something": [
            {
              "id": "b79aeb3e-47e2-49e4-b99a-cafb4ff98619",
              "type": "text-field",
              "label": "Something",
              "value": "",
              "description": "A top level item"
            }
          ]
        }
      },
      "target": [
        {
          "id": "urn:madoc:manifest:2",
          "type": "Manifest"
        },
        {
          "id": "urn:madoc:canvas:5",
          "type": "Canvas"
        }
      ],
      "profile": null,
      "derivedFrom": "3e5c931c-896a-424b-a3e8-173bbfd384d7",
      "contributors": {
        "urn:madoc:user:1": {
          "id": "urn:madoc:user:1",
          "type": "Person",
          "name": "Madoc TS"
        }
      }
    },
    {
      "id": "a110037c-33fd-43c0-80ad-235cd6b577d2",
      "structure": {
        "id": "940a96d2-1ed3-4941-b6ce-89050da7c94a",
        "type": "choice",
        "label": "Metadata facets",
        "items": [
          {
            "id": "2bb10ebc-acb1-4368-b53c-700675adef6f",
            "type": "model",
            "label": "Add a facet!",
            "fields": [
              [
                "metadata",
                [
                  "my-custom-facet"
                ]
              ]
            ]
          }
        ]
      },
      "document": {
        "id": "47393252-9f5a-43b0-8874-64bbfb70714c",
        "type": "entity",
        "label": "Untitled document",
        "properties": {
          "metadata": [
            {
              "id": "ea595237-810a-4041-9bee-c6170bd4b98b",
              "type": "entity",
              "label": "metadata",
              "revision": "7748400b-56de-4f64-b8eb-b92554073cc9",
              "revises": "ae60b620-8bbc-4997-b72e-c1ae3558fc18",
              "properties": {
                "my-custom-facet": [
                  {
                    "id": "d3360497-3899-444f-b737-3bed2c4209ce",
                    "type": "text-field",
                    "label": "My custom facet",
                    "value": "First in the list",
                    "revision": "7748400b-56de-4f64-b8eb-b92554073cc9"
                  }
                ]
              }
            }
          ]
        }
      },
      "target": [
        {
          "id": "urn:madoc:collection:1",
          "type": "Collection"
        },
        {
          "id": "urn:madoc:manifest:2",
          "type": "Manifest"
        },
        {
          "id": "urn:madoc:canvas:5",
          "type": "Canvas"
        }
      ],
      "profile": null,
      "derivedFrom": "b76bf9b0-f871-48cd-82de-c4c1392d5609",
      "revisions": [
        {
          "structureId": "2bb10ebc-acb1-4368-b53c-700675adef6f",
          "approved": true,
          "label": "Add a facet!",
          "id": "7748400b-56de-4f64-b8eb-b92554073cc9",
          "fields": [
            [
              "metadata",
              [
                "my-custom-facet"
              ]
            ]
          ],
          "status": "accepted",
          "revises": null,
          "authors": [
            "urn:madoc:user:1"
          ]
        }
      ],
      "contributors": {
        "urn:madoc:user:1": {
          "id": "urn:madoc:user:1",
          "type": "Person",
          "name": "Madoc TS"
        }
      }
    }
  ]
}
```

### Serialising models as JSON

From the original capture model this is the content we are interested in, the user contribution. Each model is stand-alone and resistant to change from other parts of the system. It also contains information about who created it and a who contributed to what part. We don't need all of this information in the end.

```javascript
{
  "models": [
    {
      "id": "254a73c7-2a18-4ede-b541-e6a43eecac78"
    },
    {
      "entity-test": [
        {
          "name": "",
          "description": ""
        }
      ],
      "something": "",
      "id": "55702332-e035-43b1-83d9-f740b20d0c99"
    },
    {
      "metadata": {
        "my-custom-facet": "First in the list"
      },
      "id": "a110037c-33fd-43c0-80ad-235cd6b577d2"
    }
  ]
}
```

### Open annotations

Here you can see that the annotation has been returned as an Open Annotation list. This capture model did not have a specific selector and so targets the whole canvas. If you set `selector=true` then this annotation will disappear and only annotations with selectors will be returned. Also note that the empty annotations have been filtered out – where they remain when serialised to JSON.

```javascript
{
  "@context": "http://iiif.io/api/presentation/3/context.json",
  "@id": "http://localhost:8888/s/default/madoc/api/canvases/5/models?format=open-annotation",
  "@type": "sc:AnnotationList",
  "resources": [
    {
      "@id": "http://localhost:8888/s/default/madoc/api/canvases/5/models/d3360497-3899-444f-b737-3bed2c4209ce",
      "@type": "oa:Annotation",
      "motivation": "sc:painting",
      "resource": {
        "@type": "cnt:ContentAsText",
        "chars": "First in the list"
      },
      "madoc:id": 5,
      "on": "https://view.nls.uk/iiif/7446/74464117/canvas/2"
    }
  ]
}
```

### W3C Annotations

Very similar to the above but in the W3C Annotation format.

```javascript
{
  "id": "http://localhost:8888/s/default/madoc/api/canvases/5/models?format=w3c-annotation",
  "label": null,
  "type": "AnnotationPage",
  "items": [
    {
      "id": "http://localhost:8888/s/default/madoc/api/canvases/5/models/d3360497-3899-444f-b737-3bed2c4209ce",
      "type": "Annotation",
      "motivation": "painting",
      "body": {
        "type": "TextualBody",
        "value": "First in the list"
      },
      "madoc:id": 5,
      "target": "https://view.nls.uk/iiif/7446/74464117/canvas/2"
    }
  ]
}
```

### W3C Annotation pages

This response is slightly different. It will use the metadata from the projects to pull together W3C Annotation pages that can then be dereferenced.

```javascript
{
  "pages": [
    {
      "id": "http://localhost:8888/s/default/madoc/api/canvases/5/models?format=w3c-annotation&model_id=254a73c7-2a18-4ede-b541-e6a43eecac78",
      "label": "First project",
      "type": "AnnotationPage"
    },
    {
      "id": "http://localhost:8888/s/default/madoc/api/canvases/5/models?format=w3c-annotation&model_id=55702332-e035-43b1-83d9-f740b20d0c99",
      "label": "Multiple entities test",
      "type": "AnnotationPage"
    },
    {
      "id": "http://localhost:8888/s/default/madoc/api/canvases/5/models?format=w3c-annotation&model_id=a110037c-33fd-43c0-80ad-235cd6b577d2",
      "label": "Metadata facets",
      "type": "AnnotationPage"
    }
  ]
}
```

