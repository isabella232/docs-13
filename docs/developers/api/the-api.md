# The API

The underlying technology of GraphCMS received a fantastic update and now the previous Relay and Simple API's have been merged into one. One end point, many possibilities!

To make this new "one-api-to-rule-them-all", a few changes were necessary to bring the APIs into parity.

## Queries

{% method %}
### No more explicit all- prefix
The API will be a little different from what you are used in the current version. The `all`-Prefix from the queries is gone now. So a simple Query might look like this:

{% sample lang="old" %}
```json
query {
    allPosts {
        id
        title
        content
    }
}
```

{% sample lang="new" %}
```json
query {
    posts {
        id
        title
        content
    }
}
```
{% endmethod %}

{% method %}
### Filtering
If you want to filter you now need to use the `where` argument:

{% sample lang="old" %}
```json
query {
    allAuthors(
        filter: {
            age_gt: 18
        }) {
        id
        title
        content
    }
}
```

{% sample lang="new" %}
```json
query {
  authors(where: {
    age_gt: 18
  }) {
    id
    name
  }
}
```

{% common %}
This will be familiar to may developers who've spent time writing SQL queries. 


{% endmethod %}

{% method %}
A more advanced example:

{% sample lang="old" %}
```json
query {
  posts(filter: {
    title_in: ["My biggest Adventure", "My latest Hobbies"]
  }) {
    id
    title
    published
  }
}
```

{% sample lang="new" %}
```json
query {
  posts(where: {
    title_in: ["My biggest Adventure", "My latest Hobbies"]
  }) {
    id
    title
    published
  }
}
```

{% endmethod %}

{% method %}
and as before, you can chain boolean operators `AND` and `OR`:

{% sample lang="old" %}
```json
query {
  allPosts(filter: {
    AND: [{
      title_in: ["My biggest Adventure", "My latest Hobbies"]
    }, {
      published: true
    }]
  }) {
    id
    title
    published
  }
}
```

{% sample lang="new" %}
```json
query {
  posts(where: {
    AND: [{
      title_in: ["My biggest Adventure", "My latest Hobbies"]
    }, {
      published: true
    }]
  }) {
    id
    title
    published
  }
}
```

{% endmethod %}

### Ordering
An example of ordering the response, the same as previous
```json
query {
  posts(orderBy: title_ASC) {
    id
    title
    published
  }
}
```

### Pagination
An example of a paginating query.
```json
query {
  posts(
    first: 2
    skip: 1
  ) {
    id
    title
  }
}
```

```json
query {
  posts(
    first: 2
    after: "cixnen24p33lo0143bexvr52n"
  ) {
    id
    title
  }
}
```

## Mutations
Mutations look different with the new syntax. The primary difference is the addition of a new `data` field in the body of the mutation. See below for examples.

{% method %}

### Create

{% sample lang="old" %}
```json

mutation {
  createAuthor(
      age: 42
      email: "zeus@example.com"
      name: "Zeus"
  ) {
    id
    name
  }
}
```

{% sample lang="new" %}
```json
mutation {
  createAuthor(
    data: {
      age: 42
      email: "zeus@example.com"
      name: "Zeus"
    }
  ) {
    id
    name
  }
}
```

{% endmethod %}

{% method %}

### Update
Update takes both a `data` field AND a `where` field

{% sample lang="old" %}
```json
mutation {
  updateAuthor(
      email: "zeus2@example.com"
      name: "Zeus2"
  ) {
    id
    name
  }
}
```

{% sample lang="new" %}
```json
mutation {
  updateAuthor(
    data: {
      email: "zeus2@example.com"
      name: "Zeus2"
    }
    where: {
      email: "zeus@example.com"
    }
  ) {
    id
    name
  }
}
```

{% endmethod %}

{% method %}

### Upsert (Update OR Insert)
When we want to either update an existing node, or create a new one in a single mutation, we can use _upsert_ mutations. Previously this was accomplished with the little documented utility, `updateOrCreate`.


{% sample lang="new" %}
```json
mutation {
  upsertAuthor(
    where: {
      email: "zeus@example.com"
    }
    create: {
      email: "zeus@example.com"
      age: 42
      name: "Zeus"
    }
    update: {
      name: "Zeus"
    }
  ) {
    name
  }
}
```

Since this is not a very DRY exmaple, a more common practice would be something like this:

```json
mutation UpsertAuthor($email: String!, $age: Int, $name: String) {
  upsertAuthor(
    where: {
      id: $email
    }
    create: {email: $email, age: $age, name: $name}
    update: {email: $email, age: $age, name: $name}
  ) {
    name
    id
    email
  }
}
```

{% common %}
Note, `email` here is a unique field on the model. For more about variables, [look here.](http://graphql.org/learn/queries/#variables)

{% endmethod %}

{% method %}

### Delete
Deleting builds on the usage of `where` but omits the `data` field.

{% sample lang="old" %}
```json
mutation {
  deleteAuthor(
      id: "12345"
  ) {
    id
    name
  }
}
```

{% sample lang="new" %}
```json
mutation {
  deleteAuthor(where: {
    id: "cjcdi63l20adx0146vg20j1ck"
  }) {
    id
    name
    email
  }
}
```

{% endmethod %}