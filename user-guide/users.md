# Users

In order to manage users you must be a global administrator. Users are managed through the [Omeka administration page](../administration-pages.md#omeka-admin-page).

## Creating a new user

From the Omeka administration interface, you can access the current list of users from the "Users" link in the menu.

![](<../.gitbook/assets/Screenshot 2020-09-02 at 21.21.21.png>)

Here you will see a list of all of the users currently added to the whole installation. You can add a user using the button in the top right corner.

![](<../.gitbook/assets/Screenshot 2020-09-02 at 21.22.04.png>)

You will be prompted to fill in some basic information for the user. You can choose to activate the user - and avoid sending an email to them. Once a user is created you can change the password to a temporary one if you want to send on details. They can then change this password later on. If you do not choose to activate the user, then they will be send an email where they can click a link and register to the site.

This is the default way of adding users in Omeka. However this can make it hard to invite users or open up registrations.

### Adding a user to a site

Once you have created a user, you need to give that user access to a give site. From the admin page, if you click on "Sites" and then choose your site from the list you will be able to see the following menu:

![](<../.gitbook/assets/Screenshot 2020-09-02 at 21.25.26.png>)

From here you can choose User permissions and be presented with a list of users that currently have access to the site on left and a list of all of the users on the right. If you search for a user and then click their name, they will be added to the main list. You can choose a role for the user for this particular site.

![](<../.gitbook/assets/Screenshot 2020-09-02 at 21.25.53.png>)

You can also click on the red icon to remove a user from a site.

{% hint style="info" %}
Due to the way that authentication is handled on Madoc a user who has had their permissions removed from a site may still be able to access the site for a few hours before their session expires. Similarly, if you add new permissions for a user, they may have to logout and log back in to see the new permissions.
{% endhint %}

## Inviting users to projects

When you choose a site in the admin, you will also see on the left side an option for invitations.

![](<../.gitbook/assets/Screenshot 2020-09-02 at 21.29.13.png>)

The invitations page will take you through the steps you need to do in order to enable invitations. This includes enabling registrations on at least one public site for the user to register.

You can also generate invitation links from this page that can be shared. This will allow users with a link to register to your site - if the registrations are not open.

## Customising roles

The roles on the site are as follows:

* **Viewer** - can view the site, anonymous users have this role on public sites
* **Transcriber** - can transcribe or contribute to models on the site
* **Limited transcriber** - can transcribe or contribute to tasks assigned to them
* **Reviewer** - can manage models and contributions, including reviewing approving and rejecting submissions.
* **Limited reviewer** - can manage contributions that have been assigned to them.&#x20;
