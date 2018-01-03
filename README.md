https://jasenpan1987.github.io/natours/

# Some Notes
## Centering an element
```
parent {
  position: relative;
}

child {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

## key to make good css
- Responsive, maintainable code, web performance

How css resolve conflict
1. Inportance:
   User !important > Author !important > user > author > browser default
2. Same importance higher specificity
Inline style > id > class, pseudo class, attr > element, pseudo elements
3. Same importance, same specificity, the later wins, so put your css after all third party css


example:
```
example: 
.button { // 0 (inline), 0 (id), 1(class), 0(element)
  color: red;
}

nav#nav div.pull-right .button { // 0, 1, 2, 2
  color: yellow;
}

a { // 0, 0, 0, 1
  color: green;
}

#nav a.button:hover { // 0, 1, 2, 1
  color: white;
}

0010
0122 <- win
0001
0121
```

## How Css value are processed
```
html, body {
  font-size: 16px;
  width: 80vw; // based on browser’s view port width
}

header {
  font-size: 150%; // reference to parent 16 * 1.5 = 24px
        /*here is different, the padding here is 24 * 2 = 48px*/
  /*for font, the ref is parent, for length/width, the ref is the current element*/
  padding: 2em; 
  margin-bottom: 10rem; // ref to root 16 * 10 = 160px
  height: 90vh;// based on browser’s view port height
  width: 1000px;
}

.header-child {
  font-size: 3em;// ref to parent 3 * 24 = 72
  padding: 10%; // reference to parent 1000 * 10 = 100px
}
```

## visual formatting model render page

1. Box-model
Content -> padding -> border -> margin (outside boxes) 
| ————— fill-area ————|

Width = content width + left border + right border + left padding + right padding
Height = content height + top border + bottom border + top padding + bottom padding

Box-sizeing: border-box; => border and padding will be include in the total width

2. block, inline, inline-block
block:
default: display block
100% parent width
Vertically one after another
Box-model applied

Inline:
Default: inline
Occupy only its content space
No line-breaks
No height and width
Padding and margin only apply to left and right
No box-model applied

Inline-block:
Default: inline-block
Occupy only is content space
No line-breaks
Box-model applied

3. Position
### Normal float:
- Default: position relative
- Default position scheme
- Not floated
- Not absolute positioned
- Element layout to their source order

### Floats:
- Default position float(left, right)
- Element is removed from the normal flow
- Text and inline elements will be wrapped around the flow element
- The container will not adjust its height to the element

### Absolute
- Default: position absolute
- Element is removed from the normal flow
- No impact on surrounding elements or content
- We use top, left, right, bottom to position it to its parent relative container

4. Stacking context
Z-index, the higher the more top among others
Opacity, transform, filter can also create stacking context

## Css architecture 
- 7-1 structure
 
 +base/
 
 +components/
 
 +layout/
 
 +pages/
 
 +themes/
 
 +abstract/
 
 +vendors/

-main.scss

## Sass demos:
https://codepen.io/anon/pen/zpqoVx?editors=1100

## Layout type
1. Float Layout
2. Flex-box
3. Css-grid

Float Layout:
Row and col system
```
  // this * will select every single element that has an attribute of class CONTAINS "col-"
