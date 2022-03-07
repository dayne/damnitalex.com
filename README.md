damnitalex.com
==============

damnitalex.com

### This to do ###
- [ ] add keyboard left/right arrow navigation
- [ ] add tags to photos and allow navigation based on tags


### Images

The target image size is: 940x500

```
convert orig-image -resize "940x500>" -size 940x500 \
        xc:black +swap -gravity center  -composite fixed_up_image.jpg
```
