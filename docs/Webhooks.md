# Webhooks

[Webhooks](http://www.webhooks.org/) are a powerful concept that enable you to execute your own business logic in case of specified events.

In GraphCMS, they allow you to define triggers on your content models, so you can run, for example, custom code if your content changes.

## Setting up a Webhook

Creating a webhook in GraphCMS is straight forward. Switch to the `WEBHOOKS` view and click the `ADD WEBHOOK` button.

### 1. Trigger

Select the content model and the action that should trigger the webhook event. You can choose between the actions `CREATE`, `UPDATE` and `DELETE`.

> **Example**
>
>  Trigger a webhook on model `Post`, if there was an `UPDATE` on the content of the model

### 2. Payload

Define the payload that should be transfered with your webhook event. This is done simply with a GraphQL query.

> **Example for `CREATE` trigger**
>
> `{ createdNode { id title content } }`

<!-- -->
> **Example for `UPDATE` trigger**
>
> `{
  changedFields
  previousValues {
    id
    title
    content
  }
  updatedNode {
    id
    title
    content
  }
}`

### 3. Webhook Settings

Here you can define the endpoint URL for your custom business logic code. This could be, for example, an [AWS lambda function](https://aws.amazon.com/de/lambda/details/).

You can also define if your webhook is triggered synchronously or asynchronously. I.e., if you set up a synchronous `CREATE` webhook for model `Post`, a create mutation for this model will return after your custom business logic has finished.

Your webhook will be active by default. You can disable it by clicking on the switch in the webhook´s entry.

!!! Warning ""
    If you change your content models or modify your fields, remember to update affected webhooks accordingly! This does not happen automatically.
