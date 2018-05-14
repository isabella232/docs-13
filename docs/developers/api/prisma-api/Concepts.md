# Concepts

## Data model

The API of GraphCMS is fully centered around its data model. The API is automatically generated based on the data model that you defined in your project.

Every operation exposed in the API is associated with a model or relation from your data model:

Every operation exposed in the Prisma API is associated with a model or relation from your data model:

- [Queries](./Queries)
    - query one or more nodes of a certain model
    - query nodes across relations
    - query data aggregated across relations

- [Mutations](./Mutations)
    - create, update, upsert and delete nodes of a certain model
    - create, connect, disconnect, update and upsert nodes across relations
    - batch update or delete nodes of a certain model

## Advanced API Concepts

### Node Selection

Many operations in the Prisma API only affect a subset of the existing nodes in the database, oftentimes even only a single node.

In these case, you need a way to ask for specific nodes in the API - most of the time this is done via a `where` argument.

Nodes can be selected via any field that's annotated with the `@unique` directive. That means every field that you selected as unique, as well as the ID of an entry, which is unique by default.

For the following examples, consider the following simple data model:

```json
type Post {
  id: ID! @unique
  title: String!
  published: Boolean @default(value: "false")
}
```

Here are a few scenarios where node selection is required.

**Retrieve a single node by its `email`:**
```json
query {
  post(where: {
    email: "test@graphcms.com"
  }) {
    id
  }
}
```

**Update the `title` of a single node:**
```json
mutation {
  updatePost(
    where: {
      id: "ohco0iewee6eizidohwigheif"
    }
    data: {
      title: "GraphQL is awesome"
    }
  ) {
    id
  }
}
```

**Update `published` of a many nodes at once** (also see [Batch operations](#batch-operations)):
```json
mutation {
  updatePost(
    where: {
      id_in: ["ohco0iewee6eizidohwigheif", "phah4ooqueengij0kan4sahlo", "chae8keizohmiothuewuvahpa"]
    }
    data: {
      published: true
    }
  ) {
    count
  }
}
```

## Batch Operations

One application of the node selection concept is the exposed batch operations. Batch updating or deleting is optimized for making changes to a large number of nodes. As such, these mutations only return how many nodes have been affected, rather than full information on specific nodes.

For example, the mutations `updateManyPosts` and `deleteManyPosts` provide a `where` argument to select specific nodes, and return a `count` field with the number of affected nodes (see the example above).

## Connections

In contrast to the simpler object queries that directly return a list of nodes, connection queries are based on the [Relay Connection](https://facebook.github.io/relay/graphql/connections.htm) model. In addition to pagination information, connections also offer advanced features like aggregation.

For example, while the `posts` query allows you to select specific `Post` nodes, sort them by some field and paginate over the result, the `postsConnection` query additionally allows you to *count* all unpublished posts:

```json
query {
  postsConnection {
    # `aggregate` allows to perform common aggregation operations
    aggregate {
      count
    }
    edges {
      # each `node` refers to a single `Post` element
      node {
        title
      }
    }
  }
}
```

## Transactional Mutations

Single mutations in the GraphCMS API that are not batch operations are always executed transactionally, even if they consist of many actions that potentially spread across relations. This is especially useful for [nested mutations](../Mutations#nested-mutations) that perform several database writes on multiple types.

An example is creating a `User` node and two `Post` nodes that will be connected, while also connecting the `User` node to two other, already existing `Post` nodes, all in a single mutation. If any of the mentioned actions fail (for example because of a violated `@unique` field constraint), the entire mutation is rolled back!

Mutations are transactional, meaning they are *[atomic](https://en.wikipedia.org/wiki/Atomicity_(database_systems))* and *[isolated](https://en.wikipedia.org/wiki/Isolation_(database_systems))*. This means that between two separate actions of the same nested mutation, no other mutations can alter the data. Also the result of a single action cannot be observed until the complete mutation has been processed.

## Cascading Deletes

GraphCMS supports different deletion behaviours for relations in your data model. There are two major deletion behaviours:

- `CASCADE`: When a node with a relation to one or more other nodes gets deleted, these nodes will be deleted as well.
- `SET_NULL`: When a node with a relation to one or more other nodes gets deleted, the fields referring to the deleted node are set to `null`.

Consider this example:

As mentioned above, you can specify a dedicated deletion behaviour for the related nodes. That's what the `onDelete` argument of the `@relation` directive is for.

Consider the following example:

```json
type User {
  id: ID! @unique
  comments: [Comment!]! @relation(name: "CommentAuthor", onDelete: CASCADE)
  blog: Blog @relation(name: "BlogOwner", onDelete: CASCADE)
}

type Blog {
  id: ID! @unique
  comments: [Comment!]! @relation(name: "Comments", onDelete: CASCADE)
  owner: User! @relation(name: "BlogOwner", onDelete: SET_NULL)
}

type Comment {
  id: ID! @unique
  blog: Blog! @relation(name: "Comments", onDelete: SET_NULL)
  author: User @relation(name: "CommentAuthor", onDelete: SET_NULL)
}
```

Let's investigate the deletion behaviour for the three types:

- When a `User` node gets deleted,
    - all related `Comment` nodes will be deleted.
    - the related `Blog` node will be deleted.
- When a `Blog` node gets deleted,
    - all related `Comment` nodes will be deleted.
    - the related `User` node will have its `blog` field set to `null`.
- When a `Comment` node gets deleted,
    - the related `Blog` node continues to exist and the deleted `Comment` node is removed from its `comments` list.
    - the related `User` node continues to exist and the deleted `Comment` node is removed from its `comments` list.


!!! info
    API docs are provided by GRAPHCOOL

    [![Graphcool Logo](../img/graphcool.svg)](https://graph.cool)