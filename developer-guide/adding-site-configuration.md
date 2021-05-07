# Adding new project config

Add the desired type to the Type definition and add your type:

```diff
// services/madoc-ts/src/types/project-configuration.ts

export type ProjectConfiguration = {
+ defaultEditorOrientation: 'vertical' | 'horizontal';
  allowCollectionNavigation: boolean;
  allowManifestNavigation: boolean;
  allowCanvasNavigation: boolean;
};
```

Add a default value in the base madoc configuration. This will be applied to sites that may not have saved your new configuration value.

```text
// services/madoc-ts/config.json

{
  "cookieName": "madoc",
  "tokenExpires": 3600,
  "defaultSiteConfiguration": {
+   "defaultEditorOrientation": "horizontal",
    "allowCollectionNavigation": true,
    "allowManifestNavigation": true,
    "allowCanvasNavigation": true,
...
```

Add the configuration to the capture model for editing config. You can see examples of the various form types in this file.

```diff
     type: 'checkbox-field',
     inlineLabel: 'Skip automatically processing OCR during IIIF manifest import',
   },
+  defaultEditorOrientation: {
+    label: 'Default editor orientation',
+    description:
+      'When a user makes a contribution they will see the form either to the right of (horizontal) or below the image (vertical). The user can still change this if they want.',
+    type: 'dropdown-field',
+    options: [
+      { value: 'vertical', text: 'Vertical (to the right)' },
+      { value: 'horizontal', text: 'Horizontal (under) ' },
+    ],
+  },
 };
```

Now you can use your configuration value. If you want to use it in a React component you can use the following hook:

```typescript
import { useSiteConfiguration } from '../features/SiteConfigurationContext';

const MyComponent: React.FC = () => {
  const config = useConfiguration();

  const ourValue = config.project.defaultEditorOrientation; // we just added.

  // ...
}
```

The configuration for the whole site is available to any component. Depending on where you component is rendered you may be given overrides for that particular context \(for example: projects\)

