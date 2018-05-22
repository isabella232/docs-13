# Schemas
Schemas are the bedrock of your project. You can read more about GraphQL schemas proper [here.](https://graphql.org/learn/schema/) In GraphCMS, the Schema tab of a project window allows you to define models and enumerations.

As a developer, you have full control over the shape of these schemas. You can define who can view the content from the CMI, who can query it from the API, what fields the model supports and what options an enum contains. For more information on which default user types have access to a model by default, see the information below.

Every field you add to a model and every option you add to an enumeration will be visible to a inflection query. For more about inflection, please see ["making a call"]('./api/making-a-call.md').


## Models
<!-- TODO: Insert image of Model View -->
A model is an object of one ore more fields and have a default set of permissions.

### Fields
* Single Line Text
* Multi Line Text
* Markdownt
* Integer
* Float
* Checkbox
* Date
* DateTime
* Json
* Enumeration (Reference/Select)

### Default Permisions
| _Perm_ | Editors | Developers | Admins |
|---|---|---|---|
| Create |   |   |   |
| Read |   |   |   |
| Update |   |   |   |
| Delete |   |   |   |
<!-- TODO: Write Permissions scope for Models -->


## Enumerations
<!-- TODO: Insert image of Enumeration view. -->
An enumeration is a collection with a couple of identifying properties. A unique feature of enums is that we can make the assumptions that the value will always be ONE OF the defined options. That is, exactly one and one of the defined options.

### Values
* Display Name (String)
* API ID (Unique)
* Description (String)
* Possible Values (Array: String)


### Default Permisions
| _Perm_ | Editors | Developers | Admins |
|---|---|---|---|
| Create |   |   |   |
| Read |   |   |   |
| Update |   |   |   |
| Delete |   |   |   |
<!-- TODO: Write Permissions scope for Enums -->