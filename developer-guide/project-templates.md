---
description: API definition for creating project templates
---

# Project templates

Project templates are a way to customise, extend or limit how projects are created and allow reuse of configurations for multiple projects. Eventually you will be able to create these templates from inside Madoc from existing projects.

Project templates are split into 2 sections. Static and dynamic. Static is serialisable configuration that can be represented as static JSON. Dynamic parts of the configuration are code that runs at different times in the lifecycle of a project.

To demonstrate how this configuration works, here is the "Crowdsourced transcription" template.

```typescript
import { captureModelShorthand, ProjectTemplate } from '@madoc.io/types';

type CrowdsourcedTranscriptionOptions = {
  modelLabel: string;
  modelDescription: string;
  crowdsourcingInstructions: string;
  reviewStrategy: 'one-contributor' | 'two-contributors' | 'many-contributors' | 'manual';
};

export const crowdsourcedTranscription: ProjectTemplate<CrowdsourcedTranscriptionOptions> = {
  type: '@madoc.io/crowdsourced-transcription',
  metadata: {
    label: 'Crowdsourced Transcription',
    actionLabel: 'Start transcribing',
    description: `Content added to this project will be available to browse 
    and users will be able to pick up and transcribe individual images. These
    images will be submitted for review, where you will be able to accept, reject
    or merge contributions.`,
    documentation: 'https://docs.madoc.io/incomplete-user-guide/workflows/transcribing-a-set-of-images',
    version: '1.0.0',
    thumbnail: `<svg width="109" height="109" xmlns="http://www.w3.org/2000/svg">
        <g fill="none" fill-rule="evenodd">
          <rect stroke="#E7E9EC" stroke-width="2" x="1" y="1" width="107" height="107" rx="5"/>
          <path fill="#000" d="M17 28.4h75.4v5.3H17z"/>
          <path fill="#5B78E5" d="M17 19.6h75.4v5.3H17zM17 28.4h75.4v5.3H17z"/>
          <path fill="#E7E9EC" d="M17 37.1h75.4v5.3H17z"/>
          <path fill="#5B78E5" d="M17 37.1h37.9v5.3H17z"/>
          <path fill="#E7E9EC" d="M17 46.1h75.4v5.3H17zM17 66.1h75.4v5.3H17zM17 74.8h75.4v5.3H17zM17 54.9h25.7v5.3H17zM17.9 83.8h36.6v4.3H17.9z"/>
        </g>
      </svg>
    `,
  },
  configuration: {
    defaults: {
      allowSubmissionsWhenCanvasComplete: false,
      claimGranularity: 'canvas',
      allowCanvasNavigation: true,
      allowManifestNavigation: true,
      allowCollectionNavigation: true,
      randomlyAssignCanvas: false,
      maxContributionsPerResource: 1,
      skipAutomaticOCRImport: false,
      contributionMode: 'annotation',
    },
    immutable: [
      'claimGranularity',
      'allowSubmissionsWhenCanvasComplete',
      'allowCanvasNavigation',
      'allowManifestNavigation',
      'allowCollectionNavigation',
      'randomlyAssignCanvas',
      'maxContributionsPerResource',
      'skipAutomaticOCRImport',
      'contributionMode',
      'shortExpiryTime',
      'longExpiryTime',
      'headerOptions',
      'metadataSuggestions',
    ],
    activity: {},
    frozen: false,
    status: {},
    tasks: {},
    captureModels: { preventChangeStructure: true, preventChangeDocument: true },
  },
  captureModel: {
    document: captureModelShorthand({
      transcription: {
        label: 'Transcription',
        type: 'text-field',
        minLines: 5,
        multiline: true,
        dataSources: ['plaintext-source'],
      },
    }),
  },
  setup: {
    model: {
      modelLabel: {
        label: 'Form label',
        description: 'Label for the transcription form field',
        type: 'text-field',
      },
      reviewStrategy: {
        label: 'Review strategy',
        type: 'dropdown-field',
        options: [
          { text: 'Only one contributor per image. Once reviewed, it is complete', value: 'one-contributor' },
          { text: 'Two contributors per image. Once reviewed, it is complete', value: 'two-contributors' },
          { text: 'No limit on contributors. Once one is reviewed, it is complete', value: 'many-contributors' },
          { text: 'No limit on contributors and images are manually marked as complete', value: 'manual' },
        ],
      } as DropdownFieldProps,
      modelDescription: {
        label: 'Form description',
        description: 'A longer description for your transcription form field, appears under the label (like this)',
        type: 'text-field',
      },
      crowdsourcingInstructions: {
        label: 'Crowdsourcing instructions',
        description: 'These instructions will appear to your users when working on an image.',
        type: 'text-field',
        minLines: 4,
        multiline: true,
      } as any,
    },
    defaults: {
      modelLabel: 'Transcription',
      crowdsourcingInstructions: '',
      modelDescription: '',
      reviewStrategy: 'one-contributor',
    },
    async onCreateConfiguration(config, { api, options }) {
      switch (options.reviewStrategy) {
        case 'one-contributor': {
          config.revisionApprovalsRequired = 1;
          config.maxContributionsPerResource = 1;
          break;
        }
        case 'two-contributors': {
          config.revisionApprovalsRequired = 1;
          config.maxContributionsPerResource = 2;
          break;
        }
        case 'manual': {
          config.revisionApprovalsRequired = 1000;
          config.maxContributionsPerResource = 1000;
          break;
        }
        case 'many-contributors':
        default: {
          config.revisionApprovalsRequired = 1;
          config.maxContributionsPerResource = 1000;
          break;
        }
      }
      return config;
    },
    async beforeForkDocument(doc: any, { options }) {
      if (options.modelLabel) {
        doc.properties.transcription[0].label = options.modelLabel;
      }
      if (options.crowdsourcingInstructions) {
        doc.instructions = options.crowdsourcingInstructions;
      }
      if (options.modelDescription) {
        doc.properties.transcription[0].description = options.modelDescription;
      }
      return doc;
    },
    async beforeForkStructure(fullModel: any, { options }) {
      if (options.crowdsourcingInstructions) {
        fullModel.structure.items[0].instructions = options.crowdsourcingInstructions;
        return fullModel.structure;
      }
    },
  },
};ty
```

