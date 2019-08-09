# Introduction

GraphCMS is a GraphQL based headless CMS. It allows you to build rich content APIs within minutes while taking full advantage of the powerful GraphQL language. Your choice: content stored in GraphCMS can be delivered with a *simple* and/or *relay compliant* GraphQL endpoint.

GraphCMS allows you to build websites and applications that are not restricted to any specific templating or frontend framework. We store your content and your frontend developers choose how they like to present it. Simply consider GraphCMS as a flexible content management system for your websites, web apps, mobile apps, smartwatch and TV apps and for what is yet to come.

In this docs we will show you how to setup a project, define your content model, manage your content and connect your applications.

## New to GraphQL?

[GraphQL](http://graphql.org/learn/) is a data query language and runtime designed and used at Facebook to request and deliver data to any kind of websites and apps since 2012.

### Key Benefits
Why did we choose to build GraphCMS around GraphQL?

#### One Endpoint to Rule Them All
With GraphQL as a query language, it is up to your client application to specify the shape of the data it requires from the server. A GraphQL query returns exactly what a client asks for and no more. There is just one endpoint on the server that is capable of serving all the data that is requested.

#### Declarative and Strongly-Typed
The GraphQL type system helps to ensure that your queries are valid at the time of development. This saves you from frustration of invalid queries and boosts your productivity.

#### Minimum Payload
Since your application receives only the data it requested, the payload is limited to the minimum. This is especially important in mobile or low bandwidth scenarios. Also communication overhead is reduced: querying a complex content graph, GraphQL will be able to deliver all data in just one round trip.

#### Generated API Documentation
Writing and maintaining API documentation can be cumbersome. With GraphQL, you don't have to worry about documentation at all. Through introspection all of your API documentation will be generated automatically.

!!! tip
    If you want to learn more about GraphQL, we recommend you to visit the [GraphQL.org](https://graphql.org/) website as a good starting point.
