# The API

Welcome to the new GraphCMS API. The previous two API is being deprecated in favor of a combined syntax that supports all GraphQL clients.

## Querying the API
To query the API you need to use your project specific Endpoint URL. The URL is composed by the API URL and your project ID:

`https://api.graphcms.com/simple/v1/$YOUR_PROJECT_ID$`

This URL can be used by clients like Apollo, Lokka, UrQL, or even a simple curl-request.
For example if you wan't to query the title of all posts within your project you could use the following curl command:

`curl 'https://api.graphcms.com/simple/v1/$YOUR_PROJECT_ID$' -H 'content-type: application/json' --data-binary '{"query":"query {posts {title}}"}' --compressed`

## Generated Queries
Using _queries_ you can ask for data you are interested in. Within each query you define a set of fields, which should be returned within the response.
All queries are generated for you and can have different arguments which can be passed into the query.

### Query a single entry
For every content model there is one query to fetch a specific entry. You have to pass a selector as an argument to this query, to retrieve the right entry.
You can either pass the entries ID or any scalar field which is marked as _unique_ within this model.
For example if you want to get the `name` and the `createdAt` field from the content model `Artist`, the following request can be used:
```json
query {
  artist( where: {
    id: "cixnen2vv33lo0143bdwvr52n"
  }
  ) {
    name
    createdAt
  }
}
```
{hint style="info"}
Note the use of the `where` clause. This is a new addition to the API and is used for all selection-like queries.
{% endhint %}

Any unique field in the model can be used for search/selection, such as `slug`:
```json
query {
  artist( where: {
    slug: "my-awesome-artist"
  } ) {
    name
    createdAt
  }
}
```
{hint style="info"}
You cannot use both parameters within one query
{% endhint %}

### Query multiple entries
The API contains automatically generated queries to fetch all entries of a certain model. For example, for the Artist model the top-level query `artists` will be generated.

A few examples for query names
| Model Name | Query Name |
|---|---|
| Artist | artists |
| Track | tracks |
| Review | reviews |

A query which fetches all entries from the `Artist` content model could look like the following:
```json
query {
  artists {
    id
    name
  }
}
```

{% hint style="info" %}
Note: The query name approximates the plural rules of the English language. If you are unsure about the actual query name, explore available queries in your API EXPLORER.
{% endhint %}

The query response of a query fetching multiple entries can be further controlled by supplying different query arguments. The response can be _ordered_, _filtered*_ or _paginated_

#### Ordering entries
When querying all entries of a model you can supply the orderBy argument for every scalar field of the model:
`orderBy: <field>_ASC` or `orderBy: <field>_DESC.`

Example:
```json
query {
  artists( where: {
    orderBy: name_ASC
  }) {
    id
    name
  }
}
```

#### Filtering entries
When querying all entries of a model you can supply different parameters to the filter argument to filter the query response accordingly. The available options depend on the scalar fields defined on the model in question.

If you supply exactly one parameter to the filter argument, the query response will only contain entries that fulfill this constraint:
```json
query {
  artists( where: {
      published: false
}) {
    id
    name
    published
  }
}
```

Depending on the type of the field you want to filter by, you have access to different advanced criteria you can use to filter your query response:
```json
artists( where: {
    name_in: [
      "Velvet Parker",
      "Cat Stevie"
    ]
  }) {
  id
  name
  published
}
```

A non-exhaustive list of these "_helper" filters are:
| Matches | Behavior |
| --- | --- |
| *_in | One of |
| *_lt | Less than |
| *_gt | Greater than |
| *_not | Not this |
| *_lte | Less than or equal to |
| *_gte | Greater than or equal to |
| *_not_in | Not one of |
| *_starts_with | Starts with string |
| *_not_starts_with | Doesn't start with string |
| *_ends_with | Ends with string |
| *_not_ends_with | Doesn't end with string |
| *_contains | Includes string |
| *_not_contains | Does not include string |


For **to-one** relations, you can define conditions on the related entry by nesting the according argument in filter:
```json
{
  records( where: {
      artist: {
          name: "Cat-Stevie"
      }}) {
    id
    slug
  }
}
```
##### Every, Some, None

For to-many relations, three additional arguments are available: `every`, `some` and `none`, to define that a condition should match every, some or none related entries.
```json
{
  artists(where: {
      records_every: {
          slug: "All-your-Base"}}) {
      id
      name
    }
  }
```

