You can rotate your image in a range clockwise from 0 degrees (no rotation) to 359 (nearly all the way around). You can also mirror an image horizontally or vertically. For more information see below.

```
Rotate Task URL Formats:
https://media.graphcms.com/rotate=[options]/[File Handle]
```
<!-- -->
> `rotate=deg:0 to 359 or exif`
>
> > Can be abbreviated as `d:90`
> > 
> > The degree by which to rotate the image clockwise. This can be an integer in a range from 0 to 359. Alternatively, you can set the degree to 'exif' and the image will be rotated based upon any exif metadata it may contain.

<!-- -->
> `rotate=exif:false`
>
> > Can be abbreviated as `e:false`
> >
> > Sets the EXIF orientation of the image to EXIF orientation 1. The exif=false parameter takes an image and sets the exif orientation to the first of the eight EXIF orientations. The image will behave as though it is contained in an html img tag if displayed in an application that supports EXIF orientations. Read more about EXIF rotation and associated issues.

<!-- -->
> `rotate=background:white or FFFFFFFF`
>
> > Can be abbreviated as `b:black`
> >
> > Sets the background color to display if the image is rotated less than a full 90 degrees. This can be the word for a color, or the hex color code. [Accepted Colors](https://www.filestack.com/docs/image-transformations/colors)

### Rotate Example

>Original Image: [https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B](https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B)

>![Original Image](https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B)

>Rotated Image: [https://media.graphcms.com/resize=w:500/rotate=deg:180/bXa8m7oYQdmyWpmo2I2B](https://media.graphcms.com/resize=w:500/rotate=deg:180/bXa8m7oYQdmyWpmo2I2B)

>![Rotated Image](https://media.graphcms.com/resize=w:500/rotate=deg:180/bXa8m7oYQdmyWpmo2I2B)

>Rotated with Background: [https://media.graphcms.com/resize=w:500/rotate=deg:145,background:black/bXa8m7oYQdmyWpmo2I2B](https://media.graphcms.com/resize=w:500/rotate=deg:145,background:black/bXa8m7oYQdmyWpmo2I2B)

>![Rotated Image with Background](https://media.graphcms.com/resize=w:500/rotate=deg:145,background:black/bXa8m7oYQdmyWpmo2I2B)