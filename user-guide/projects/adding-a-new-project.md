# Adding a new project

You can navigate to your projects from the left hand navigation in the admin.

![](<../../.gitbook/assets/Screenshot 2022-03-01 at 10.22.21.png>)

On the main projects page you will see a list of all of the projects that are running on your site. Projects are listed in a paginated list and you can page through if you have more. To create a new project click on the button at the top.

![](<../../.gitbook/assets/Screenshot 2022-03-01 at 10.23.28.png>)

When you create a project you can choose a project template, or create a custom project. Each of the project templates should have their own documentation linked.

![](<../../.gitbook/assets/Screenshot 2022-03-01 at 10.24.18.png>)

You will be guided through a form to fill in some information about your project. You will be prompted to add a label, description and a slug for your project. The slug will be used in the URL when navigating the frontend site. You can change these labels later, you cannot easily change the "slug" after this point.

![](<../../.gitbook/assets/Screenshot 2021-05-06 at 17.22.08.png>)

Once you created your project you will be taken to the main project landing page. From here you can edit the metadata of the project, manage its content add a model and visualise the progress of the project. These will be covered in the following chapters.

## Technical details

Under the hood, the metadata for a project (title and description) are modelled as a IIIF collection. All of the content in a project is stored in this collection and is accessible via the same APIs as normal collections. The one difference is that it is a flat collection. When you add a collection to a project, all of the manifests inside of that collection will be extracted and also added to the project. This creates a flat structure where all of the manifests and collections can be seen directly under the project.
