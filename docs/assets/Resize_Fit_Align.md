!!! Hint ""
    The maximum accepted image size is 100.000.000 pixels. An image with this many pixels could have width and height combinations like 10.000 x 10.000 or 5.000 x 20.000, etc. The maximum file size of an image can not be larger than 256mb.

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