Cropping is done by entering coordinates for where the boundary of the crop will be.

!!! Hint ""
    The maximum accepted image size is 100.000.000 pixels. An image with this many pixels could have width and height combinations like 10.000 x 10.000 or 5.000 x 20.000, etc. The maximum file size of an image can not be larger than 256mb.

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

### How cropping works

The X and Y coordinates start from [0,0] and correspond to the top left hand corner of the image to be cropped. The Width and Height parameters dictate the size in pixels of the cropping rectangle once the point to start the crop at is selected by the X and Y coordinates. If you set coordinates that create a crop area that extends outside the frame of the image, then you will only receive back the part of the image that is within the crop area.

<!-- -->
> Cropping Example
>
> Original Image:
>[https://media.graphcms.com/resize=w:500/IvwxgEnhSOWuQsVxvvnc](https://media.graphcms.com/resize=w:500/IvwxgEnhSOWuQsVxvvnc)
>
>![TransformedImage](https://media.graphcms.com/resize=w:500/IvwxgEnhSOWuQsVxvvnc)
>
> Cropped Image
>[https://media.graphcms.com/crop=dim:[2000,900,1800,1300]/resize=w:500/IvwxgEnhSOWuQsVxvvnc](https://media.graphcms.com/crop=dim:[2000,900,1800,1300]/resize=w:500/IvwxgEnhSOWuQsVxvvnc)
>
>
![TransformedImage](https://media.graphcms.com/crop=dim:[2000,900,1800,1300]/resize=w:500/IvwxgEnhSOWuQsVxvvnc)