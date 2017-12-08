# Getting started: Blog with create-react-app & Apollo Client

_Note: This guide assumes you have some knowledge about React and GraphQL. If you don't yet, we *highly* recommend you check out [this](https://www.howtographql.com/) to learn about GraphQL and [this](https://reactjs.org/tutorial/tutorial.html) to learn about React._

In this tutorial, we'll learn how to create a basic blog using `create-react-app`, `Apollo Client` and `GraphCMS`. The complete code for this example is available [here](https://github.com/GraphCMS/graphcms-examples/tree/master/react-apollo-blog).

You can also see and play around with it in the awesome CodeSandbox editor below!
<iframe src="https://codesandbox.io/embed/github/GraphCMS/graphcms-examples/tree/master/react-apollo-blog" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

# Preparation
What we'll need to get started is the `create-react-app` CLI
```
npm i -g create-react-app
```
Next up, we're going create our awesome app
```
create-react-app graphcms-starter-blog
```
then let's `cd` to our app and install all dependencies
```
cd graphcms-starter-blog && yarn
```
Now open up the code in the editor of your choice. You can see the main entry point to our application, which is `src/index.js` file. We'll get right on it and modify it a bit to feed the GraphQL data to our app.

For this, we'll use [Apollo Client](https://www.apollographql.com/) which is a universal GraphQL client that takes care of things like caching, pagination and feeding the data to our components in a performant way so we don't have to worry about writing all of that ourselves! Let's install everything we need with:
```
yarn add apollo-client react-apollo apollo-cache-inmemory apollo-link-http graphql-tag
```
That's quite a lot of packages, don't you worry though, we're gonna look at what each of them does.
* `apollo-client` is our main hero here, we'll use it to create our GraphQL client using [ApolloClient](https://www.apollographql.com/docs/react/basics/setup.html#ApolloClient).
* `react-apollo` gives us the access to [ApolloProvider](https://www.apollographql.com/docs/react/basics/setup.html#ApolloProvider) React component which *provides* the React Apollo functionality to all the other components in the application without passing it explicitly. The package also contains `graphql` function used to "enchance" our components with data.
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

For this example we'll also use `react-router` for routing and `react-markdown` to parse the markdown we get from GraphCMS post's `content` field.
```
yarn add react-router-dom react-markdown
```


# Coding up our app
## `index.js`

Alright, we now have everything we need to start hacking! Let's come back to our `index.js` and add the following lines at the top of it
```
import { ApolloClient } from 'apollo-client'
import { HttpLink } from 'apollo-link-http'
import { InMemoryCache } from 'apollo-cache-inmemory'
import { ApolloProvider } from 'react-apollo'
```

Now we can initialize our Apollo Client! To do so, add this piece after the imports
```
const GRAPHCMS_API = 'https://api.graphcms.com/simple/v1/YOUR-PROJECT-ID'

const client = new ApolloClient({
  link: new HttpLink({ uri: GRAPHCMS_API }),
  cache: new InMemoryCache()
})
```
and replace `YOUR-PROJECT-ID` with your project's id.

Next, we need to wrap our rendered `<App />` component in `ApolloProvider` so we can access our data throughout the application.

This is how it should look like after the modification
```
ReactDOM.render(
  <ApolloProvider client={client}>
    <App />
  </ApolloProvider>,
  document.getElementById('root')
)
```

## `index.css`
Styling is the least important part of it all and we've prepared the most basic set of styles to get you started. To use them, just replace the content of the `index.css` file with [this](https://github.com/GraphCMS/graphcms-examples/blob/master/react-apollo-blog/src/index.css)

## `App.js`
In our example, the purpose of `App` is mainly related to routing and displaying a header at the top of our application so we won't go into details here. Just go ahead and replace it's content with this
```
import React from 'react'
import { BrowserRouter as Router, Route } from 'react-router-dom'

import Header from './Header'
import Home from './Home'
import About from './About'
import Post from './Post'

const App = () => (
  <Router>
    <div>
      <Header />
      <main>
        <Route exact path='/' component={Home} />
        <Route path='/about' component={About} />
        <Route path='/post/:slug' component={Post} />
      </main>
    </div>
  </Router>
)

export default App
```

## components
Let's make a `components` folder in our `src` directory and create 4 components:
* `Header.js`
* `Home.js`
* `About.js`
* `Post.js`

### `Header.js`
Similar to our `App.js`, `Header.js` is only here to provide routing for our application. We can go ahead and paste the code below into our file
```
import React from 'react'
import { NavLink } from 'react-router-dom'

export default () => (
  <header className='Header-header'>
    <h1 className='Header-h1'>GraphCMS Starter blog</h1>
    <nav className='Header-nav'>
      <NavLink
        exact to='/'
        className='Header-navLink'
        activeClassName='Header-isActive'
      >
        Home
      </NavLink>
      <NavLink
        to='/about'
        className='Header-navLink'
        activeClassName='Header-isActive'
      >
        About
      </NavLink>
    </nav>
  </header>
)
```

### `Home.js`

This is the homepage of our application also responsible for showing the list of posts and a `Load more` pagination button. Let's go through it bit by bit.

First up, let's import all the required modules and add a pagination constant that we will need in a minute:

```
import React from 'react'
import { Link } from 'react-router-dom'
import { graphql } from 'react-apollo'
import gql from 'graphql-tag'

const POSTS_PER_PAGE = 4
```

Then, we create our main `Home` component
```
const Home = ({ data: { loading, error, allPosts, _allPostsMeta }, loadMorePosts }) => {
  if (error) return <h1>Error fetching posts!</h1>
  if (!loading) {
    const areMorePosts = allPosts.length < _allPostsMeta.count
    return (
      <section>
        <ul className='Home-ul'>
          {allPosts.map(post => (
            <li className='Home-li' key={`post-${post.id}`}>
              <Link to={`/post/${post.slug}`} className='Home-link'>
                <div className='Home-placeholder'>
                  <img
                    alt={post.title}
                    className='Home-img'
                    src={`https://media.graphcms.com/resize=w:100,h:100,fit:crop/${post.coverImage.handle}`}
                  />
                </div>
                <h3>{post.title}</h3>
              </Link>
            </li>
          ))}
        </ul>
        <div className='Home-showMoreWrapper'>
          {areMorePosts
            ? <button className='Home-button' onClick={() => loadMorePosts()}>
              {loading ? 'Loading...' : 'Show More Posts'}
            </button>
            : ''}
        </div>
      </section>
    )
  }
  return <h2>Loading posts...</h2>
}
```
_Note: Your project has to have more posts than the `POSTS_PER_PAGE` indicates or you wont see the `Load more` button_

As you can see, our functional component takes in 2 props, `data` (from which we take the things we want with a bit of ES6 destructuring magic) and `loadMorePosts` function that our `<button />` uses.

You can think of `loading` and `error` as simple conditionals that tell us what's the current state of the data fetching process. When `error` prop is true, error message will be rendered instead of our component. Similarly, when `loading` is true, loading message will be rendered. As soon as the loading is finished, the message will be replaced with our component (or the error message if something goes wrong). `loadMorePosts` is something we will get to in a minute.

Right below our `Home` component, let's place:
```
export const allPosts = gql`
  query allPosts($first: Int!, $skip: Int!) {
    allPosts(orderBy: dateAndTime_DESC, first: $first, skip: $skip) {
      id
      slug
      title
      dateAndTime
      coverImage {
        handle
      }
    },
    _allPostsMeta {
      count
    }
  }
`
```
This is the query we use to tell Apollo what *exact* data we'd like it to get for us. We also specify that our `query allPosts` takes in 2 [variables](http://graphql.org/learn/queries/#variables) `first` and `skip` which we will then pass as [arguments](http://graphql.org/learn/queries/#arguments) to the query to specify how many posts to fetch (`first`) and where to start (`skip`). This will be useful for our pagination.

Now, for the pagination itself, at the end of the file we add:
```
export const allPostsQueryVars = {
  skip: 0,
  first: POSTS_PER_PAGE
}

