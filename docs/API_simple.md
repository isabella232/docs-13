# Simple API

GraphCMS provides two different APIs for you: _Relay_ and _Simple_.
Which one you choose is up to you and depends on your use case. The simple API is less complex, in comparison to the Relay API. If you don't use [Facebook's Relay](https://facebook.github.io/relay/) it might be simpler to start with the Simple API.
Of course it is up to you which one you prefer. You might also use both APIs inside your app if you want to.

## Querying the API
To query the Simple API you need to use your project specific Endpoint URL. The URL is composed by the API URL and your project ID:

`https://api.graphcms.com/simple/v1/$YOUR_PROJECT_ID$`

This URL can be used by clients like Apollo, Lokka or simple curl-request.
For example if you wan't to query the title of all posts within your project you could use the following curl command:

`curl 'https://api.graphcms.com/simple/v1/$YOUR_PROJECT_ID$' -H 'content-type: application/json' --data-binary '{"query":"query {allPosts {title}}"}' --compressed`

## Generated Queries
Using _queries_ you can ask for data you are interested in. Within each query you define a set of fields, which should be returned within the response.
All queries are generated for you and can have different arguments which can be passed into the query.

### Query a single entry
For every content model there is one query to fetch a specific entry. You have to pass a selector as an argument to this query, to retrieve the right entry.
You can either pass the entries ID or any scalar field which is marked as _unique_ within this model.
For example if you wan't to get the `name` and the `createdAt` field from the content model `Artist`, the following request can be used:
```
query {
  Artist(
    id: "cixnen2vv33lo0143bdwvr52n"
  ) {
    name
    createdAt
  }
}
```

Or, if the Artist model has a unique field `slug`:
```
query {
  Artist(
    slug: "my-awesome-artist"
  ) {
    name
    createdAt
  }
}
```
!!! hint ""
    You cannot use both parameters within one query

### Query multiple entries
The Simple API contains automatically generated queries to fetch all entries of a certain model. For example, for the Artist model the top-level query allArtists will be generated.

A few examples for query names
- model name: Artist, query name: allArtists
- model name: Track, query name: allTracks
- model name: Review, query name: allReviews.

A query which fetches all entries from the `Artist` content model could look like the following:
```
query {
  allArtists {
    id
    name
  }
}
```

!!! hint ""
    Note: The query name approximates the plural rules of the English language. If you are unsure about the actual query name, explore available queries in your API EXPLORER.

The query response of a query fetching multiple entries can be further controlled by supplying different query arguments. The response can be _ordered_, _filtered_ or _paginated_

#### Ordering entries
When querying all entries of a model you can supply the orderBy argument for every scalar field of the model:
`orderBy: <field>_ASC` or `orderBy: <field>_DESC.`

Example:
```
query {
  allArtists(
    orderBy: name_ASC
  ) {
    id
    name
  }
}
```

#### Filtering entries
When querying all entries of a model you can supply different parameters to the filter argument to filter the query response accordingly. The available options depend on the scalar fields defined on the model in question.

If you supply exactly one parameter to the filter argument, the query response will only contain entries that fulfill this constraint:
```
query {
  allArtists(
    filter: {
      published: false
    }
  ) {
    id
    name
    published
  }
}
```

Depending on the type of the field you want to filter by, you have access to different advanced criteria you can use to filter your query response:
```
allArtists(
  filter: {
    name_in: [
      "Velvet Parker",
      "Cat Stevie"
    ]
  }
) {
  id
  name
  published
}
}
```

For to-one relations, you can define conditions on the related entry by nesting the according argument in filter:
```
{
  allRecords(filter: {artist: {name: "Cat-Stevie"}}) {
    id
    slug
  }
}
```

For to-many relations, three additional arguments are available: `every`, `some` and `none`, to define that a condition should match every, some or none related entries.
```
query {
  {
    allArtists(filter: {records_every: {slug: "All-your-Base"}}) {
      id
      name
    }
  }
}
```

You can use the filter combinators `OR` and `AND` to create an arbitrary logical combination of filter conditions.
```
{
  allRecords(filter: {AND: [
    {artist: {name: "Cat-Stevie"}},
    {cover: {isPublic: true}}
  ]}) {
    id
    slug
    cover {
      url
    }
  }
}
```

You can combine and even nest the filter combinators `AND` and `OR` to create arbitrary logical combinations of filter conditions:
```
{
  allRecords(filter: {OR: [
    {AND: [{artist: {name: "Cat-Stevie"}}, {cover: {isPublic: true}}]},
    {OR: [{artist: {name_not: "Cat-Stevie"}}, {cover: {isPublic: false}}]}
  ]}) {
    id
    slug
    cover {
      url
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
  allArtists(first: 5) {
    id
    name
  }
}
```

To query the first two artists after the artists with id `cixnen2ssewlo0143bexdd52n`:
```
query {
  allArtists(
    first: 2,
    after: "cixnen2ssewlo0143bexdd52n"
  ) {
    id
    name
  }
}
```

To query the last 5 artists use `last`:
```
query {
  allArtists(last: 5) {
    id
    name
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
  createArtist(name:"Cat Stevie", slug:"cat-stevie") {
    id
    name
    slug
  }
}
```
!!! hint ""
    Note: The subselection of fields cannot be empty. If you have no specific data requirements, you can always select id as a default.

### Modifying entries
For every content model in your project, there are different mutations to _create_, _update_ and _delete_ entries.

#### Creating entries
Creates a new entry for a specific model that gets assigned a new id. All required fields of the model without a default value have to be specified, the other fields are optional arguments.
The query response can contain all fields of the newly created entry, including the id field.
```
mutation {
  createArtist(name:"Cat Stevie", slug:"cat-stevie") {
    id
    name
    slug
  }
}
```

When creating an entry you can directly connect it to another entry on the one-side of a relation. You can either choose to connect it to an existing entry, or even create the entry yourself:
```
mutation {
  createArtist(name: "Cat Stevie", slug: "cat-stevie", records: [{title: "Summer Breeze", slug: "summer-breeze"}]) {
    id
    name
    slug
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
  updateArtist(id:"cixnen2ssewlo0143bexdd52n" slug:"cat-stevie") {
    id
    slug
  }
}
```

#### Deleting entries
Deletes an entry specified by the id field.
The query response can contain all fields of the deleted entry.
```
mutation {
  deleteArtist(id:"cixnen2ssewlo0143bexdd52n") {
    id
  }
}
```

#### Connect two entries in a one-to-one relation
Creates a new edge between two entries specified by their id. The according models have to be in the same relation.
The query response can contain both entries of the new edge. The names of query arguments and entry names depend on the field names of the relation.
```
mutation {
	setArtistReview(reviewReviewId:"cixnen2ssewlo0143bexdd52n" artistArtistId:"cixnen2sse223412bexdd52n") {
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
  updateArtist(id: "cixnen2sse223412bexdd52n", reviewId: "cixnen2ssewlo0143bexdd52n") {
    id
  }
}
```

To remove an edge of an entry you have can use the `unset` mutation.
The query response can contain both entries of the former edge. The names of query arguments and entry names depend on the field names of the relation:
```
mutation {
  unsetArtistReview(reviewReviewId: "cixnen2ssewlo0143bexdd52n", artistArtistId: "cixnen2sse223412bexdd52n") {
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
  addToTrackList(tracksTrackId: "cixnen2sddseq143bexdd52n", recordRecordId: "cixnen222dfsdwebexdd52n") {
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
  removeFromTrackList(tracksTrackId: "cixnen2sddseq143bexdd52n", recordRecordId: "cixnen222dfsdwebexdd52n") {
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
