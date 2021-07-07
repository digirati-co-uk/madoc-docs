# Technical FAQ

### Contributions

#### What happens if a user submits the same model twice?

It depends on a few factors. 

1. Do the fields being targeted in the [structure](capture-models/structure.md) allow multiple instances? 
2. Is the project configured to allow multiple contributions?
3. Has the user submitted in transcriber mode?

**1.** This is the most complicated. If you were running a transcription project with a single field for that transcription in your document \(allowMultiple = false\) then if a user did submit twice they would be submitting 2 different replacement transcriptions. They would appear in the review interface as 2 submissions. However the same [review process](reviews/merging-submissions.md) applies and only one will be canonical. However if you were running a project and the model did allow multiple submissions - like identifying people - those could both be approved and all of the people would be accepted. For more complex structures with mixes of these types of field it depends on the [model root](technical-faq.md#model-root) and the existing data. 

**2.** If a project is configured to allow multiple contributions then users will be allowed to submit more than one while the resource is not yet marked as complete. There is a separate configuration option to allow submissions _after_ a resource is marked as complete.

**3.** If the project is in transcriber mode they should not be able to submit more than once, they should always see their submissions clearly.

#### What does "randomly assigned" mean?

There are a few cases where canvases or manifests may be randomly assigned or navigated to. However the process for choosing is not fully random. A resource will be chosen when it matches the following:

* Has this resource been completed? **No**
* Has this resource reached the maximum number of contributors? **No**
* Am I assigned to the resource? **No**

There are 2 more edge cases where the random process is less random. 

**1. The project is set with a claim granularity of manifest**  
In this case it would not be ideal for users to be taken to a random canvas half way through a manifest if users are expected to work through all of the manifests. Instead they will go to the first canvas in a random manifest.

**2. The project is configured with "**_**Prioritise \[x\] sequentially**_**"**  
Sometimes you want to work through a project roughly from start to finish but still spread users out to different resources. When this configuration is set, users will be given a random resource but much more likely to be from the next 5-10 resources. 

When selecting a random resource a set of available resources matching all of the above conditions is pooled together in the order they appear and one resource is randomly chosen. 

#### When is a canvas or manifest marked as complete?

{% hint style="warning" %}
This refers to marking as complete in relation to a project as a whole. Users may see manifests that they have completed showing as complete, like in transcription mode.
{% endhint %}

A canvas is marked as complete during the review process. When a crowdsourcing task is marked as complete we check if the number of accepted contributions has reached the configuration value "_Submission approvals required_". This value is set when a crowdsourcing task is [prepared](technical-faq.md#prepare-step) and can be changed on a per-resource bases on the canvas page in the details tab. If this is set to a very high number, the canvas can be manually marked as complete and prevent further submissions \(if that configuration value is set as such\) or simply indicate to other contributors that the resource is done.

Once all of the canvases in a manifest have been marked as complete, the manifest itself is marked as complete.

#### What determines wether or not a user can contribute on a resource?

In a project we build up a [structure of tasks](projects/visualising-project-progress.md) that mirrors the content in the project. This is created as user go to pages to contribute and not all at once. When one of these structural items is created we look at the project configuration and store a "maximum number of contributions" specifically for that manifest or canvas. This is the "[prepare contribution](technical-faq.md#prepare-step)" step.

We then count how many unique users are contributing to that resource. We compare that with the maximum value and prevent further submissions from users. When the user first saves a contribution to a resource, a task is created assigned to them. This is the "[create claim](technical-faq.md#claim-and-update-claim-step)" step. When a user submits more, or submit for review the task will be updated. This is the "[update claim](technical-faq.md#claim-and-update-claim-step)" step. 

#### Can I assign certain users to a project?

The short answer is no. Originally there was not a concept of multiple projects running on a site, instead only different sites. However this caused friction when creating new projects, with administrators required to create a new site and configure it in the same way as previous sites, add content and then link the two sites together, and bring users in. 

We decided to allow a single site with a user-base to run multiple projects. If you are looking to run a project for a different set of users you can create a new site and invite the users you want to it.

#### What happens if I prevent users from navigating to Manifest or Canvases in the configuration?

{% hint style="danger" %}
This feature is not compatible with many configuration values and **should be avoided** unless you are familiar with related configuration.
{% endhint %}

If you prevent users from navigating to canvases then users will be able to browse manifests in your project but not be able to browse the images inside. With only this configuration chosen users will not be able to contribute to your project. You will have to assign resources manually. Administrators and reviewers will still be able to browse the images.

![This is displayed to users in place of the manifest images](../.gitbook/assets/screenshot-2021-05-07-at-17.39.17.png)

If you also choose "Randomly assign canvas to user" then in addition to not being able to browse a manifests images, they will be able to be randomly assigned an image.

![This is displayed in place of the manifest images](../.gitbook/assets/screenshot-2021-05-07-at-17.41.10.png)

If you prevent users from navigating to manifests then they will only be able to work on resources that have been assigned to them.

#### How and when is the reviewer of a resource determined?

The process is described [here](reviews/creating-reviews.md#assignment-of-reviews). It is still a work in progress and may be improved or changed. Feedback is appreciated!

### Projects

#### I changed configuration of my project and it's not applied

Some configuration, such as warning time or maximum contributors on a resource are applied to resources during the [prepare step](technical-faq.md#prepare-step) and can be changed on a per-resource basis if required. This is by design to make projects more flexible as they progress. The trade off is this unexpected inconsistency when configuration is changed during a project. _In almost all cases project configuration should be set before starting._ If this becomes more of a problem we may consider developing tools to re-prepare items in a project.

#### I changed my capture model and some pages are still showing the old capture model

Capture models are created during the [prepare step](technical-faq.md#prepare-step) and once created are completely separate documents. As described in the [capture models](capture-models/#background) section, they can be thought of as a photocopy of the capture model in the project. This is by design to ensure they can be preserved and continue to work once created. Similar to configuration it is recommended that capture models are planned and created before a project runs. If this becomes more of a problem we may consider developing tools to re-prepare capture models to add missing fields.

#### I change my capture model and some pages are showing a blank screen

All capture models in a project point to the same [structure](capture-models/structure.md) but have different documents. This allows you to run a project in stages. You may have an initial batch of text-only items and then a second batch with illustrations. So long as your document has all of the fields for both text-only items and illustrations you can change the structure during the project. 

However - if you add a new field to the document and change your structure to point to that new field, any images that have been [prepared](technical-faq.md#prepare-step) will not have that field in their document but the structure will be pointing to it. 

This is a case that you should try to avoid where possible.

#### I changed the structure in my capture model but the changes are not saved

Change the label from "Default" to anything else and it will work. To make models easier to create we will automatically add all fields to the default structure until you change the label or add a choice. 

### Omeka

#### How do I enable registrations on a site?

In the Omeka administration panel you need to:

1. Go to sites
2. Choose your site
3. Click on "_Settings_" in the left bar under your site
4. Ensure "_Enable registrations_" is set under "_Public user module_"
5. Ensure "_Show user bar on public views_" is set to "_Always_"

{% hint style="info" %}
It is recommended to create [invitations](users.md#inviting-users-to-projects) for new users.
{% endhint %}

#### Error when creating or registering users or forgot password is not working

If you experience an error when creating users you need to configure Omeka with an SMTP server. Configuration can be found [here](https://omeka.org/s/docs/user-manual/configuration/#mail) which in turn is driven by the following environment variables in the madoc \(not to be confused with [madoc-ts](technical-faq.md#madoc-vs-madoc-ts)\) 

| Environment variable | description |
| :--- | :--- |
| `OMEKA__SMTP_HOST` | Host for SMTP server |
| `OMEKA__SMTP_SECURITY` | Security for SMTP \(`tls`\) |
| `OMEKA__SMTP_USERNAME` | Username to authenticate with SMTP server |
| `OMEKA__SMTP_PASSWORD` | Password to authenticate with SMTP server |

Additional configuration for Omeka can be found at  `/srv/omeka/application/config/`  in the docker container. These can be volumed in if required.

### Appendix

#### Madoc vs Madoc TS

Originally Madoc was built using PHP in the form of Omeka-S modules. Since 2019 however the frontend of Madoc has transitioned to use server-side rendered React + Typescript \(Madoc TS\). This focus on a Typescript frontend has accelerated the development of custom administrator interfaces for managing complex data  such as capture models and IIIF. Most development is done in Madoc TS and has grown to replace some of the functionality previous provided by Omeka. Today, Omeka provides users and sites with their associated roles. To reduce complexity, this might be simplified in the future.

#### **Model root**

The model root is an option in a capture model structure that drives at what level in a nested structure a revision should copy. If you have a deeply nested model `page -> line -> word` . It's reasonable to allow users to add new paragraphs or lines. However in the structure we just list which fields we want to edit.

{% tabs %}
{% tab title="Model root = line" %}
```text
page -> line -> word
             \ 
              -> new word
```
{% endtab %}

{% tab title="Model root = page" %}
```
page -> line -> word
    \ 
     -> line 2 -> new word
```
{% endtab %}
{% endtabs %}

This additional configuration reduced the amount of options that the transcription UI needs to present to the user. By default if there is a structure like this, they will be able to navigate through and choose to add new lines or words. 

Since this problem is a complex UI and UX issue for displaying editing UIs it is not available in the capture model editor and is not used by Madoc in most cases. However these complex cases _have_ been considered in the protocol and underlying data model.

#### **Prepare step**

![This is what happens when a resource is prepared for contribution](../.gitbook/assets/image%20%286%29.png)

#### **Claim and update claim step**

![This is what happens when a user creates or updates a submission](../.gitbook/assets/image%20%285%29.png)

These diagrams show the process for when a user _can_ contribute. During the create and update step we further verify that the resource can be contributed to. 