export default graphql(allPosts, {
  options: {
    variables: allPostsQueryVars
  },
  props: ({ data }) => ({
    data,
    loadMorePosts: () => {
      return data.fetchMore({
        variables: {
          skip: data.allPosts.length
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
  })
})(Home)
```

Here, we use `allPostsQueryVars` to set the values of the variables we will be passing to the query. We `skip` 0 to start from the 1st article and we fetch the `first` 4 posts, because that's what our `POSTS_PER_AGE` equals to.

Last bit is the `graphql` function that we call with [(query, [config])(component)](https://www.apollographql.com/docs/react/basics/setup.html#graphql). Component is of course our `Home` component, the query is `allPosts` but the options is where things get interesting.

Since we don't want to display all our posts at once but rather show the first few and load `POSTS_PER_PAGE` more with each subsequent click of our `<button />`, we need to tell Apollo what we're up to.

We start by telling Apollo with [options](https://www.apollographql.com/docs/react/basics/setup.html#graphql-config-options) that we want it to use our previously declared `allPostsQueryVars` as variables in the `allPosts` query:

```
{
  options: {
    variables: allPostsQueryVars
  }
}
```

We follow up with props that allows us to define a map function that takes in the props of our component (including those defined by Apollo). 
[props](https://www.apollographql.com/docs/react/basics/setup.html#graphql-config-props) is most useful when you want to abstract away complex functions calls into a simple prop that you can pass down to your component.
```
props: ({ data }) => ({
    data,
    loadMorePosts: () => {
      return data.fetchMore({
        variables: {
          skip: data.allPosts.length
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
  })
```
Here the function takes `data` from our props, and creates a new set of props containing both `data` and the `loadMorePosts` function (that our `<button />` is using). The function returns the result of calling the [fetchMore](https://www.apollographql.com/docs/react/recipes/pagination.html#fetch-more) method on our data prop object. `fetchMore` allows us to do a new GraphQL query and merge the result into the original result.

In this case we want our button click to change the number of posts we `skip` in the new query by the amount of posts we already have on the page. We then want to return the `previousResult` of the query if there are no more posts to fetch OR a *brand new object** containing all our current posts + the newly fetched ones (we use the [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator) to connect them together).

_*We generally avoid mutating stuff where it's unnecessary because [reasons](https://www.youtube.com/watch?v=I7IdS-PbEgI)_

And boom, we now have a complete `Home` component with a neat pagination button! Also, if you haven't already, we strongly encourage you read more about pagination in Apollo and GraphQL in general. [This](https://www.apollographql.com/docs/react/recipes/pagination.html) page and [that](https://dev-blog.apollodata.com/understanding-pagination-rest-graphql-and-relay-b10f835549e7) post are great places to start.

### `Post.js`

If you managed to follow what happened in the `Home` component, this one is much simpler and requires little more explaining. You can go ahead and paste this into our `Post.js` file:
```
import React from 'react'
import gql from 'graphql-tag'
import { graphql } from 'react-apollo'
import Markdown from 'react-markdown'

const Post = ({ data: { loading, error, Post } }) => {
  if (error) return <h1>Error fetching the post!</h1>
  if (!loading) {
    return (
      <article>
        <h1>{post.title}</h1>
        <div className='Post-placeholder'>
          <img
            alt={post.title}
            src={`https://media.graphcms.com/resize=w:650,h:366,fit:crop/${post.coverImage.handle}`}
          />
        </div>
        <Markdown
          source={post.content}
          escapeHtml={false}
        />
      </article>
    )
  }
  return <h2>Loading post...</h2>
}

export const singlePost = gql`
  query singlePost($slug: String!) {
    Post(slug: $slug) {
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

export default graphql(singlePost, {
  options: ({ match }) => ({
    variables: {
      slug: match.params.slug
    }
  })
})(Post)
```

As you can see, it only gets simpler now. All we do is "enchance" our `Post` with data and pass `slug` that we get from `react-router`'s `params` to the query. This is because when we enter the page `/post/:slug` we'd like our `data` prop to contain only the post matching the `slug`.

_You can read more about `react-router` route params [here](https://github.com/reactjs/react-router-tutorial/tree/master/lessons/06-params)_

### `About.js`

Last piece of the puzzle is the `About` component that will display the list of blog authors. Go ahead and paste this in:
```
import React from 'react'
import gql from 'graphql-tag'
import { graphql } from 'react-apollo'

const About = ({ data: { loading, error, allAuthors } }) => {
  if (error) return <h1>Error fetching authors!</h1>
  if (!loading) {
    return (
      <div>
        {allAuthors.map(author => (
          <div className='About-author' key={author.id}>
            <div className='About-infoHeader'>
              <img
                className='About-img'
                alt={author.name}
                src={`https://media.graphcms.com/resize=w:100,h:100,fit:crop/${author.avatar.handle}`}
              />
              <h1>Hello! My name is {author.name}</h1>
            </div>
            <p>{author.bibliography}</p>
          </div>
        ))}
      </div>
    )
  }
  return <h2>Loading author...</h2>
}

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

export default graphql(allAuthors)(About)
```

As you can see, there's nothing new going on in here, the `About` component is the simplest of them all.

# 🎉 We've made it! 🎉

With all of this in place, we can go ahead and launch our application with 
```
yarn start
```
Congratulations! Our basic React Apollo blog is now ready, hack away!