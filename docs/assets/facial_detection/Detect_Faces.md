GraphCMS offers an accurate and versatile facial detection feature. The below will illustrate how to use the function to detect faces and crop pictures automatically based on the face detection information.

The feature is divided into four endpoints, one for detection, one for cropping and two filters. We will discuss the detection first.

```
Facial Detection URL Format:
https://media.graphcms.com/detect_faces=[options]/[File Handle]
```
<!-- -->
> `detect_faces`
>
> > The root task cannot be abbreviated
> >
> > Users can use `detect_faces` without any options and the default settings will be used.

<!-- -->
> `detect_faces=maxsize:N`
>
> > Can be abbreviated as `n:.35`
> >
> > This parameter is used to weed out objects that most likely are not faces. This can be an integer or a float in a range from 0.01 to 10000. The default value is 0.35. If the value set is larger than 1, it is used as the expected minimum size of face objects in pixels. So for example if the value is set to 200, then the smallest face object it will detect is 200x200 pixels. If the value that is set is less than 1, i.e. 0.3, then it filters out all face objects it detects that are smaller than the middle/average object size by 30%. This means that if a set of 3 objects are detected that are (size in pixels) ['100x100', '200x200', '300x300'], then setting minsize to 0.3 will result in a calculation where the middle object size 200 is used: 200-0.30*200=140 and all objects smaller than 140x140 would be eliminated.

<!-- -->
> `detect_faces=maxsize:X`
>
> > Can be abbreviated as `x:.35`
> >
> > This parameter is used to weed out objects that most likely are not faces. This can be an integer or a float in a range from 0.01 to 10000. The default value is 0.35. If the value set is larger than 1, it is used as the expected maximum size of face objects in pixels. So for example if the value is set to 200, then the largest face object it will detect is 200x200 pixels. If the value that is set is less than 1, i.e. 0.3, then it filters out all face objects it detects that are larger than the middle/average object size by 30%. This means that if a set of 3 objects are detected that are (size in pixels) ['100x100', '200x200', '300x300'], then setting maxsize to 0.3 will result in a calculation where the middle object size 200 is used: 200+0.30*200=260 and all objects larger than 260x260 would be eliminated.

<!-- -->
> `detect_faces=color:white or FFFFFFF`
>
> > Can be abbreviated as `c:white or FFFFFFFF`
> >
> > Will change the color of the "face object" boxes and text. This can be the word for a color, or the hex color code. Click here for a [list of accepted colors](https://www.filestack.com/docs/image-transformations/colors).

<!-- -->
> `detect_faces=export:true`
>
> > Can be abbreviated as `e:true`
> >
> > Will export all face objects to a JSON object.

### Detection Example

>Original Image: [https://media.graphcms.com/resize=w:500/v7SJmanPThaNnkj0Xujt](https://media.graphcms.com/resize=w:500/v7SJmanPThaNnkj0Xujt)

>![Original Image](https://media.graphcms.com/resize=w:500/v7SJmanPThaNnkj0Xujt)

>Detected Faces Image: [https://media.graphcms.com/detect_faces/resize=w:500/v7SJmanPThaNnkj0Xujt](https://media.graphcms.com/detect_faces/resize=w:500/v7SJmanPThaNnkj0Xujt)

>![Detected Faces](https://media.graphcms.com/detect_faces/resize=w:500/v7SJmanPThaNnkj0Xujt)