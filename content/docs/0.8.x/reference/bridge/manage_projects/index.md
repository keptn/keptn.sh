---
title: Manage Projects
description: Create, update and deletes projects from the Bridge
weight: 21
keywords: [0.8.x-bridge]
---

The Bridge provides features to make managing projects more convenient. It is possible to create and update projects directly from the UI.
The following sections explain in detail which functionalities you can use to set up your projects and where to find them.

This may also be interesting for you:

* [Create a project with the CLI](https://keptn.sh/docs/0.8.x/reference/cli/commands/keptn_create_project/)
* [Update a project with the CLI](https://keptn.sh/docs/0.8.x/reference/cli/commands/keptn_update_project/)

## Create a new project in Bridge

Going to the project overview, there is a "Create a new project" button at the bottom of the page.
Clicking on this button redirects you to a new page with a form that provides all the necessary information needed for creating a new project in Keptn.

{{< popup_image
link="./assets/project-dashboard-empty.png"
caption="Empty project page">}}

{{< popup_image
link="./assets/create-new-project.png"
caption="Create project page">}}

{{< popup_image
link="./assets/create-new-project-filled-in.png"
caption="Create project page filled in with examples">}}

This information needs to be provided:

#### Project name
The project name has to be unique in the Keptn installation. Therefore, it must not be a name that is already in use for another project.

#### Git repository settings
The Git upstream can be set by providing the Git repository URL, Git username and the Git token. This is parallel to the Git ``--git-user=GIT_USER --git-token=GIT_TOKEN --git-remote-url=GIT_REMOTE_URL`` parameters when using the Keptn CLI.
More information about how you can set up your git provider can be found in the [Git-based upstream documentation](https://keptn.sh/docs/0.8.x/manage/git_upstream/).

#### Shipyard file
Please provide the `shipyard.yaml` file here for your project. You can either drag it into the panel or select it manually from your file system with the "Select a file" button.<br/>
You can find more information about the shipyard file on the [Shipyard documentation page](https://keptn.sh/docs/0.8.x/manage/shipyard/).

After the successful creation of the project, you are redirected directly to the new project's settings page.
Your project should already be available in the project overview and in the navigation.

{{< popup_image
link="./assets/create-new-project-success.png"
caption="Successful creation of a project">}}

{{< popup_image
link="./assets/project-dashboard.png"
caption="Project overview">}}

## Update your project
On the project settings page, you can update your project.

{{< popup_image
link="./assets/settings.png"
caption="Project settings">}}

## Delete your project
On the project settings page, you can delete your project. Please be careful when using this feature, as the project will be removed 
permanently from your Keptn installation and cannot be restored.

{{< popup_image
link="./assets/delete-project.png"
caption="Delete project">}}

### Update the Git upstream settings
The git upstream can be updated by providing the Git repository URL, Git username and the Git token. This can also be achieved by
using the [keptn update project](https://keptn.sh/docs/0.8.x/reference/cli/commands/keptn_update_project/) command.
More information about how you can set up your Git provider can be found in the [Git-based upstream documentation](https://keptn.sh/docs/0.8.x/manage/git_upstream/).
