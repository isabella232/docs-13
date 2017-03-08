# Exporting your data or schema

To export the content or it's schema out of your GraphCMS project, you can use the exporting function within the _Settings_ page.

You can export the schema definition from your GraphCMS project by clicking the _DOWNLOAD SCHEMA_ button within the _Export_ panel.

![Screenshot](../img/guides/download_schema_and_content.png)

This will automatically download a `schema.txt` file, containing all schema information of your current content API.

An example schema is shown below. This example shows the schema for 3 content models: `Article`, `Author` and `Media`.

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

To download a dump of all your data you can use the `DOWNLOAD DATA` button. This creates a zip archive containing all the current data of your content api. Each content model is saved in a single JSON file within the zip archive.
