# Media

Working with media is an essential part of content management. GraphCMS allows you to upload and assign media files to any content model in your project. The uploaded files will be assigned to the project´s internal `Media` model. `Media fields` will then connect the dots between a content model and assigned files.

Media files that are stored in GraphCMS are served via a global content delivery network. This assures quick response times and fast loading of your media.

![Screenshot](img/animated/mediaScreenCapture.gif)

## Querying a Media Field

Uploaded media files can be queried from your content API. Let´s assume we have a content model `Post`, with a field configuration of:

* Title `#title` `Single Line Text`
* Images `#images` `Media` `Allow Multiple Values`

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

Field `#images` will return an array of media objects, which contain the fields:

* `fileName` the original file name of the uploaded file
* `handle` the identifier of the uploaded file
* `url` the full URL to the uploaded file
* `mimeType` the media type of the uploaded file
* `size` the total size in bytes

!!! Hint ""
    If you know the desired display size of your images, it is highly recommended to use our integrated image transformation engine to scale your images to the desired size. This will increase the loading time of your content and result in a better user experience for your visitors.

## GraphCMS Image Transformations

An essential feature of GraphCMS is the image processing engine. It enables you on the fly image transformations, such as resizing or cropping just by adding parameters to your media´s URL.

### Transformation URL Structure with GraphCMS Handle

```
https://media.graphcms.com/[(1) Transformation Tasks]/[(2) File Handle]
```

1) One or multiple transformation tasks and their parameters

2) The source image to run the transformation tasks on.

### Available Transformations

An overview of supported image transformations.

!!! Hint ""
    The maximum accepted image size is 100.000.000 pixels. An image with this many pixels could have width and height combinations like 10.000 x 10.000 or 5.000 x 20.000, etc. The maximum file size of an image can not be larger than 256mb.

#### Resize Fit and Align

The resizing feature comprises two main functions, manipulating the width and height of an image and changing the fit and alignment of the image.

```
Resize Task URL Format:
https://media.graphcms.com/resize=[options]/[File Handle]
```

> Basic resize example with a GraphCMS file handle
>
>[https://media.graphcms.com/resize=width:300/bYbYJmGyQfynUBaBkRnP](https://media.graphcms.com/resize=width:300/bYbYJmGyQfynUBaBkRnP)
![TransformedImage](https://media.graphcms.com/resize=width:300/bYbYJmGyQfynUBaBkRnP)

<!-- -->
> `resize=width:100`
>
> > Can be abbreviated as `w:100`
> >
> > The width in pixels to resize the image to. The value must be an integer from 1 to 10000.

<!-- -->
> `resize=height:100`
>
> > Can be abbreviated as `h:100`
> >
> > The height in pixels to resize the image to. The value must be an integer from 1 to 10000.

<!-- -->
> `resize=fit:clip, crop, scale, or max`
>
> > Can be abbreviated as `f:clip`
> >
> > The default value for the fit parameter is `fit:clip`.
>
> `fit:clip`
> > Resizes the image to fit within the specified parameters without distorting, cropping, or changing the aspect ratio.
>
> `fit:crop`
> > Resizes the image to fit the specified parameters exactly by removing any parts of the image that don't fit within the boundaries.
>
> `fit:scale`
> > Resizes the image to fit the specified parameters exactly by scaling the image to the desired size. The aspect ratio of the image is not respected and the image can be distorted using this method.
>
> `fit:max`
> > Resizes the image to fit within the parameters, but as opposed to 'fit:clip' will not scale the image if the image is smaller than the output size.

<!-- -->
> `resize=align:center, top, bottom, left, right, or faces`
>
> > Can be abbreviated as `a:top`
> >
> > Using align, you can choose the area of the image to focus on. Possible values:
> > `align:center` `align:top` `align:bottom` `align:left` `align:right` `align:faces`
> >
> > By default, it crops from the center.
You can also specify pairs e.g. align:[top,left]. Center cannot be used in pairs.

<!-- -->
> Example with `resize` + `fit:crop`
>
>[https://media.graphcms.com/resize=w:300,h:300,fit:crop/bYbYJmGyQfynUBaBkRnP](https://media.graphcms.com/resize=w:300,h:300,fit:crop/bYbYJmGyQfynUBaBkRnP)
![TransformedImage](https://media.graphcms.com/resize=w:300,h:300,fit:crop/bYbYJmGyQfynUBaBkRnP)

#### Crop

Cropping is done by entering coordinates for where the boundary of the crop will be.

```
Crop Task URL Format:
https://media.graphcms.com/crop=[options]/[File Handle]
```

> `crop=dim:[x,y,width,height]`
>
> > Can be abbreviated as `d:[10,20,200,250]`
> >
> > Crops the image to a specified rectangle.
> >
> > The input for this parameter must be exactly 4 integers: `x coordinate, y coordinate, width, height`. For example, an input of `crop=dim:[10,20,200,250]` selects a 200x250 pixel rectangle starting 10 pixels from the left edge of the image and 20 pixels from the top edge of the image.

##### How cropping works

The X and Y coordinates start from [0,0] and correspond to the top left hand corner of the image to be cropped. The Width and Height parameters dictate the size in pixels of the cropping rectangle once the point to start the crop at is selected by the X and Y coordinates. If you set coordinates that create a crop area that extends outside the frame of the image, then you will only receive back the part of the image that is within the crop area.

<!-- -->
> Cropping Example
>
> Original Image:
>[https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B](https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B)
>
>![TransformedImage](https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B)
>
> Cropped Image
>[https://media.graphcms.com/crop=dim:[2000,900,1800,1300]/resize=w:500/bXa8m7oYQdmyWpmo2I2B](https://media.graphcms.com/crop=dim:[2000,900,1800,1300]/resize=w:500/bXa8m7oYQdmyWpmo2I2B)
>
>
![TransformedImage](https://media.graphcms.com/crop=dim:[2000,900,1800,1300]/resize=w:500/bXa8m7oYQdmyWpmo2I2B)
### Chaining Transformations

The transformation engine also supports task chaining. This allows you to apply multiple transformations on an image. This is simply done by seperating transformations with a `/`.

```
URL format for chaining:
https://media.graphcms.com/[task]=[options]/[task]=[options]/[File Handle]
```
