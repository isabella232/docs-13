# Adding people to your project

You can add other GraphCMS users as team members to your projects. This can be done in the _Settings_ view. There you find a panel _Team_ which shows all current members of your project.

![Screenshot](../img/guides/addTeamMembers.png)

By default this table is empty and only the project owner is displayed above.
To add other members you first have to choose their role within your project. Currently there are four predefined roles for a user:

* Admin
* Developer
* Editor
* Collaborator

Depending on the role, the user has access to different parts of GraphCMS. The table below shows all the actions which depend on the users role within a project.

|Action | OWNER | ADMIN | DEVELOPER | EDITOR | CONTRIBUTOR |
| -------- | ------- | ------------- | ------- | -------- |
| webhooks | ✔ | ✔  | ✔ | 𐄂 | 𐄂 |
| delete project | ✔ | 𐄂 | 𐄂 | 𐄂 | 𐄂 |
| delete project data | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 |
| Manage members | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 |
| change Project Config | ✔ | ✔ | 𐄂 | 𐄂 | 𐄂 |
| manage AuthTokens | ✔ | ✔ | ✔ | 𐄂 | 𐄂 |
| CRUD Model | ✔ | ✔ | ✔ | 𐄂 | 𐄂 |
| change API settings | ✔ | ✔ | ✔ | 𐄂 | 𐄂 |
| export data | ✔ | ✔ | ✔ | 𐄂 | 𐄂 |