[class*="col-“]{}

  // this $ will select every single element that has an attribute of class END "col-"
 [class$="col-“]{}

  // this ^ will select every single element that has an attribute of class START with "col-"
  [class^="col-"] {}
  ```

  ### // how to make text with gradient
  ```
background-image: linear-gradient(to right, $color-primary-light, $color-primary-dark);
  display: inline-block;
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
```
### Hover zoomin effect
```
&--p3 {
      left: 20%;
      top: 10rem;
      z-index: 10;
    }

    &:hover {
      transform: scale(1.05);
      box-shadow: 0 2.5rem 4rem rgba($color-black, 0.8);
      z-index: 20;
    }
```
### Flip Card Effect
https://codepen.io/anon/pen/rpMrga?editors=1100

## Background-blending
```
background-image: linear-gradient(
  to right bottom, 
  $color-secondary-light, $color-secondary-dark),
  url("../img/nat-5.jpg"); // this will later be compiled into css file in /css
  background-blend-mode: screen;
```

### Make a circle layout
```
height: 12rem;
    width: 12rem;
    float: left;
    -webkit-shape-outside: circle(50% at 50% 50%);    
shape-outside: circle(50% at 50% 50%);
    -webkit-clip-path: circle(50% at 50% 50%);
    clip-path: circle(50% at 50% 50%);
```

### Background-video
```
<!-- html -->
<div class=“parent”>
<div class="bg-video">
  <video class="bg-video__content" autoplay muted loop>
    <source src="img/video.mp4" type="video/mp4">
    <source src="img/video.webm" type="video/webm">
      Your browser is not support.
    </video>
  </div>
</div>

/* css */
.bg-video {
  // add position relative to its parent
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  z-index: -1;
  opacity: 0.15;
  overflow: hidden;

  &__content {
    width: 100%;
    height: 100%;
    object-fit: cover; // will automatically remove the offset
  }
}
```

### Sibling selector
```
.a +.b {}: direct sibling, <.a/><.b/>

.a ~.b{}: indirect sibling, <.a/><.c/><.d/>...<.b/>
```

## Responsive design
1. Mobile first: min-width
2. Desktop first: max-width

Breakpoints
In sass we can nest the media query inside the block
```
.thing {
  width: 100px;
  @media (max-width: 600px) {
    width: 20px;
  }
}
```
Also, we can use mixin to even more reduce this work
The order do maters

```
@mixin respond($breakpoint) {
  @if $breakpoint == phone {
    @media (max-width: 37.5em) { @content; } // 600px / 16
  }

  @if $breakpoint == tab-port {
    @media (max-width: 56.25em) { @content; } // 900px / 16
  }

  @if $breakpoint == tab-land {
    @media (max-width: 75em) { @content; } // 1200px / 16
  }

  @if $breakpoint == big-desktop {
    @media (min-width: 112.5em) { @content; } // 1800px / 16
  }
}
```

## Responsive image
Serve image based on device screen size. Save bandwidth and resources
### 3 types of responsive image strategies
1. Resolution switching
2. Density switching
3. Art direction

### 1. Density switching
```
<img
  srcset="img/logo-green-1x.png 1x, img/logo-green-2x.png 2x"
>
```
The img will switch the src depends on the current screen’s resolution

### 2. Art direction
```
<picture>
   <source
      srcset="img/logo-green-small-1x.png 1x, img/logo-green-small-2x.png 2x"
       media="(max-width: 37.5em)"
   >
   <img
     srcset="img/logo-green-1x.png 1x, img/logo-green-2x.png 2x"
     alt="logo"
     src="img/logo-green-small-2x.png"
     class="footer__logo"
    ><!-- fallback -->
</picture>
```
Will use the small image set when the screen size is less than 600px, use the normal one otherwise. And also, inside each direction, we use density switching.

### 3. Resolution switching
```
<img 
  srcset="./img/nat-1.jpg 300w, ./img/nat-1-large.jpg 1000w"
  sizes="(max-width: 900px) 20vw, (max-width: 600px) 36vw, 300px"
  src="./img/nat-1-large.jpg"
>
```
1. In secret, we inform the browser the width of each image (approximately), nat-1 is about 300px and nat-1-large is about 1000px.

2. In sizes, we specify the media query of the screen and the ratio of each image occupies, for example, developer tools, we can see the width of the image under 900px screen is about 228px, that is 228 / 900 = 0.25, 25% or 25vw, so we put 25vw there to let the browser choose it if the screen size and the element size meet this requirement.

3. If none of these requirement get satisfied, the image will be 300px fallback width. The src attribute is in case the old browser doesn’t recognize the srcset.

## Responsive Image in css
In media query, we can target on the screen resolution by using `@media (min-resolution: 192dpi) { … }`, also, we can combine it with the min-width media query to create something like this `@media (min-resolution: 192dpi) and (min-width: 600px) { … }`. This basically means if the resolution is larger than 192dpi as well as the view port width is larger than 600px, we apply the stuff inside. And the reason behind this is because some of the mobile devices has higher resolution screens.

In media query, if we have an or relationship, we can combine them by using the “,”. For example:
```
@media (min-resolution: 192dpi) and (min-width: 600px) {xxx}
@media (min-width: 2000px) {xxx}
```
It basically tells us if the screen resolution is larger than 192dpi and the viewport width is larger than 600px or if the screen is larger than 2000px then we apply xxx, so we can combine them together into:
`@media (min-resolution: 192dpi) and (min-width: 600px),
    (min-width: 2000px) {xxx}`

## Browser support
We want to add some new css feature in our project but make sure the browsers that doesn’t support it have a fallback then we can use the @support query, for example:

```
.popup {
  …
  background-color: rgba($color-black, 0.75);

  @supports(-webkit-backdrop-filter: blur(10)) or (backdrop-filter: blur(10)) {
    -webkit-backdrop-filter: blur(10);
    backdrop-filter: blur(10);
    background-color: rgba($color-black, 0.3);
  }
}
```
This means if the browser support the backdrop-filter property, we can use it, otherwise, use the background color only.

We always put the fallback property first and add the additional ones if the browser pass the `@supports test`.

In media query, we can specify if the device is a touch screen device (like phones and ipad) or a device can use mouse by using `@media (hover:none)` or `@media (hover:hover)` respectively.



