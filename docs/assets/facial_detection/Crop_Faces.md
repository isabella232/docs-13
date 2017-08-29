!!! Hint ""
    The maximum accepted image size is 100.000.000 pixels. An image with this many pixels could have width and height combinations like 10.000 x 10.000 or 5.000 x 20.000, etc. The maximum file size of an image can not be larger than 256mb.

```
Crop Faces URL Format:
https://media.graphcms.com/crop_faces=[options]/[File Handle]
```
<!-- -->
> `crop_faces=mode:thumb, crop or fill`
> 
> > Can be abbreviated as `m:thumb`
> > 
> > `mode:thumb`: Adds a buffer around the face object by default. Part of the image inside the buffer is always resized to the desired dimensions (w & h parameters).

> > `mode:crop`: Ignores the buffer around the face object.

> > `mode:fill`: Works in a similar manner to thumb mode, except that while `thumb` mode will always crop an image to fit in the whole face object while still trying to respect width and height transformations, `fill` mode will prioritze width and height parameters over displaying the whole face object.

<!-- -->
> `crop_faces=width:100`
> 
> > Can be abbreviated as `w:100`
> > 
> > Specify the width of the crop in pixels.

<!-- -->
> `crop_faces=height:100`
> 
> > Can be abbreviated as `h:100`
> > 
> > Specify the height of the crop in pixels.

<!-- -->
> `crop_faces=faces:1, [1,2,3,4], or all`
> 
> > Can be abbreviated as `f:1`
> > 
> > The faces parameter can be a single face object, an array of face objects, or you can use 'all' to choose all the detected face objects. This parameter governs the faces you want included in your crop.

<!-- -->
> `crop_faces=buffer:200`
> 
> > Can be abbreviated as `b:1`
> > 
> > Adjusts the buffer around the face object as a percentage of the original object.

### Crop Faces Example

>Original Image: [https://media.graphcms.com/resize=w:500/v7SJmanPThaNnkj0Xujt](https://media.graphcms.com/resize=w:500/v7SJmanPThaNnkj0Xujt)

>![Original Image](https://media.graphcms.com/resize=w:500/v7SJmanPThaNnkj0Xujt)

>Crop Faces Image: [https://media.graphcms.com/crop_faces=mode:thumb,faces:5/resize=w:250/v7SJmanPThaNnkj0Xujt](https://media.graphcms.com/crop_faces=mode:thumb,faces:5/resize=w:250/v7SJmanPThaNnkj0Xujt)

>![Crop Faces](https://media.graphcms.com/crop_faces=mode:thumb,faces:5/resize=w:250/v7SJmanPThaNnkj0Xujt)

>Crop 3 Faces Image: [https://media.graphcms.com/crop_faces=mode:thumb,faces:[1,2,3]/resize=w:250/v7SJmanPThaNnkj0Xujt](https://media.graphcms.com/crop_faces=mode:thumb,faces:[1,2,3]/resize=w:250/v7SJmanPThaNnkj0Xujt)

>![Crop 3 Faces](https://media.graphcms.com/crop_faces=mode:thumb,faces:[1,2,3]/resize=w:250/v7SJmanPThaNnkj0Xujt)