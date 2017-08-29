!!! Hint ""
    The maximum accepted image size is 100.000.000 pixels. An image with this many pixels could have width and height combinations like 10.000 x 10.000 or 5.000 x 20.000, etc. The maximum file size of an image can not be larger than 256mb.

```
Pixelate Faces URL Format:
https://media.graphcms.com/pixelate_faces=[options]/[File Handle]
```
<!-- -->
> `pixelate_faces=faces:1, [1,2,3,4], or all`
> 
> > Can be abbreviated as `f:all`
> > 
> > The faces parameter can be a single face object, an array of face objects, or you can use 'all' to choose all the detected face objects. This parameter governs the faces you want pixelated.

<!-- -->
> `pixelate_faces=minsize:N`
> 
> > Can be abbreviated as `n:.35`
> > 
> > Defaults to 0.35. This parameter is used to weed out objects that most likely are not faces. This can be an integer or a float in a range from 0.01 to 10000. If the value set is larger than 1, it is used as the expected minimum size of face objects in pixels. So for example if the value is set to 200, then the smallest face object it will detect is 200x200 pixels. If the value that is set is less than 1, i.e. 0.3, then it filters out all face objects it detects that are smaller than the middle/average object size by 30%. This means that if a set of 3 objects are detected that are (size in pixels) ['100x100', '200x200', '300x300'], then setting minsize to 0.3 will result in a calculation where the middle object size 200 is used: 200-0.30*200=140 and all objects smaller than 140x140 would be eliminated.

<!-- -->
> `pixelate_faces=maxsize: X`
> 
> > Can be abbreviated as `x:.35`
> > 
> > Defaults to 0.35. This parameter is used to weed out objects that most likely are not faces. This can be an integer or a float in a range from 0.01 to 10000. If the value set is larger than 1, it is used as the expected maximum size of face objects in pixels. So for example if the value is set to 200, then the largest face object it will detect is 200x200 pixels. If the value that is set is less than 1, i.e. 0.3, then it filters out all face objects it detects that are larger than the middle/average object size by 30%. This means that if a set of 3 objects are detected that are (size in pixels) ['100x100', '200x200', '300x300'], then setting maxsize to 0.3 will result in a calculation where the middle object size 200 is used: 200+0.30*200=260 and all objects larger than 260x260 would be eliminated.

<!-- -->
> `pixelate_faces=buffer:200`
> 
> > Can be abbreviated as `b:200`
> > 
> > Adjusts the buffer around the face object as a percentage of the original object. Must be an integer in a range from 0 to 1000. There is no default value.

<!-- -->
> `pixelate_faces=amount:5`
> 
> > Can be abbreviated as `a:10`
> > 
> > The amount of pixelation to apply to the selected faces. The value for this parameter can be any integer in a range from 2 to 100. The default value for this parameter is 10.

<!-- -->
> `pixelate_faces=blur:10`
> 
> > Can be abbreviated as `l:4.0`
> > 
> > The amount to blur the pixelated faces. The value for this parameter can be any float in a range from 0 to 20. The default value for this parameter is 4.0.

<!-- -->
> `pixelate_faces=type:rect`
> 
> > Can be abbreviated as `t:rect`
> > 
> > The shape of the pixelated faces. The value for this parameter is a string. The options are rect (for a rectangle shape) or oval (for and oval shape). The default value for this parameter is rect.


### Pixelate Example

>Original Image: [https://media.graphcms.com/resize=w:500/v7SJmanPThaNnkj0Xujt](https://media.graphcms.com/resize=w:500/v7SJmanPThaNnkj0Xujt)

>![Original Image](https://media.graphcms.com/resize=w:500/v7SJmanPThaNnkj0Xujt)

>Pixelate Faces Image: [https://media.graphcms.com/pixelate_faces=faces:all,amount:10,type:oval,buffer:120,blur:14/resize=w:500/v7SJmanPThaNnkj0Xujt](https://media.graphcms.com/pixelate_faces=faces:all,amount:10,type:oval,buffer:120,blur:14/resize=w:500/v7SJmanPThaNnkj0Xujt)

>![Pixelate Faces](https://media.graphcms.com/pixelate_faces=faces:all,amount:10,type:oval,buffer:120,blur:14/resize=w:500/v7SJmanPThaNnkj0Xujt)