# Adding Members to your Team

You can add other GraphCMS users as team members to your projects. This can be done from the _Team_ panel in the _Settings_ view, which also shows all members of your project.

![Screenshot](../img/guides/addTeamMembers.png)

By default, this table is empty and only the project owner is displayed in the header of the panel. If you want to change the owner, simply contact us through our On-Site-Chat.
You can invite other GraphCMS users to your project and assign them a role. Currently, there are four predefined roles:

* Admin
* Developer
* Editor
* Contributor

Depending on the role, the user has access to different parts of the CMS. The table below shows all the actions which depend on the users role within a project.

|Action | OWNER | ADMIN | DEVELOPER | EDITOR | CONTRIBUTOR |
| -------- | ------- | ------------- | ------- | -------- | --- |
| Write Content | ✔ | ✔  | ✔ | ✔ | ✔ |
| Edit Content | ✔ | ✔  | ✔ | ✔ | only unpublished |
| Publish / Unpublish Content | ✔ | ✔  | ✔ | ✔ | 𐄂 |
| Edit Content Model | ✔ | ✔ | ✔ | 𐄂 | 𐄂 |
| Configure Webhooks | ✔ | ✔  | ✔ | 𐄂 | 𐄂 |
| Manage Auth Tokens | ✔ | ✔ | ✔ | 𐄂 | 𐄂 |
| Manage API Access | ✔ | ✔ | ✔ | 𐄂 | 𐄂 |
| Export Data | ✔ | ✔ | ✔ | 𐄂 | 𐄂 |
| Delete Project Data | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 |
| Manage Team | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 |
| Change Project Config | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 |
| Delete Project | ✔ | 𐄂 | 𐄂 | 𐄂 | 𐄂 |

You can search for users by their email address. Click on the `plus icon` next to the role name and type in the email address.
If there are multiple users for an email address, e.g. if they have accounts with different social providers, you will see a list of search results. This will allow you to pick the right account. By clicking the `ADD` button, the user will be added to the project and notified via email.

Each user can be assigned to one role at a time. If you want to change the role, simply remove and reassign the user.

To remove yourself from a project, use the `LEAVE PROJECT` button in the danger zone.

!!! hint
    Currently it is not possible to invite users without a GraphCMS account. What works already is to first create a GraphCMS account using the sign up form on app.graphcms.com and invite the user afterwards. An invite feature for inviting external users will be available soon.