## Static configuration

Static configuration can be stored as JSON, and not code. A mechanism will exist to import these from URLs or from disk.

### Required fields.

In order to be a valid project template the minimum fields are the following:

```javascript
{
  "type": "some-unique-type",
  "metadata": {
    "label": "My project template"
  }
}
```

Optional metadata can also be added:

```javascript
{
  "type": "some-unique-type",
  "metadata": {
    "label": "My project template",
    "summary": "A summary of this template",
    "version": "1.0.0",
    "thumbnail": "https://.../image.jpg",
    "actionLabel": "Create project with my template",
    "documentation": "https://docs.madoc.io/some-url/my-template"
  }
}
```

These fields will be used to make using your project template better for end-users. (note: plugins will soon support local assets for the thumbnail field).

### Capture model

You can configure a project template to be loaded with a default capture model (document), with an initial set of fields populated. You have additional piece of configuration if you want to further limit how much of the capture model can be changed. This can be useful if you want to export the data and have well-known fields.

* `noCaptureModel` will remove editing the model completely, allowing a potentially empty model
* `preventChangeStructure` will remove the ability to change the models structure
* `preventChangeDocument` will remove the ability to change the models document

```javascript
{
  "configuration": {
      "captureModels": {
        "noCaptureModel": false,
        "preventChangeStructure": true,
        "preventChangeDocument": true
      }
    } 
  },
  "captureModel": {
    "document": {
      "id": "f2f7f471-8436-4d54-b13a-fbd5dd1f9410",
      "type": "entity",
      "properties": {
        "transcription": [{
          "id": "830424a5-da57-4b47-8687-a08839d49604",
          "type": "text-field",
          "value": "",
          "minLines": 6
        }]
      }
    }
  }
}
```

_Note: You can only select the document, a default structure will be created for you. We may in future open up custom structures into this configuration_

### Configuration

In addition to the capture model configuration, you can also set the default site configuration and limit changes to the configuration or individual fields required to make your template work.

* `configuration.defaults` - this is a partial configuration that will be merged in with site defaults when the project is created.
* `configuration.frozen` - Setting this to true will prevent any modifications to the configuration when using your template.
* `configuration.immutable` - A list of keys in the configuration that will be removed from the project page.

```javascript
{
  "configuration": {
    "defaults": {
       "metadataSuggestions": {
          "manifest": true,
          "collection": false,
          "canvas": true
       },
      "frozen": false,
      "immutable": ["metadataSuggestions"]
    }
  }
}
```

### Statuses

Projects can have different statuses, each with a number. By default they are as follows:

* 0 - Paused
* 1  - Active
* 2 - Published
* 3 - Archived
* 4 - Being prepared

You can change the wording, colours and messaging from the project template.

* `configuration.status.disabled` - will remove the ability to see and change the status (but can be changed through API)
* `configuration.status.defaultStatus` - When a project is created, this is what the status will be
* `configuration.status.statusMap` - Customisations to the above.

```javascript
{
  "configuration": {
    "status": {
      "disabled": false,
      "defaultStatus": 0,
      "statusMap": {
        "0": {
          "label": "Not yet started",
          "action": "Pause project",
          "info": "This project has been paused",
          "color": "teal"
        }
      }
    }
  }
}
```

Note: You can specify a background colour and an accessible text colour will be chosen for you.

### Activity streams

You can disable activity streams both functionally and in the UI:

* `configuration.defaults.activity` - Disable or enable individual streams
* `configuration.activity.noActivity` - Removes the activity tab from the Admin UI.
* `configuration.immutable` - Prevents configuration from being changed.

```javascript
{

  "configuration": {
    "defaults": {
      "activityStreams": {
        "manifest": false,
        "canvas": false,
        "curated": false,
        "published": false
      }
    },
    "immutable": ["activityStreams"],
    "activity": {
      "noActivity": true
    }
  }
}
```

