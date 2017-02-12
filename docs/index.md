# Introduction

GraphCMS is a GraphQL based Headless CMS. It allows you to build a content API within minutes while taking full advantage of the powerful GraphQL language. Content stored in GraphCMS can be delivered with a simple and/or relay compliant GraphQL endpoint.

GraphCMS allows you to build websites and applications that are not restricted to any specific templating or frontend framework. It is up to your frontend developers how they would like to present your content. This gives you great flexibility. Consider GraphCMS as a content management system for your websites, web apps, mobile apps, smartwatch and TV apps and what is yet to come.

This documentation will show you how to setup a project, define your content model, manage your content and connect your applications.

## New to GraphQL?

GraphQL is a data query language and runtime designed and used at Facebook to request and deliver data to any kind of websites and apps since 2012.

### Key Benefits

#### One Endpoint to Rule Them All
Writing applications with GraphQL enables you to define the shape of your data needs on the client side. A GraphQL query returns exactly what a client asks for and no more. There is just one endpoint on the server that is capable of serving all the data that is requested.

#### Declarative and Strong-Typed
The GraphQL type system helps to ensure that your queries are valid at the time of development. This saves you from frustration and enhances the development experience.

#### Minimum Payload
Since your application receives only the data it is requesting, the payload will be limited to the minimum. This is especially important in mobile or low bandwidth scenarios. If you are querying a complex content graph, GraphQL will be able to deliver all data in just one round trip.

#### Generated API Documentation
Writing and maintaining API documentation can be cumbersome. With GraphQL, you don't have to worry about documentation at all. Through introspection, all of your API documentation will be generated automatically.

!!! tip ""
    If you want to learn more about GraphQL, we recommend you to visit the [GraphQL.org](https://graphql.org/) website as a good starting point.