##### AND / OR
You can use the boolean operators `OR` and `AND` to create an arbitrary logical combination of filter conditions.
```json
{
  records(where: {AND: [
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

You can combine and even nest the boolean operators `AND` and `OR` to create arbitrary logical combinations of filter conditions:
```JSON
{
  records(where: {OR: [
    {AND: [
        {artist:
            {name: "Cat-Stevie"}},
            {cover: {isPublic: true}}]},
    {OR: [
        {artist:
            {name_not: "Cat-Stevie"}},
            {cover: {isPublic: false}}]}
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

- to seek forwards, use `first`; specify the pointer location with `after`.
- to seek backwards, use `last`; specify the pointer location with `before`.

You can also skip an arbitrary amount of entries in whichever direction you are seeking by supplying the `skip` argument:
```json
{
  artists(first: 5) {
    id
    name
  }
}
```
 
To query the first two artists after the artists with id `cixnen2ssewlo0143bexdd52n`:
```json
{
  artists(
    first: 2,
    after: "cixnen2ssewlo0143bexdd52n"
  ) {
    id
    name
  }
}
```

To query the last 5 artists use `last`:
```json
{
  artists(last: 5) { where: {}
    id
    name
  }
}
```

{% hint style="info" %}
Note: You cannot combine `first` with `before` or `last` with `after`.

Note: If you query more entries than exist, your response will simply contain all entries that actually do exist in that direction.
{% endhint %}

## Generated mutations
With a mutation you can modify the data of your project. Similar to queries, all mutations are automatically generated. Explore them by using the API EXPLORER inside your project.
This is an example mutation:
```json
mutation {
  createArtist(data: {
      name:"Cat Stevie",
      slug:"cat-stevie"
      }) {
    id
    name
    slug
  }
}
```
{% hint style="info"% }
Note: The sub-selection of fields cannot be empty. If you have no specific data requirements, you can always select `id` as a default.
{% endhint %}

### Modifying entries
For every content model in your project, there are different mutations to _create_, _update_ and _delete_ entries.

#### Creating entries
Creates a new entry for a specific model that gets assigned a new id. All required fields of the model without a default value have to be specified, the other fields are optional arguments.
The query response can contain all fields of the newly created entry, including the id field.
```json
mutation {
  createArtist(data: {
      name: "Cat Stevie",
      slug:"cat-stevie"
    }) {
    id
    name
    slug
  }
}
```

When creating an entry you can directly connect it to another entry on the one-side of a relation. You can either choose to connect it to an existing entry, or even create the entry yourself:
```json
mutation {
  createArtist(data: {
      name: "Cat Stevie",
      slug: "cat-stevie",
      records: [{
          title: "Summer Breeze",
          slug: "summer-breeze"}
        ]}) {
    id
    name
    slug
  }
}
```
{% hint type="info" %}
Note: This works for one-to-one and one-to-many relations but not for many-to-many relations.
{% endhint %}

#### Updating entries
Updates fields of an existing entry of a certain content model specified by the id field. The entry's fields will be updated according to the additionally provided values.
The query response can contain all fields of the updated entry.
```json
mutation {
  updateArtist(
        data: {slug:"cat-stevie"},
        where: {id:"cixnen2ssewlo0143bexdd52n"}) {
    id
    slug
  }
}
```

#### Deleting entries
Deletes an entry specified by the id field.
The query response can contain all fields of the deleted entry.
```json
mutation {
  deleteArtist(where: {
      id:"cixnen2ssewlo0143bexdd52n"
    }) {
    id
  }
}
```

#### Connect two entries in a one-to-one relation

{% hint type="danger" %}
This section is a **WIP** and is subject to change. The API listed below may or may not work.
{% endhint %}

Creates a new edge between two entries specified by their id. The according models have to be in the same relation.
The query response can contain both entries of the new edge. The names of query arguments and entry names depend on the field names of the relation.
```json
mutation {
  setArtistReview(
  reviewReviewId: "cixnen2ssewlo0143bexdd52n" artistArtistId:"cixnen2sse223412bexdd52n") {
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
{% hint style="info" %}
Note: First removes existing connections containing one of the specified entries, then adds the edge connecting both entries.
{% endhint %}

You can also use the `updateArtist` or `updateReview` to connect an artist with a review:
```json
mutation {
  updateArtist(id: "cixnen2sse223412bexdd52n", reviewId: "cixnen2ssewlo0143bexdd52n") {
    id
  }
}
```

To remove an edge of an entry you have can use the `unset` mutation.
The query response can contain both entries of the former edge. The names of query arguments and entry names depend on the field names of the relation:
```json
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

{% hint type="danger" %}
This section is a **WIP** and is subject to change. The API listed below may or may not work.
{% endhint %}

One-to-many relations relate two models to each other.
An entry of the one side of a one-to-many relation can be connected to multiple entries. An entry of the many side of a one-to-many relation can at most be connected to one entry.

To create a new edge between two entries you have to use the `addTo` mutation. The according models have to be in the same relation.
The query response can contain both entries of the new edge. The names of query arguments and entry names depend on the field names of the relation.
```json
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
```json
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

{% hint type="info" %}
API docs are inspired by / copied from GRAPHCOOL

[![Graphcool Logo](../../gitbook/images/graphcool.svg)](https://graph.cool)

{% endhint %}