### Slots

There is not enough time to cover everything that can be done with slots, but in a nutshell when a project is created you can pre-load some slots and blocks that will appear only on the project. If your project template is coming from a plugin you can use this to automatically load your own custom blocks. The specification for slots and blocks will be covered elsewhere.

There are 4 contexts you can load slots into:

* **project** - blocks that will appear on the project page
* **collection** - blocks that will appear on any collections within the project
* **manifest** - blocks that will appear on any manifests within the project
* **canvas** - blocks that will appear on any canvases (and model pages) with the project

_Note: As of writing the canvas slot is not yet enabled_

The following example configuration will add a custom HTML block to each manifest in place of the breadcrumbs. It will appear within the context of a project created with this template, and the original breadcrumbs block will still be visible.

```javascript
{
  "slots": {
    "manifest": {
      "common-breadcrumbs": {
        "slotId": "common-breadcrumbs",
        "label": { "none": ["Default breadcrumbs"] },
        "layout": "none",
        "blocks": [
          {
            "name": "Display breadcrumbs",
            "type": "default.DisplayBreadcrumbs",
            "static_data": {},
            "lazy": false,
            "order": 0
          },
          {
            "lazy": false,
            "name": "Simple HTML block",
            "type": "simple-html-block",
            "static_data": {
              "html": "<p>Testing this appears</p>"
            },
            "order": 1
          }
        ]
      }
    }
  }
}
```

### Theme

If you want to extend the current theme (for a wider page for example) you can make adjustments that will be merged.

You have access to all of the same variables as regular themes. In this example we change the background of the header. These will only appear when you are inside of your project.

```json
{
  "theme": {
    "custom": {
      "header": {
        "headerBackground": "#ff00ff"
      }
    }
  }
}
```

_Note: Themes customisation is not widely available on components in Madoc yet, but header, footer and layouts should be customisable_

## Dynamic configuration

{% hint style="danger" %}
**This is still a work in progress and has not been fully implemented**
{% endhint %}

This configuration is only available to project templates as code and are not able to be serialised into JSON. There are 2 sections inside the dynamic configuration. Setup functions and hook functions.

Set up functions will be run once when the project is created and hook functions will run at different times during the lifecycle of a project.

All setup functions and hooks are async.

```javascript
const myTemplate = {
   // ...
  setup: {
    onCreateConfiguration: async (projectConfig, { options }) => {
      // ...
    },
    hooks: {
      onAddContentToProject: async () => {
        // ...
      }
    }
  }
}
```

### Setup functions

* `onCreateConfiguration` - this is called when the default configuration for the site is created. You are passed the project configuration object before it is saved. It will be a combination of the default configuration, the site configuration and any overrides you specify in your template in the static configuration. Every value should be available here so you can inspect and change any configuration.
* `beforeForkDocument` - This is called before the capture model is saved. It will contain a reference to the original capture model template so you cannot change any properties on the document passed in, but you can return a new document and this will be used instead.
* `onCreateProject` - Once the project is fully created this will be called with the project.&#x20;

### Hooks

This is a provisional list of hooks yet to be implemented. When each hook is called should be self explanatory.

* `onCreateUserRevisionSession` - This will be called when a user first lands on an annotation page. You can return any mutable object from here and it will be passed to other hooks on this page. This allows for some shared state between your hooks.
* `onAddContentToProject` - Called when a new manifest or collection is added to a project
* `onRemoveContentFromProject` - Called when a manifest or collection is removed from a project
* `canContentBeAddedToProject` - A boolean check to see if a manifest or collection can be added to a project
* `onProjectStatus` - When the project status changes
* `onLoadRevision` - When a revision / annotation is loaded
* `beforeSaveRevision` - Hook just before persisting a revision / submission from a user
* `beforeCloneModel` - Hook just before a capture model is displayed to the user after it is created. We use this to populate default fields with OCR content specific to the canvas you are viewing.
* `onSubmitRevision` - Hook just _after_ a user has saved a submission
* `onRevisionApproved` - When a user submission has been approved by a reviewer
* `onRevisionRejected` - When a user submission has been rejected by a reviewer
* `onResourceComplete` - When a canvas, manifest or collection is marked as complete (or when all sub-resources are complete)
* `onCreateReview` - When a review is created.
* `onAssignReview` - When a reviewer is assigned to a review.

## Misc

* `setupModel` - this will be a capture model that you can specify that will be shown to the user as they are creating a project. The output of these choices will be available in the setup functions.
* `components.customEditor` - Instead of a capture model, this will replace the project creation with a custom React component. The component will be passed functions for creating the project. This will enable very fine control of the project creation process.

## Project templates from content

Not covered in this document or pull request is the ability to create a project from a manifest or a collection. This will make it much easier to get started with projects quickly. Ideally when you are on a manifest and you choose "Create transcription project" or "Crowdsource OCR corrections" from a manifest or collection page it will give the user the option to either create a fresh new project or add it to an existing project of the same type (i.e. same project template).
