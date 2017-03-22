# Concepts

A quick overview on the concepts of GraphCMS.

## Projects

Your content lives within a project. In a project you can:

* Define the shape of your content by adding `content models`
* Add `fields` to your models
* Manage and browse your project´s content
* Upload and assign media files
* Invite others to your team
* Track the activities of your team
* Create `permanent auth tokens` for your applications and content consumers
* Use the integrated GraphiQL playground to run queries and mutations against your project´s endpoint

## Content Models

Content models describe the shape of your content. They consist of several fields while each field can store various types of data (e.g: text, numbers or images). A field can also be a reference to another model, which allows you to build a complex content graph. The fields you associate with a model will also define how its content editing user interface will look like.

## Fields

Fields are the building blocks of your content models. Each field type can store a specific type of data.
GraphCMS offers the following field types:

* **Text:** names, titles, list of names, comments, formatted text, markdown...
* **Number:** ID, product number, price, quantity...
* **Boolean:** true or false, yes or no...
* **Date:** post date, opening hours, date of birth...
* **Enum:** selection on a predefined set of values
* **JSON:** data in JSON format
* **Color:** rgba or hex color string
* **Location:** geographic coordinates: latitude and longitude
* **Media:** any asset, e.g. image, video...
* **Relation:** for referencing other content models. E.g. the author of a blog post


![Screenshot](img/screenshots/fieldWizardClean.png)

## Webhooks

Webhooks are a powerful concept that enable you to execute your own business logic in case of specified events.

In GraphCMS, they allow you to define triggers on your content models, so you can run, for example, custom code if your content changes.

Please see the [webhooks section](/Webhooks) for more information.

## GraphQL Endpoints

Any GraphCMS project comes with two GraphQL endpoints:

* The `simple endpoint` serves your content with a simple GraphQL schema for use with GraphQL clients like [Apollo](http://dev.apollodata.com/), [Apollo iOS](https://github.com/apollostack/apollo-ios) or [Lokka](https://github.com/kadirahq/lokka)
* The `relay endpoint` serves your content with a relay conform GraphQL schema for [data driven react applications](https://facebook.github.io/relay/)  

## Permanent Auth Tokens

To connect your client applications with your GraphCMS backend, you will need to create `permanent auth tokens`. This will allow your external client to:

* `CREATE` new content entries
* `READ` existing content entries
* `UPDATE` existing content entries
* `DELETE` existing content entries

[How to work with permanent auth tokens](/guides/Using_permanent_auth_tokens)

!!! warning ""
    Be careful! Anyone that gains access to one of your tokens will be able to execute all of these operations and manipulate your content. So it is never a good idea to store a token on the client side, i.e. a JavaScript client application.

    We will soon release a feature that will allow you to create `read-only tokens`, so even if someone gains access to such a token, your data will be safe from manipulation.

!!! hint ""
    As an alternative, you can set up public access for create, read, update and delete operations ([see here](/guides/Public_API_access)). This will allow you to skip the token step.

## System Artifacts

Each GraphCMS project comes with a few system artifacts, that users should be aware of. Those are models and fields that are created automatically by the system.

### Media Model

The `Media` system model is where your uploaded files get stored. Each time you assign a media field to one of your content models, a relation to this model is created.

The fields of the `Media` model are:

* `fileName` the name of the file
* `handle` the unique identifier of the file
* `url` the public url of the file, which is always media.graphcms.com/`handle`
* `size` the size of the file
* `mimeType` the internet media type of the file
* `isPublic` a flag that indicates if the file is available for the public (currently always `true`)

### System Fields

Each content model in GraphCMS has the following system fields by default:

* `id` stores the unique identifier of a content entry
* `createdAt` the date the content entry was created
* `updatedAt` the date the content entry was updated the last time
* `isPublished` indicates if the content entry is published ([see here](/guides/Publishing_workflow))
