# Getting started: create-react-app and Apollo blog

🚀 **[Live Demo](https://sumdemo.com/)**

What we'll need to get started is the CRA, duh
```
npm i -g create-react-app
```
Next up, gotta create our awesome app
```
create-react-app graphcms-starter-blog
```
Next up,
```
cd graphcms-starter-blog && npm install
```
We can then start our webpack dev server with HMR, live error reporting and many other cool features by running
```
npm start
```
Now open up the code in the editor of your choice. You can see the main entry point to our application, which is `App.js` file. We'll get right on it and modify it a bit to feed the GraphQL data to our app.

For this, we'll use [Apollo Client](https://www.apollographql.com/) which is a universal GraphQL client that takes care of things like caching, pagination and feeding the data to our components in an easy and performant way so we don't have to worry about writing all of that by ourselves! Go ahead and install everything we'll need with:
```
npm i -S apollo-client react-apollo apollo-cache-inmemory apollo-link-http graphql-tag
```
That may seem like a lot of packages, don't you worry though, we're gonna look at what each of them does.
* `apollo-client` is our main hero here, we'll use it to create our GraphQL client using `new ApolloClient`.
* `react-apollo` gives us the access to `<ApolloProvider>` component which we will use to feed the data directly to our component tree.
* `apollo-cache-inmemory` the recommended cache implementation for Apollo Client 2.0. We'll take `InMemoryCache` from it. The `InMemoryCache` normalizes your data before saving it to the store by splitting the result into individual objects, creating a unique identifier for each object, and storing those objects in a flattened data structure.
* `apollo-link-http` is a standard interface for modifying control flow of GraphQL requests and fetching GraphQL results. We'll pass it the endpoint of our project so Apollo knows where to get the data from.
* `graphql-tag` is a template literal tag we will use to concisely write a GraphQL query that is parsed into the standard GraphQL AST, like so:
```
const query = gql`
  {
    user(id: 5) {
      firstName
      lastName
    }
  }
`
```

