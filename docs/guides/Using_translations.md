# Using Translations

This guide will illustrate the usage of our localization feature.

!!! danger " "
    To make use of Content Localization you need to have a project in trial or purchase a [Growth plan](https://graphcms.com/pricing)

## Setup

!!! hint " "
    Translations can only be activated for new fields. Once a field is created without "isTranslatable" you cannot make is translatable afterwards!

Before diving into the content creation we want to setup our project for the locales we need later. To set this up, we go into "Settings" and scroll to "Content Localization"

![Settings Localization](../img/guides/translations/settings_locale.png)

Here you can now search for existing languages and add them to your collection. When clicking on the language abbreviation you set it as default. 

!!! hint " "
    You are also able to define custom locales! Get yourself a Klingon website 🖖

<video controls>
  <source src="../NewLocale.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

## Adding Translatable Fields

We can now switch back to the "Model" View and create new a model or use a existing one. To this model we will now add some fields which we mark as "Field is translatable"

![Field translatable](../img/guides/translations/SetFieldTranslatable.png)

The Model View should now look something like this:

![Model View](../img/guides/translations/ModelViewForTranslation.png)

Translation is handled like a Relation, which gets important when accessing it from the API.

## Creating Localized Content

When switching to the content view, you can now create new entries for the translated fields. You have the option to group the fields by language or by type for easier editing. You can also select which locales you want to have displayed in the editing view. 

<video height="600px" controls>
  <source src="../CreatePostTranslate.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

## API Perspective

Since Translations will be handled as a Relation, you need to parse it as such. Here is a simple demo query:

```json
{
  allPosts {
    translations {
      language
      postTitle
      postContent
    }
  }
}
```

The response then would be an array of all the set translations for these fields:
```json
{
  "data": {
    "allPosts": [
      {
        "translations": [
          {
            "language": "DE",
            "postTitle": "Das ist der Titel",
            "postContent": "Inhalt\n"
          },
          {
            "language": "EN",
            "postTitle": "This is the title",
            "postContent": "Content\n"
          }
        ]
      }
    ]
  }
}
```

!!! hint " "
    If any question are still unanswered feel free to message us via On-Site-Chat or [Slack](https://slack.graphcms.com)