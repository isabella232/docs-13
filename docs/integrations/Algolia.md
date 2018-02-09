# Algolia

!!! Hint ""
    All of Algolias indexing is not covered within our pricing plans, so you might create extra costs with a lot of indices or records. Algolias free community plan includes 10.000 records and 100.000 operations and you have to display their logo on your search results. Details [here](https://www.algolia.com/pricing)

Algolia is a hosted full-text, numerical, and faceted search engine capable of delivering realtime results from the first keystroke. Algolia’s powerful API lets you quickly and seamlessly implement search within your websites and mobile applications. The search API powers billions of queries for thousands of companies every month, delivering relevant results in under 100ms anywhere in the world.

## Initial Setup

To get started with Algolia you first need an [Algolia Account](https://www.algolia.com/users/sign_up). In the next step you log into your GraphCMS project dashboard and choose "Integrations" in the left menu bar. There you select "Algolia".

![Algolia Integrations View](../img/integrations/integration_view.png)

You will be prompted with your Algolia `Application ID` and `API Key`. 

![Algolia Prompt](../img/integrations/algolia_prompt.png)

To retrieve them you switch back to your Algolia dashboard and select API Keys on the menu. The first entry there is your `Application ID`. 

Next we need to create an `API Key`. Click on *"All API Keys"* and then *"New API Key"*.
![Algolia API Keys](../img/integrations/algolia_api_keys.png)

The new API Key needs to have *"Add records"*, *"Delete Records"* and *"Delete Index"* checked.
![Create New API Key](../img/integrations/algolia_create_key.png)

Now you can copy & paste both the `Application ID` and `API Key` in the GraphCMS prompt and click "Add".

## Creating Indices

After you entered your credentials you will be redirected to the Algolia configuration. From here you can now create indices for Algolia.

!!! Hint ""
    The Index will only get created if you have data in your content model. If you create an index for a content model without content, the index will not be created until you add content to it.

![IMAGE ALT TEXT HERE](https://media.giphy.com/media/xT1Ra3NA1YjMQMpB4Y/giphy.gif)

After you added your indices they should become visible in the Algolia dashboard. Whenever you create, edit or delete content for the indexed model Algolia will update as well.

## Demo Application

Now you are ready to integrate Algolia in your frontend. We prepared a small demo application with create-react-app over at [Github](https://github.com/belazer/graphcms-algolia) with a live demo [here](https://graphcms-algolia.netlify.com)

## Limitations

The initial index of your content entries only allows 1000 entries. If you try to index more in the initial index, you will need to update your indices manually.