# Relay API

The Relay API is meant to be used with the GraphQL client [Facebook's Relay](https://facebook.github.io/relay/). If you don't use Relay it might be simpler to start with the Simple API.
Of course it is up to you which one you prefer. You might also use both APIs inside your app if you want to.

## Querying the API
Using _queries_ you can ask for data you are interested in. Within each query you define a set of fields, which should be returned within the response.
All queries are generated for you and can have different arguments which can be passed into the query.

### Query a single entry
For each model in your project, the Relay API provides an automatically generated query to fetch one specific entry of that model. To specify the entry, all you need to provide is its _ID_ or another _unique_ field.

For example if you wan't to get the `name` and the `createdAt` field from the content model `Artist`, the following request can be used:

```
{
  viewer {
    Artist(id: "cixnen2vv33lo0143bdwvr52n") {
      name
      createdAt
    }
  }
}
```

Or, if the Artist model has a unique field `slug`:

```
{
  viewer {
    Artist(slug: "my-awesome-artist") {
      name
      createdAt
    }
  }
}
```

!!! hint ""
    Note: You cannot specify two or more unique arguments for one query at the same time.

### Query multiple entries
The Simple API contains automatically generated queries to fetch all entries of a certain model. For example, for the Artist model the query allArtists will be generated.

A few examples for query names
- model name: Artist, query name: allArtists
- model name: Track, query name: allTracks
- model name: Review, query name: allReviews.

A query which fetches all entries from the `Artist` content model could look like the following:
```
{
  viewer {
    allArtists {
      edges {
        node {
          id
          name
        }
      }
    }
  }
}
```

#### Ordering entries
When querying all entries of a model you can supply the orderBy argument for every scalar field of the model:
`orderBy: <field>_ASC` or `orderBy: <field>_DESC.`

Example:
```
{
  viewer {
    allArtists(orderBy: name_ASC) {
      edges {
        node {
          id
          name
        }
      }
    }
  }
}
```

#### Filtering entries
When querying all entries of a model you can supply different parameters to the filter argument to filter the query response accordingly. The available options depend on the scalar fields defined on the model in question.

If you supply exactly one parameter to the filter argument, the query response will only contain entries that fulfill this constraint:

```
{
  viewer {
    allArtists(filter: {published: false}) {
      id
      name
      published
    }
  }
}
```

Depending on the type of the field you want to filter by, you have access to different advanced criteria you can use to filter your query response:

```
{
  viewer {
    allArtists(filter: {name_in: ["Velvet Parker", "Cat Stevie"]}) {
      edges {
        node {
          id
          name
          bio
        }
      }
    }
  }
}
```

For to-one relations, you can define conditions on the related entry by nesting the according argument in filter:
```
{
  viewer {
    allRecords(filter: {artist: {name: "Cat-Stevie"}}) {
      edges {
        node {
          id
          slug
        }
      }
    }
  }
}
```

For to-many relations, three additional arguments are available: `every`, `some` and `none`, to define that a condition should match every, some or none related entries.
```
{
  viewer {
    allArtists(filter: {records_every: {slug: "All-your-Base"}}) {
      edges {
        node {
          id
          name
        }
      }
    }
  }
}
```

You can use the filter combinators `OR` and `AND` to create an arbitrary logical combination of filter conditions.
```
{
  viewer {
    allRecords(filter: {AND: [
      {artist: {name: "Cat-Stevie"}},
      {cover: {isPublic: true}}
    ]}) {
      edges {
        node {
          id
          slug
          cover {
            url
          }
        }
      }
    }
  }
}
```

You can combine and even nest the filter combinators `AND` and `OR` to create arbitrary logical combinations of filter conditions:
```
{
  viewer {
    allRecords(filter: {OR: [
      {AND: [{artist: {name: "Cat-Stevie"}}, {cover: {isPublic: true}}]},
      {OR: [{artist: {name_not: "Cat-Stevie"}}, {cover: {isPublic: false}}]}
    ]}) {
      edges {
        node {
          id
          slug
          cover {
            url
          }
        }
      }
    }
  }
}
```

#### Pagination
When querying all entries of a specific model you can supply arguments that allow you to paginate the query response.
Pagination allows you to request a certain amount of entries at the same time. You can seek forwards or backwards through the entries and supply an optional starting entry:

- to seek forwards, use `first`; specify a starting entry with after.
- to seek backwards, use `last`; specify a starting entry with before.

You can also skip an arbitrary amount of entries in whichever direction you are seeking by supplying the skip argument:
```
{
  viewer {
    allArtists(first: 5) {
      edges {
        node {
          id
          name
        }
      }
    }
  }
}
```

To query the first two artists after the artist with id `cixnen2ssewlo0143bexdd52n`:
```
{
  viewer {
    allArtists(first: 2, after: "cixnen2ssewlo0143bexdd52n") {
      edges {
        node {
          id
          name
        }
      }
    }
  }
}
```

To query the last 5 artists use `last`:
```
query {
  allArtists(last: 5) {
    edges {
      node {
        id
        name
      }
    }
  }
}
```

!!! hint ""
    Note: You cannot combine first with before or last with after. Note: If you query more entries than exist, your response will simply contain all entries that actually do exist in that direction.


## Generated mutations

With a mutation you can modify the data of your project. Similar to queries, all mutations are automatically generated. Explore them by using the API EXPLORER inside your project.
This is an example mutation:
```
mutation {
  createArtist(input: {name: "Cat Stevie", slug: "cat-stevie", clientMutationId: "abc123"}) {
    artist {
      id
      name
      slug
    }
  }
}
```

Every mutation has to include the `clientMutationId` argument. If you are running the mutation in the API EXPLORER, you can choose some arbitrary value like `clientMutationId: "abc123"` for this argument. If you run a mutation with Relay, the `clientMutationId` is automatically filled by Relay.

!! hint ""
    Note: The subselection of fields cannot be empty. If you have no specific data requirements, you can always select id as a default.


### Modifying entries
For every content model in your project, there are different mutations to _create_, _update_ and _delete_ entries.

#### Creating entries
Creates a new entry for a specific model that gets assigned a new id. All required fields of the model without a default value have to be specified, the other fields are optional arguments.
The query response can contain all fields of the newly created entry, including the id field.
```
mutation {
  createArtist(input: {name:"Cat Stevie", slug:"cat-stevie", clientMutationId: "abc123" }) {
    artist {
      id
      name
      slug  
    }
  }
}
```

When creating an entry you can directly connect it to another entry on the one-side of a relation. You can either choose to connect it to an existing entry, or even create the entry yourself:
```
mutation {
  createArtist(input: {name: "Cat Stevie", slug: "cat-stevie", records: [
    {title: "Summer Breeze", slug: "summer-breeze"}
  ], clientMutationId: "abc123"}) {
    artist {
      id
      name
      slug
    }
  }
}
```
!!! hint ""
    Note: This works for one-to-one and one-to-many relations but not for many-to-many relations.

#### Updating entries
Updates fields of an existing entry of a certain content model specified by the id field. The entry's fields will be updated according to the additionally provided values.
The query response can contain all fields of the updated entry.
```
mutation {
  updateArtist(input: {id: "cixnen2ssewlo0143bexdd52n", slug: "cat-stevie", clientMutationId: "abc123"}) {
    artist {
      id
      slug
    }
  }
}
```

#### Deleting entries
Deletes an entry specified by the id field.
The query response can contain all fields of the deleted entry.
```
mutation {
  deleteArtist(input: {id: "cixnen2ssewlo0143bexdd52n", clientMutationId: "abc123"}) {
    artist {
      id
    }
  }
}
```

#### Connect two entries in a one-to-one relation
Creates a new edge between two entries specified by their id. The according models have to be in the same relation.
The query response can contain both entries of the new edge. The names of query arguments and entry names depend on the field names of the relation.
```
mutation {
  setArtistReview(input: {reviewReviewId: "cixnen2ssewlo0143bexdd52n", artistArtistId: "cixnen2sse223412bexdd52n", clientMutationId: "abc123"}) {
    artistArtist {
      id
      name
    }
    reviewReview {
      id
      title
    }
  }
}
```
!!! hint ""
    Note: First removes existing connections containing one of the specified entries, then adds the edge connecting both entries.

You can also use the `updateArtist` or `updateReview` to connect an artist with a review:
```
mutation {
  updateArtist(input: {id: "cixnen2sse223412bexdd52n", reviewId: "cixnen2ssewlo0143bexdd52n", clientMutationId: "abc123"}) {
    artist {
      id
    }
  }
}
```

To remove an edge of an entry you have can use the `unset` mutation.
The query response can contain both entries of the former edge. The names of query arguments and entry names depend on the field names of the relation:
```
mutation {
  unsetArtistReview(input: {reviewReviewId: "cixnen2ssewlo0143bexdd52n", artistArtistId: "cixnen2sse223412bexdd52n", clientMutationId: "abc123"}) {
    artistArtist {
      id
    }
    reviewReview {
      id
    }
  }
}
```

#### Connect two entries in a one-to-many relation
One-to-many relations relate two models to each other.
An entry of the one side of a one-to-many relation can be connected to multiple entries. An entry of the many side of a one-to-many relation can at most be connected to one entry.

To create a new edge between two entries you have to use the `addTo` mutation. The according models have to be in the same relation.
The query response can contain both entries of the new edge. The names of query arguments and entry names depend on the field names of the relation.
```
mutation {
  addToTrackList(input: {tracksTrackId: "cixnen2sddseq143bexdd52n", recordRecordId: "cixnen222dfsdwebexdd52n", clientMutationId: "abc123"}) {
    recordRecord {
      id
    }
    tracksTrack {
      id
    }
  }
}

```

To remove one edge between two entries use the `removeFrom` mutation.
The query response can contain both entries of the former edge. The names of query arguments and entry names depend on the field names of the relation.
```
mutation {
  removeFromTrackList(input: {tracksTrackId: "cixnen2sddseq143bexdd52n", recordRecordId: "cixnen222dfsdwebexdd52n", clientMutationId: "abc123"}) {
    recordRecord {
      id
    }
    tracksTrack {
      id
    }
  }
}
```

!!! hint ""
    API docs are provided by GRAPHCOOL

    [![Graphcool Logo](img/graphcool.svg)](https://graph.cool)
