GraphCMS now offers a standalone task specifically for compression. It utilizes mozjpeg to offer improved compression for jpgs over the algorithm used for output=compress:true. It will not attempt to re-compress a previously compressed image. Compress will only work with png and jpg files. If any other format is passed in, it will return an unchanged image. For the best results, compress should be the last task in your transformation chain.

!!! Hint ""
    The maximum accepted image size is 100.000.000 pixels. An image with this many pixels could have width and height combinations like 10.000 x 10.000 or 5.000 x 20.000, etc. The maximum file size of an image can not be larger than 256mb.

```
Compress URL Format:
https://media.graphcms.com/compress=[options]/[File Handle]
```
<!-- -->
> `compress`
> 
> > The root task cannot be abbreviated
> > 
> > Users can use `compress` without any options and the default settings will be used.

<!-- -->
> `compress=metadata:true`
> 
> > Can be abbreviated as `m:true`
> > 
> > By default the compress task will strip photo metadata out of the image to reduce the file size. If you need to maintain the file metadata, you can pass `metadata:true` in order to prevent the metadata from being removed.

### Compress Example

>Original Image: [https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B](https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B)

>![Original Image](https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B)

>Compressed Image: [https://media.graphcms.com/resize=w:500/compress/bXa8m7oYQdmyWpmo2I2B](https://media.graphcms.com/resize=w:500/compress/bXa8m7oYQdmyWpmo2I2B)

>![Compressed Image](https://media.graphcms.com/resize=w:500/compress/bXa8m7oYQdmyWpmo2I2B)