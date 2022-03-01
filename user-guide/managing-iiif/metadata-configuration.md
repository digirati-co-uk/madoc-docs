# Metadata configuration

{% hint style="info" %}
Currently this configuration is only available at the site level and applied to all resources but we plan to make this configuration more granular in the future for specific projects, collections or manifests.
{% endhint %}

Under site configuration in the admin menu you can configure how your IIIF metadata values are displayed on the frontend.

By default we will display all of the metadata in the order in the underlying resource. However you can choose to customise which properties are shown and in which order by dragging the properties from the right into the middle.

![](<../../.gitbook/assets/Screenshot 2021-05-07 at 18.19.25.png>)

When you bring properties into the main section **all other metadata will be hidden**. You will need to drag in all of the metadata you want to display.

![](<../../.gitbook/assets/Screenshot 2021-05-07 at 18.20.56.png>)

From this UI you can combine multiple properties together and change their label. You can also add translations for you labels. The order that these properties are in on this page will be reflected when loading collections, manifests or canvases metadata in the frontend.
