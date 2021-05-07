# IIIF Collections

Starting from the [Madoc admin page](../../administration-pages.md#madoc-admin-page) in the first column on the left you will see "Manage collections". The rest if this guide will start from this page.

### Creating a new collection

Before you have added any content you may see a single button on the manage collections page: "Import Collection". If you click this button you will be taken to the collection creation page. You have two choices when creating a collection, you can create a brand new collection that only exists in Madoc and add manifests or collections to it yourself, or you can import a IIIF collection from an endpoint. 

To start with, we will create a collection from scratch. 

![](../../.gitbook/assets/screenshot-2021-05-06-at-17.17.17.png)

To get started you only need to give your collection a name. This will be used as the "label" for the IIIF collection. You can also change the language of the label you choose.

Once you have created your collection you will be redirected to a page for that collection.

![](../../.gitbook/assets/screenshot-2021-05-06-at-17.17.55.png)

From here you can start to add or edit the properties of the collection.

### Importing an existing collection

If you have an existing IIIF collection available at a URL you can import it into Madoc. From the "Import collection" page mentioned above, on the right hand side you will see a box where you can paste in the link to your IIIF collection.

![](../../.gitbook/assets/screenshot-2021-05-06-at-17.18.10.png)

When you paste in your URL, it will be fetched by the browser and show a preview of the contents of your collection.

![](../../.gitbook/assets/screenshot-2020-09-02-at-20.37.51.png)

You can choose to preview individual manifests to make sure that the collection will import correctly. The code that is being used to generate the thumbnails here is the same that is used during the import. This can be useful to catch any issues before importing.

When you are ready to import, click on "Import collection and **X** manifests". As collections can be very large in size, the task is broken down into chunks and each element is imported individually. You will be able to track the progress of this import on the following screen after clicking the button.

![](../../.gitbook/assets/screenshot-2020-09-02-at-20.41.30.png)

You can click into individual manifests to see them being processed. If you see there is an issue at the end of the import you can click the "Retry" button on the right hand side. Most collections shouldn't take too long to process and import. 

{% hint style="info" %}
If you have imported a collection before, this will not create 2 of the same collection, it will however go back through and make sure everything imports correctly. Similarly, if you run multiple sites and import the same content into those sites they will be efficiently stored once and shared by both sites.
{% endhint %}

Once the import has finished you will see the following screen, and you can now click on "view collection" to be taken directly the collection page.

![](../../.gitbook/assets/screenshot-2020-09-02-at-20.44.02.png)

### Adding labels and metadata

Now that you have a collection, you can start to add or edit the metadata. This can be useful if you plan to show the collection on the frontend site. From the main collection page you will see a variety of tabs under the title. Clicking on "Edit metadata" will take you the metadata editor.

![](../../.gitbook/assets/screenshot-2020-09-02-at-20.44.54.png)

From this page you can add a label and a summary to your collection, including translations for different languages. If the imported collection had key/value metadata pairs, these can also be found here.

![](../../.gitbook/assets/screenshot-2020-09-02-at-20.46.10.png)

{% hint style="info" %}
It is not currently possible to start a new metadata key-value list for collections.
{% endhint %}

### Managing and adding items to your collection

From any collection you can click on the "Edit structure" tab to see all of the direct manifests and collections that are contained in this collection. 

![](../../.gitbook/assets/screenshot-2020-09-02-at-20.48.23.png)

There are two search boxes where you can search for existing manifests or collections and add them to this collection. You can also remove an item using the button the right or reorder the collection by dragging on the handle icon on the left.

### Seeing which projects your collection is in

Once you create a [project](../projects/) if this collection is a part of any projects the project will be available from the projects tab on the collection page.

### Deleting a collection

The final tab on the collection page is for removing a collection. If you click on this tab you will be asked to confirm before the collection is removed. **Note: removing a collection does not remove any of the manifests contained in the collection.**

