# Import Contentful Projects

## 1. The Importing Process

To import a contentful project you first need to create a new project in GraphCMS. You specify a name, description and choose your desired region. After you click next, you will be able to choose `Import from other CMS`. Select the card and click next. Now you select `Contentful` and click next again.

You are now prompted for your Contentful project tokens: `Space ID`, `Content Delivery Token` and `Content Management Token`. You can find these in Contentfuls Navigation bar under `APIs`. 


## 2. Fixing your Relations

!!! danger " "
        Relations in Contentful work a little different from the ones in GraphCMS. You will get an error when importing a project with relation fields, which didn't specify a discrete entry type.

After successfully entering all you necessary tokens and clicking `Import` you will most likely be presented with an error, when having relations in Contentful. This issue is probably occurring since you have relation fields and didn't specify a discrete entry type:

![Relations in Contentful](../img/guides/Contentful_relation.png)

You need to set these manually for every relation you have. Afterwards you should be able to continue the importing process.

## 3. Continuing the Import

After fixing the relation issues, we can now continue to the next screen, where potential `Model` and `Field` conflicts are shown. In GraphCMS `Model` names start with a capital letter instead of lowercase in Contentful.