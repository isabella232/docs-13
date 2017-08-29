## Format Conversions and Quality Transformations

You can convert many document types to images and vice versa. The quality and compression of an image may also be manipulated.

!!! Hint ""
    The maximum accepted image size is 100.000.000 pixels. An image with this many pixels could have width and height combinations like 10.000 x 10.000 or 5.000 x 20.000, etc. The maximum file size of an image can not be larger than 256mb.

```
Output URL Format:
https://media.graphcms.com/output=[options]/[File Handle]
```
<!-- -->
> `output=format:file type`
> 
> > Can be abbreviated as `f:png`
> > 
> > The format to which you would like to convert the file.
For a complete list of possible conversions, see the table below.

<!-- -->
> `output=background:white or FFFFFFFF`
> 
> > Can be abbreviated as `b:ff0000`
> > 
> > Set a background color when converting transparent .png files into other file types. The default background when converting a transparent png file is white. The background parameter allows you to control the background color of the returned file when used in the following way: `output=format:jpg,background:red`. The background value can be the name of a color or a hex color code. Click here for a [list of accepted color names](https://www.filestack.com/docs/image-transformations/colors)

<!-- -->
> `output=page:1 to 10000`
> 
> > Can be abbreviated as `p:4`
> > 
> > If you are converting a file that contains multiple pages such as a PDF or powerpoint file, you can extract a specific page using the page parameter. The value for the page parameter must be an integer between 1 and 10000, the default is 1.

<!-- -->
> `output=density:1 to 500`
> 
> > Can be abbreviated as `d:50`
> > 
> > You can adjust the density when converting documents like PowerPoint, PDF, AI and EPS files to image formats like JPG or PNG. This can improve the resolution of the output image. The value for the density parameter must be an integer between 1 and 500.

<!-- -->
> `output=compress:true`
> 
> > Can be abbreviated as `c:true`
> > 
> > You can take advantage of Filestack's image compression which utilizes JPEGtran and OptiPNG. The value for this parameter is boolean. If you want to compress your image then the value should be `compress:true`. Compression is off/false by default.

<!-- -->
> `output=quality:1 to 100 or input`
> 
> > Can be abbreviated as `q:80`
> > 
> > You can change the quality (and reduce the file size) of JPEG images by using the quality parameter. The value for this parameter must be an integer between 1 and 100. The quality is set to 100 by default. A quality setting of 80 provides a nice balance between file size reduction and image quality. If the quality is instead set to "input" the image will not be recompressed and the input compression level of the jpg will be used.

<!-- -->
> `output=strip:true`
> 
> > Can be abbreviated as `t:true`
> > 
> > This parameter will remove any metadata embedded in an image.

<!-- -->
> `no_metadata`
> 
> > The root task cannot be abbreviated
> > 
> > This alternative method for stripping metadata from an image is not part of the output task, and is a task all by itself. It will remove any metadata embedded in an image.

<!-- -->
> `output=colorspace:RGB, CMYK or Input`
> 
> > Can be abbreviated as `o:input`
> > 
> > By default we convert all the images to the RGB color model in order to be web friendly. However, we have added an option to preserve the original colorspace by appending `output=colorspace:input`, or switch the colorspace by using `output=colorspace:RGB` or `output=colorspace:CMYK`. This will work for JPEGs and TIFFs.

<!-- -->
> `output=secure:true`
> 
> > Can be abbreviated as `s:true`
> > 
> > This parameter applies to conversions of HTML and SVG sources. When the secure parameter is set to true, the HTML or SVG file will be stripped of any insecure tags (HTML sanitization). The default setting for the secure parameter is false.

<!-- -->
> `output=docinfo:true`
> 
> > Can be abbreviated as `i:true`
> > 
> > The docinfo parameter can be used to get information about a document, such as the number of pages and the dimensions of the file. This information is delivered as a JSON object that will look like this: `{"numpages":41,"dimensions":{"width":538,"height":718}}`. The value for this parameter is boolean and the default setting for the info parameter is false.

<!-- -->
> `output=pageformat:letter`
> 
> > Can be abbreviated as `a:legal`
> > 
> > The pageformat parameter can be used to set the page size used for the layout of the resultant document. This parameter can be used when converting the format of one document into PDF, PNG, or JPG. Possible values are: `a3, A3 ,a4, A4, a5, A5, b4, B4, b5, B5, letter,legal, tabloid´


<!-- -->
> `output=pageorientation:portrait or landscape`
> 
> > Can be abbreviated as `r:landscape`
> > 
> > The pageorientation parameter can be used to determine the orientation of the resultant document. This parameter can be used when converting the format of one document into PDF, PNG, or JPG. Possible values are `landscape` and `portrait`.

## List of Possible Format Conversions by Document Type

| Original Format | Possible File Type Conversions |
| --------------- | ------------------------------ |
|PDF              | jpg, odp, ods, odt, png, svg, txt, and webp |
|DOC	          |docx, html, jpg, odt, pdf, png, svg, txt, and webp |
|DOCX	          |doc, html, jpg, odt, pdf, png, svg, txt, and webp|
|ODT	          |doc, docx, html, jpg, pdf, png, svg, txt, and webp|
|XLS	          |jpg, pdf, ods, png, svg, xlsx, and webp|
|XLSX	          |jpg, pdf, ods, png, svg, xls, and webp|
|ODS	          |jpg, pdf, png, xls, svg, xlsx, and webp|
|PPT	          |jpg, odp, pdf, png, svg, pptx, and webp|
|PPTX	          |jpg, odp, pdf, png, svg, ppt, and webp|
|ODP	          |jpg, pdf, png, ppt, svg, pptx, and webp|
|BMP	          |jpg, odp, ods, odt, pdf, png, svg, and webp|
|GIF	          |jpg, odp, ods, odt, pdf, png, svg, and webp|
|JPG	          |jpg, odp, ods, odt, pdf, png, svg, and webp|
|PNG	          |jpg, odp, ods, odt, pdf, png, svg, and webp|
|WEBP	          |jpg, odp, ods, odt, pdf, png, svg, and webp|
|TIFF	          |jpg, odp, ods, odt, pdf, png, svg, and webp|
|AI	              |jpg, odp, ods, odt, pdf, png, svg, and webp|
|PSD	          |jpg, odp, ods, odt, pdf, png, svg, and webp|
|SVG	          |jpg, odp, ods, odt, pdf, png, and webp|
|HTML	          |jpg, odt, pdf, svg, txt, and webp|
|TXT	          |jpg, html, odt, pdf, svg, and webp|


## Output Example

>Original Image: [https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B](https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B)

>![Original Image](https://media.graphcms.com/resize=w:500/bXa8m7oYQdmyWpmo2I2B)

>Output Image: [https://media.graphcms.com/resize=w:500/output=format:png/bXa8m7oYQdmyWpmo2I2B](https://media.graphcms.com/resize=w:500/output=format:png/bXa8m7oYQdmyWpmo2I2B)

>![Output Image](https://media.graphcms.com/resize=w:500/output=format:png/bXa8m7oYQdmyWpmo2I2B)