# Getting started: Blog with Vue CLI & Apollo Client
In this tutorial, we'll learn how to create a basic blog using `Vue CLI`, `Apollo Client` and `GraphCMS`.
The complete code for this example is available [here](https://github.com/GraphCMS/graphcms-examples/tree/master/vue-apollo-blog)
# Preparation
What we'll need to get started is the `Vue CLI`
```
npm i -g vue-cli
```
Next up, we're going create our awesome app
```
vue init webpack-simple graphcms-starter-blog
```
Vue will ask us a couple of questions about our app's name, description, license type and author. You can go ahead and just `enter` through them, then answer `no` to the last `Use sass?` question.

Let's `cd` to our app and install all dependencies
```
cd graphcms-starter-blog && npm install
```
Now open up the code in the editor of your choice. You can see the main entry point to our application, which is `src/main.js` file. We'll get right on it and modify it a bit to feed the GraphQL data to our app.

For this, we'll use [Apollo Client](https://www.apollographql.com/) which is a universal GraphQL client that takes care of things like caching, pagination and feeding the data to our components in a performant way so we don't have to worry about writing all of that ourselves! Let's install everything we need with:
```
npm i -S apollo-client vue-apollo apollo-cache-inmemory apollo-link-http graphql-tag
```
That's quite a lot of packages, don't you worry though, we're gonna look at what each of them does.
* `apollo-client` is our main hero here, we'll use it to create our GraphQL client using [ApolloClient](https://www.apollographql.com/docs/react/basics/setup.html#ApolloClient).
* `vue-apollo` will be used to [install](https://github.com/Akryum/vue-apollo#create-a-provider) Apollo plugin in our Vue app and create a provider which *provides* the Apollo functionality to all the other components in the application without passing it explicitly.
* `apollo-cache-inmemory` is the recommended cache implementation for Apollo Client 2.0. `InMemoryCache` will normalize our data before saving it to the store by splitting the result into individual objects, creating a unique identifier for each object, and storing those objects in a flattened data structure.
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

For this example we'll also use `vue-router` for routing.
```
npm i -S vue-router
```


# Coding up our app
## `main.js`

Alright, we now have everything we need to start hacking! Let's come back to our `index.js` and add the following lines at the top of it
```
import { ApolloClient } from 'apollo-client'
import { HttpLink } from 'apollo-link-http'
import { InMemoryCache } from 'apollo-cache-inmemory'
import VueApollo from 'vue-apollo'
import router from './router.js'
```
_Don't worry about the `router` import not existing, we'll get to it in a moment_

Now we can initialize our Apollo Client! We will also tell Vue to install the `vue-apollo` plugin for us. To do so, add this piece after the imports
```
const GRAPHCMS_API = 'https://api.graphcms.com/simple/v1/YOUR-PROJECT-ID'

const apolloClient = new ApolloClient({
  link: new HttpLink({ uri: GRAPHCMS_API }),
  cache: new InMemoryCache()
})
Vue.use(VueApollo)
```
and replace `YOUR-PROJECT-ID` with your project's id.

Next, we need to create the `apolloProvider` and include it in our root components. A provider holds the apollo client instances that can then be used by all the child components. We will also add the imported `router` to use our `vue-router`.

This is how the rest of our `main.js` should look like after the modification
```
const apolloProvider = new VueApollo({
  defaultClient: apolloClient
})

new Vue({
  el: '#app',
  apolloProvider,
  router,
  template: '<App/>',
  components: { App }
})
```

## Styling
Styling is the least important part of it all and we've prepared the most basic set of styles to get you started. They will be included at the end of each component's overview.

## `router.js`
Since the only purpose of this file is to provide the routing for our application, we won't be digging too much into it, just pasting the following in the `router.js` will suffice:
```
import Vue from 'vue'
import Router from 'vue-router'
import Home from './components/Home.vue'
import About from './components/About.vue'
import Post from './components/Post.vue'

Vue.use(Router)

export default new Router({
  mode: 'history',
  routes: [
    {
      path: '/',
      name: 'home',
      component: Home
    },
    {
      path: '/about',
      name: 'about',
      component: About
    },
    {
      path: '/post/:slug',
      name: 'post',
      component: Post
    }
  ]
})
```
If you want to read on how `vue-router` works, you can find the documentation [here](https://router.vuejs.org/en/)

## `App.vue`
In our example, the purpose of `App` is mainly related to routing and displaying a header at the top of our application so we won't go into details here aswell. Just go ahead and replace it's content with this
```
<template>
  <div id="app">
    <app-header />
    <main>
      <router-view/>
    </main>
  </div>
</template>

<script>
  import AppHeader from './components/AppHeader.vue'

  export default {
    name: 'app',
    components: { AppHeader }
  }
</script>

<style>
  #app {
    font-family: 'Source Sans Pro', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    margin: 0;
    font-size: 16px;
    line-height: 1.5;
  }
  main {
    max-width: 650px;
    margin: 32px auto;
    padding: 0 24px;
  }
  a {
    color: deepskyblue;
    text-decoration: none;
  }
  article {
    margin: 0 auto;
    max-width: 650px;
  }
</style>
```

## components
Let's make a `components` folder in our `src` directory and create 4 components:
* `AppHeader.vue`
* `Home.vue`
* `About.vue`
* `Post.vue`

### `AppHeader.vue`
Similar to our `App`, `AppHeader` is only here to provide routing for our application. We can go ahead and paste the code below into our file
```
<template>
  <header>
    <h1>GraphCMS Starter blog</h1>
    <nav>
      <router-link exact to="/" class="link">Home</router-link>
      <router-link to="/about" class="link">About</router-link>
    </nav>
  </header>
</template>

<script>
  export default {
    name: 'AppHeader'
  }
</script>
```
Styles for `App` (just paste them at the end of the file):
```
<style>
  #app {
    font-family: 'Source Sans Pro', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    margin: 0;
    font-size: 16px;
    line-height: 1.5;
  }
  main {
    max-width: 650px;
    margin: 32px auto;
    padding: 0 24px;
  }
  a {
    color: deepskyblue;
    text-decoration: none;
  }
  article {
    margin: 0 auto;
    max-width: 650px;
  }
</style>
```

### `Home.vue`

This is the homepage of our application also responsible for showing the list of posts and a `Load more` pagination button. Let's go through it bit by bit.

First, let's create our main `Home` template
```
<template>
  <div>
    <section v-if="allPosts">
      <ul>
        <li v-for="post in allPosts" :key="post.id">
          <router-link :to="`/post/${post.slug}`" class="link">
            <div class="placeholder">
              <img
                :alt="post.title"
                :src="`https://media.graphcms.com/resize=w:100,h:100,fit:crop/${post.coverImage.handle}`"
              />
            </div>
            <h3>{{post.title}}</h3>
          </router-link>
        </li>
      </ul>
      <button v-if="postCount && postCount > allPosts.length" @click="loadMorePosts">
        {{loading ? 'Loading...' : 'Show more'}}
      </button>
    </section>
    <h2 v-else>
      Loading...
    </h2>
  </div>
</template>
```

Now, we add the component's logic:
```
<script>
  import gql from 'graphql-tag'

  const POSTS_PER_PAGE = 2

  const allPosts = gql`
    query allPosts($first: Int!, $skip: Int!) {
      allPosts(orderBy: dateAndTime_DESC, first: $first, skip: $skip) {
        id
        slug
        title
        dateAndTime
        coverImage {
          handle
        }
      }
    }
  `

  export default {
    name: 'HomePage',
    data: () => ({
      loading: 0
    }),
    apollo: {
      $loadingKey: 'loading',
      allPosts: {
        query: allPosts,
        variables: {
          skip: 0,
          first: POSTS_PER_PAGE
        }
      },
      postCount: {
        query: gql`{ _allPostsMeta { count } }`,
        update: ({ _allPostsMeta }) => _allPostsMeta.count
      }
    },
    methods: {
      loadMorePosts () {
        this.$apollo.queries.allPosts.fetchMore({
          variables: {
            skip: this.allPosts.length
          },
          updateQuery: (previousResult, { fetchMoreResult }) => {
            if (!fetchMoreResult) {
              return previousResult
            }
            return Object.assign({}, previousResult, {
              allPosts: [...previousResult.allPosts, ...fetchMoreResult.allPosts]
            })
          }
        })
      }
    }
  }
</script>
```
_Note: Your project has to have more posts than the `POSTS_PER_PAGE` indicates or you wont see the `Load more` button_

There are quite a few things going on in here, let's go through them bit by bit:

* We start with importing the `gql` module and using it to define the query we'd like to fetch the data with. We also create a `POSTS_PER_PAGE` constant to specify how many posts we'd like to have on every page.
```
  import gql from 'graphql-tag'

  const POSTS_PER_PAGE = 2

  const allPosts = gql`
    query allPosts($first: Int!, $skip: Int!) {
      allPosts(orderBy: dateAndTime_DESC, first: $first, skip: $skip) {
        id
        slug
        title
        dateAndTime
        coverImage {
          handle
        }
      }
    }
  `
```

* Next up, we name our component and tell Vue what data do we want it to have.
```
export default {
    name: 'HomePage',
    data: () => ({
      loading: 0
    }),
    apollo: {
      $loadingKey: 'loading',
      allPosts: {
        query: allPosts,
        variables: {
          skip: 0,
          first: POSTS_PER_PAGE
        }
      },
      postCount: {
        query: gql`{ _allPostsMeta { count } }`,
        update: ({ _allPostsMeta }) => _allPostsMeta.count
      }
    }
  }
```

Here we say that we'd like:

  As initial data:
  * a variable `loading` with an initial value of 0

  From apollo:
  * `$loadingKey` to be mapped to our `loading` from the initial data
  * a variable `allPosts` produced from getting the allPosts `query` data with `variables`
  * a variable `postCount` produced from getting the _allPostsMeta `query` data and extracting the `count` of our posts with `update`.

You can think of `loading` as a simple conditional that tells us what's the current state of the data fetching process. When `loading` is true, loading message will be rendered. As soon as the loading is finished, the message will be replaced with our component.

The last part of our default export is the list of methods:
```
methods: {
  loadMorePosts () {
    this.$apollo.queries.allPosts.fetchMore({
      variables: {
        skip: this.allPosts.length
      },
      updateQuery: (previousResult, { fetchMoreResult }) => {
        if (!fetchMoreResult) {
          return previousResult
        }
        return Object.assign({}, previousResult, {
          allPosts: [...previousResult.allPosts, ...fetchMoreResult.allPosts]
        })
      }
    })
  }
}
```
In our case, the list contains only 1 method `loadMorePosts`, which we'll use to load more posts with our `<button />`. The function returns the result of calling the [fetchMore](https://github.com/Akryum/vue-apollo#pagination-with-fetchmore) method on our `allPosts` query result. `fetchMore` allows us to do a new GraphQL query and merge the result into the original result.

In this case we want our button click to change the number of posts we `skip` in the new query by the amount of posts we already have on the page. We then want to return the `previousResult` of the query if there are no more posts to fetch OR a *brand new object** containing all our current posts + the newly fetched ones (we use the [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator) to connect them together).

_*We generally avoid mutating stuff where it's unnecessary because [reasons](https://www.youtube.com/watch?v=I7IdS-PbEgI) (The video talks about React but those rules apply here aswell)_

And boom, we now have a complete `Home` component with a neat pagination button! Also, if you haven't already, we strongly encourage you read more about pagination in Apollo and GraphQL in general. [This](https://www.apollographql.com/docs/react/recipes/pagination.html) page and [that](https://dev-blog.apollodata.com/understanding-pagination-rest-graphql-and-relay-b10f835549e7) post are great places to start.

Styles for `Home` (just paste them at the end of the file):
```
<style>
  #app {
    font-family: 'Source Sans Pro', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    margin: 0;
    font-size: 16px;
    line-height: 1.5;
  }
  main {
    max-width: 650px;
    margin: 32px auto;
    padding: 0 24px;
  }
  a {
    color: deepskyblue;
    text-decoration: none;
  }
  article {
    margin: 0 auto;
    max-width: 650px;
  }
</style>
```

### `Post.vue`

If you managed to follow what happened in the `Home` component, this one is much simpler and requires little more explaining. You can go ahead and paste this into our `Post.vue` file:
```
<template>
  <h2 v-if="loading > 0">
    Loading...
  </h2>
  <div v-else>
    <article>
      <h1>{{post.title}}</h1>
      <div class='placeholder'>
        <img
          :alt="post.title"
          :src="`https://media.graphcms.com/resize=w:650,h:366,fit:crop/${post.coverImage.handle}`"
        />
      </div>
      <div v-html="post.content" />
    </article>
  </div>
</template>

<script>
  import gql from 'graphql-tag'

  const post = gql`
    query post($slug: String!) {
      post: Post(slug: $slug) {
        id
        slug
        title
        coverImage {
          handle
        }
        content
        dateAndTime
      }
    }
  `

  export default {
    name: 'PostPage',
    data: () => ({
      loading: 0
    }),
    apollo: {
      $loadingKey: 'loading',
      post: {
        query: post,
        variables () {
          return {
            slug: this.$route.params.slug
          }
        }
      }
    }
  }
</script>
```

As you can see, it only gets simpler now. All we do is pass the data from apollo to our Vue component and pass `slug` that we get from `vue-router`'s `params` to the query. This is because when we enter the page `/post/:slug` we'd like our `post` variable to be the post matching the `slug`.

_You can read more about `vue-router` route params [here](https://router.vuejs.org/en/essentials/dynamic-matching.html)_

Styles for `Post`:
```
<style scoped>
  .placeholder {
    height: 366px;
    background-color: #eee;
  }
</style>
```

### `About.vue`

Last piece of the puzzle is the `About` component that will display the list of blog authors. Go ahead and paste this in:
```
<template>
  <h2 v-if="loading > 0">
    Loading...
  </h2>
  <div v-else>
    <div v-for="author in allAuthors" :key="author.id">
      <div class='author'>
        <div class='info-header'>
          <img
            :alt="author.name"
            :src="`https://media.graphcms.com/resize=w:100,h:100,fit:crop/${author.avatar.handle}`"
          />
          <h1>Hello! My name is {{author.name}}</h1>
        </div>
        <p>{{author.bibliography}}</p>
      </div>
    </div>
  </div>
</template>

<script>
  import gql from 'graphql-tag'

  export const allAuthors = gql`
    query allAuthors {
      allAuthors {
        id
        name
        bibliography
        avatar {
          handle
        }
      }
    }
  `

  export default {
    name: 'AboutPage',
    data: () => ({
      loading: 0
    }),
    apollo: {
      $loadingKey: 'loading',
      allAuthors: {
        query: allAuthors
      }
    }
  }
</script>
```

As you can see, there's nothing new going on in here, the `About` component is the simplest of them all.

Styles for `About`:
```
<style scoped>
  .author {
    margin-bottom: 72px;
  }
  .info-header {
    text-align: center;
  }
  img {
    height: 120px;
    width: auto;
  }
</style>
```

# 🎉 We've made it! 🎉

With all of this in place, we can go ahead and launch our application with 
```
npm dev
```
Congratulations! Our basic Vue Apollo blog is now ready, hack away!