!!! Hint ""
    The maximum accepted image size is 100.000.000 pixels. An image with this many pixels could have width and height combinations like 10.000 x 10.000 or 5.000 x 20.000, etc. The maximum file size of an image can not be larger than 256mb.

Uploaded asset files can be queried from your content API. Let´s assume we have a content model `Post`, with a field configuration of:

* Title `#title` `Single Line Text`
* Images `#images` `Assets` `Allow Multiple Values`

This will allow us to store an arbitrary number of images for a `Post`.

We can fetch `allPosts` with the following query:

```
{
  allPosts {
    title
    images {
      fileName
      handle
      url
      mimeType
      size
    }
  }
}
```

Which will give us a result like this:

```
{
  "data": {
    "allPosts": [
      {
        "title": "Some nice post",
        "images": [
          {
            "fileName": "pexels-photo-295821.jpeg",
            "handle": "7AmzJToStuJrNqkpPSWO"
            "url": "https://media.graphcms.com/7AmzJToStuJrNqkpPSWO",
            "mimeType": "image/jpeg",
            "size": 100075,
          }
        ]
      }
    ]
  }
}
```

Field `#images` will return an array of asset objects, which contain the fields:

* `fileName` the original file name of the uploaded file
* `handle` the identifier of the uploaded file
* `url` the full URL to the uploaded file
* `mimeType` the asset type of the uploaded file
* `size` the total size in bytes

!!! Hint ""
    If you know the desired display size of your images, it is highly recommended to use our integrated image transformation engine to scale your images to the desired size. This will decrease the loading time of your content and result in a better user experience for your visitors.