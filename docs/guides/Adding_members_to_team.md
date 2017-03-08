# Adding members to your team

You can add other GraphCMS users as team members to your projects. This can be done from the _Team_ panel in the _Settings_ view, which also shows all members of your project.

![Screenshot](../img/guides/addTeamMembers.png)

By default, this table is empty and only the project owner is displayed in the header of the panel.
You can invite other GraphCMS users to your project and assign them a role. Currently, there are four predefined roles:

* Admin
* Developer
* Editor
* Contributor

Depending on the role, the user has access to different parts of the CMS. The table below shows all the actions which depend on the users role within a project.

|Action | OWNER | ADMIN | DEVELOPER | EDITOR | CONTRIBUTOR |
| -------- | ------- | ------------- | ------- | -------- |
| Write Content | ✔ | ✔  | ✔ | ✔ | ✔ | ✔ |
| Edit Content Model | ✔ | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 |
| Configure Webhooks | ✔ | ✔  | ✔ | 𐄂 | 𐄂 | 𐄂 |
| Manage Team Members | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 | 𐄂 |
| Change Project Config | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 | 𐄂 |
| Manage Auth Tokens | ✔ | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 |
| Change API Access Settings | ✔ | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 |
| Export Schema and Data | ✔ | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 |
| Delete Project Data | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 | 𐄂 |
| Delete Project | ✔ | 𐄂 | 𐄂 | 𐄂 | 𐄂 | 𐄂 |
