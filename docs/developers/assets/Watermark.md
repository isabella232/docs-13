You can add watermarks to images by overlaying another image on top of your main image.

!!! Hint ""
    The maximum accepted image size is 100.000.000 pixels. An image with this many pixels could have width and height combinations like 10.000 x 10.000 or 5.000 x 20.000, etc. The maximum file size of an image can not be larger than 256mb.

```
Watermark Task URL Formats:
https://media.graphcms.com/watermark=[options]/[File Handle]
```
<!-- -->
> `watermark=file:VsXhJVWReSrYU7UoFyfw`
>
> > Can be abbreviated as `f:VsXhJVWReSrYU7UoFyfw`
> > 
> > The file handle of the image that you want to layer on top of another image as a watermark.

<!-- -->
> `watermark=size:75`
>
> > Can be abbreviated as `s:75`
> > 
> > The size of the overlayed image as a percentage of its original size. The value must be an integer between 1 and 500.

<!-- -->
> `watermark=position:top, middle, bottom, left, center, or right`
>
> > Can be abbreviated as `p:top`
> > 
> > The position of the overlayed image. Possible values for position are: top, middle, bottom, left, center, and right. These values can be paired as well like `position:[top,right]`.

### Watermark Example

>Original Image: [https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B](https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B)

>![Original Image](https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B)

>Watermarked Image: [https://media.graphcms.com/resize=w:500/watermark=file:7FlqIg31QFCbrNcLmiNQ,size:25,position:[top,left]/bXa8m7oYQdmyWpmo2I2B](https://media.graphcms.com/resize=w:500/watermark=file:7FlqIg31QFCbrNcLmiNQ,size:25,position:[top,left]/bXa8m7oYQdmyWpmo2I2B)

>![Watermarked Image](https://media.graphcms.com/resize=w:500/watermark=file:7FlqIg31QFCbrNcLmiNQ,size:25,position:[top,left]/bXa8m7oYQdmyWpmo2I2B)