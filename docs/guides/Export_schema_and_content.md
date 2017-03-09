# Export your Schema and Content

To export the GraphQL schema or the content of your GraphCMS project, you can use the export function from the _Settings_ page.

![Screenshot](../img/guides/download_schema_and_content.png)

This will automatically download a `schema.txt` file, containing all schema information of your current content API.

An example schema is shown below. It shows the schema for three content models: `Article`, `Author` and `Media`.

Media is a system model created by GraphCMS.
The content model `Article` has one custom field `title` of type `String`. The content model `Author` has also one custom field with name `name` and type `String`.
Both models are connected via a one-to-many relation with name `AuthorArticles`.

```
type Article {
  author: Author @relation(name: "AuthorArticles")
  createdAt: DateTime!
  id: ID!
  title: String
  updatedAt: DateTime!
}

type Author {
  articles: [Article!]! @relation(name: "AuthorArticles")
  createdAt: DateTime!
  id: ID!
  name: String
  updatedAt: DateTime!
}

type Media {
  createdAt: DateTime!
  fileName: String!
  handle: String!
  id: ID!
  isPublic: Boolean
  mimeType: String
  size: Int!
  updatedAt: DateTime!
  url: String!
}
```

To download your complete content, use the `DOWNLOAD DATA` button. This creates a zip archive containing all the stored data of your project. The data of each content model is stored in a separate JSON file..
