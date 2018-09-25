# Optimize your JPG files for web delivery

Set the quality of your JPG or WEBP images without the danger of possibly generating a larger file. This is great for reducing the file size of your image before running it through the compress task. Quality is a standalaone task which is separate from the quality parameter available in the output task, and operates differently in that it will never increase the size of a file.

!!! Hint ""
    The maximum accepted image size is 100.000.000 pixels. An image with this many pixels could have width and height combinations like 10.000 x 10.000 or 5.000 x 20.000, etc. The maximum file size of an image can not be larger than 256mb.

```
Quality Task URL Format:
https://media.graphcms.com/quality=[options]/[File Handle]
```

> `quality=value:75`
> 
> > Can be abbreviated as `v:75`
> > 
> > This task will take a JPG or WEBP file and reduce the file size of the image by reducing the quality. If the file is not a JPG, the original file will be returned. If after the conversion, the resulting file is not smaller than the original, the original file will be returned. Using quality as a seperate task is better when no previous image manipulations (no previous recompressions) have been done. `quality=value:75`.

### Quality Examples

>Original Image: [https://media.graphcms.com/resize=w:500/IvwxgEnhSOWuQsVxvvnc](https://media.graphcms.com/resize=w:500/IvwxgEnhSOWuQsVxvvnc)

>![Original Image](https://media.graphcms.com/resize=w:500/IvwxgEnhSOWuQsVxvvnc)

>Change Quality Image: [https://media.graphcms.com/resize=w:500/quality=value:70/compress/IvwxgEnhSOWuQsVxvvnc](https://media.graphcms.com/resize=w:500/quality=value:70/compress/IvwxgEnhSOWuQsVxvvnc)

>![Change Quality Image](https://media.graphcms.com/resize=w:500/quality=value:70/compress/IvwxgEnhSOWuQsVxvvnc)