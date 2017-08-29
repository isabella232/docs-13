# Using Nested Mutations in GraphQL

To simplify content creation and updating when using relations you can make use of `nested mutations`.
Nested mutations allow you to interact with connected parts of your type schema.

- `Nested create mutations` can be used to create and connect to a new node on the opposite side of a relation.
- `Nested connect mutations`can be used to connect and existing node to the opposite side of a relation.

## Nested Create Mutations

Nested create mutations connect the created node to a new node in the related type. Let's assume the following schema:

```
type Post implements Node {
    id: ID! @isUnique
    author: Author @relation(name: "AuthorPostRelation")
    postDescription: String
    postTitle: String
}

type Author implements Node {
    id: ID! @isUnique
    authorBib: String
    authorName: String
    posts: [Post!]! @relation(name: "AuthorPostRelation")
}
```

In the following examples we are exploring the possibilities for to-many and to-one Relations.

### To-Many Relations

To make use of a To-Many Relation we create a new `Author` node and connect it to multiple `Posts`:

```
mutation createAuthorAndPosts {
  createAuthor(
    authorName: "Hans Hansen"
    authorBib: "Hans is a very interesting guy"
    posts: [{
        postTitle: "First Post"
        postDescription: "This is a description"
      
    }, {
        postTitle: "Second Post"
        postDescription: "This is a description"
    }]
  ) {
    authorName
    authorBib
    posts {
      postTitle
      postDescription
    }
  }
}
```

The nested `posts` object takes an array of Posts with their list of arguments needed for a `createPost` mutation.
The mutations also work with query variables.

Our return should be:

```
{
  "data": {
    "createAuthor": {
      "authorName": "Hans Hansen",
      "authorBib": "Hans is a very interesting guy",
      "posts": [
        {
          "postTitle": "First Post",
          "postDescription": "This is a description"
        },
        {
          "postTitle": "Second Post",
          "postDescription": "This is a description"
        }
      ]
    }
  }
}
```

### To-One Relations

The To-one Relations works almost like the To-many one. The only difference is that we do not need to declare an array with the list of arguments for a let's say author. 

Here's what the mutation would look like if we create a `Post` and connect a new `Author` node to it:

```
mutation createPostAndAuthor {
  createPost(
    postTitle: "My only post"
      postDescription: "This is a description"
    author: {
      authorName: "Daniel Winter"
    }
  ) {
    id
    author {
      id
    }
  }
}
``` 

The example above shows, that a Post can only have a single author, which usually is the case.
Again, note that `author` gets passed all the arguments that a `create Author` mutation would also need.

The return would look like this:
```
{
  "data": {
    "createPost": {
      "id": "cj6w88kmmbh1o0119h6uqx3fj",
      "author": {
        "id": "cj6w88kmmbh1p0119kskxup11"
      }
    }
  }
}
```

All of these examples also work with update mutations. The syntax should nearly be the same.

## Nested Connect Mutations

Nested connect mutations work in a smiliar way to `nested create mutations`. You create a new node and connect an existing one to it.

Let's assume the same schema we uses for `create mutations`:
```
type Post implements Node {
    id: ID! @isUnique
    author: Author @relation(name: "AuthorPostRelation")
    postDescription: String
    postTitle: String
}

type Author implements Node {
    id: ID! @isUnique
    authorBib: String
    authorName: String
    posts: [Post!]! @relation(name: "AuthorPostRelation")
}
```

### To-Many Relations

For To-Many Relations we want to create a new author node and connect it to different posts.

```
mutation createAuthorAndPosts {
  createAuthor(
    authorName: "Hans Hansen"
    authorBib: "Hans is a very interesting guy"
    postsIds: ["cj6w8f5vna3n401426pa694nr", "cj6qhy34z1i4w0176r8o4yohb"]
  ) {
    authorName
    authorBib
    posts {
      postTitle
    }
  }
}
```

We simply specifiy an array which holds all the IDs of the posts we want to connect to the new author.

The return should look like this:
```
{
  "data": {
    "createAuthor": {
      "authorName": "Hans Hansen",
      "authorBib": "Hans is a very interesting guy",
      "posts": [
        {
          "postTitle": "A blog post"
        },
        {
          "postTitle": "This is a new post"
        }
      ]
    }
  }
}
```

### To-One Relations

We now want to connect a new post to an existing author. We do this as follows:
```
mutation createPostAndConnectAuthor {
  createPost(
    postTitle: "This is a new post"
      postDescription: "This is a description"
    authorId: "cj6w88kmmbh1p0119kskxup11"
  ) {
    id
    author {
      id
    }
  }
}
```

We define the `authorId` field with an ID that already exists and boom connected - as simple as that!

The return should look like this:
```
{
  "data": {
    "createPost": {
      "id": "cj6w8f5vna3n401426pa694nr",
      "author": {
        "id": "cj6w88kmmbh1p0119kskxup11"
      }
    }
  }
}
```

All of these examples also work with update mutations. The syntax should nearly be the same.

## Limitations

- Nested delete mutations are not available yet. Neither are cascading deletes.

- Currently, the maximum nested level is 3. If you want to nest more often than that, you need to split up the nested mutations into two separate mutations.
