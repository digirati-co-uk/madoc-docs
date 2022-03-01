# Themes

Madoc support creating custom themes on Disk. You can also include themes in a plugin ([example](https://github.com/stephenwf/madoc-example-plugin/blob/main/src/plugin.ts#L35))



The folder structure of a theme is as follows:

```
my-custom-theme
├── fragments
│   ├── footer.html
│   └── header.html
├── public
│   ├── script.js
│   └── style.css
└── theme.json
```



#### Theme definition file

The `theme.json` file contains basic metadata and variables for your theme. The definition for themes is available as a [JSON-Schema](https://raw.githubusercontent.com/digirati-co-uk/madoc-platform/main/services/madoc-ts/schemas/BaseTheme.json).

**Simple example**

```json
{
  "$schema": "https://raw.githubusercontent.com/digirati-co-uk/madoc-platform/main/services/madoc-ts/schemas/BaseTheme.json",
  "name": "Default Madoc Theme",
  "version": "1.0.0",
  "description": "Example madoc theme with default theme values.",
  "thumbnail": "https://user-images.githubusercontent.com/8266711/118507448-25750b80-b726-11eb-9f87-d96f43c0a894.png",
  "theme": {
    "header": "default",
    "accent": "default",
    "siteContainer": "light",
    "footer": "default",
    "global": "default",
    "custom": {}
  }
}
```

If you are using an editor, the JSON schema should help provide completions for creating a custom theme. The easiest way to configure themes is to choose presets created internally. You can also use the `custom` key to specify overrides for variables.

### Header

The header themes the background and text for the main site navigation and search box.

| Preset name |                                                                 |
| ----------- | --------------------------------------------------------------- |
| `default`   | ![](<../.gitbook/assets/Screenshot 2022-02-28 at 16.16.54.png>) |
| `dark`      | ![](<../.gitbook/assets/Screenshot 2022-02-28 at 16.12.36.png>) |
| `midnight`  | ![](<../.gitbook/assets/Screenshot 2022-02-28 at 16.17.21.png>) |

Custom options

```json
{
  "theme": {
    "custom": {
      "header": {
        "globalBackground": "blue",
        "headerBackground": "red",
        "headerText": "teal",
        "menuHoverBackground": "orange",
        "searchBorder": "3px solid #000",
        "searchBorderFocusColor": "limegreen"
      }
    }
  }
}
```

![](<../.gitbook/assets/Screenshot 2022-02-28 at 16.27.09.png>)

### Site container

The site container theme controls the background of the main content and the background of the page container.

| Preset name |                                                                 |
| ----------- | --------------------------------------------------------------- |
| `default`   | ![](<../.gitbook/assets/Screenshot 2022-02-28 at 16.29.05.png>) |
| `light`     | ![](<../.gitbook/assets/Screenshot 2022-02-28 at 16.27.40.png>) |
| `dark`      | ![](<../.gitbook/assets/Screenshot 2022-02-28 at 16.30.10.png>) |

Custom options

```json
{
  "theme": {
    "custom": {
      "siteContainer": {
        "background": "red",
        "containerBackground": "blue"
      }
    }
  }
}
```

![](<../.gitbook/assets/Screenshot 2022-02-28 at 16.32.02.png>)

### Footer

|         |                                                                 |
| ------- | --------------------------------------------------------------- |
| `light` | ![](<../.gitbook/assets/Screenshot 2022-02-28 at 16.33.10.png>) |
| `dark`  | ![](<../.gitbook/assets/Screenshot 2022-02-28 at 16.33.36.png>) |

Custom options

```json5
{
  "theme": {
    "background": "red",
    "color": "blue",
    "containerBackground": "green"
  }
}
```

![](<../.gitbook/assets/Screenshot 2022-02-28 at 16.34.46.png>)

### Global

Only one preset `default` but with the following custom options

```json
 {
   "theme": {
     "global": {
        "maxWidth": "1880px"
     }
   }
 }
```

`1880px` is the default max page width defined in the theme, but this can be changed.
