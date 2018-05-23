# Content
For a reference on using the content view to fill in content, please see the [user documentation on content](../users/cmi/content.md).

For a developer, content composes the bedrock of a query. Every data field present on a content model is queriable.

Take a project with this type of model:

!["Overview of CMI for Content Models"](../gitbook/images/project-api-overview.png)

You can write queries like these:
```json
{
    superHeroes {
        name,
        id,
        superPower {
            strength
        }
    }
}
```

```json
{
    superHeroes(where: {
        alliance {
            displayName: "Noble"
        }
    }) {
        name,
        id,
        superPower {
            strength
        }
    }
}
```

And they will return the exact data you are looking for.

That's the power of GraphQL.

We are also adding support for saved views. In a saved view you could write a custom query or add custom labels to projects to group them together for ultimate content creator workflow.