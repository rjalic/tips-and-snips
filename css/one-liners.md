# CSS One Liners

## Gap

Adds a gap between flex or grid items.

```css
/* https://developer.mozilla.org/en-US/docs/Web/CSS/gap */
gap: length | percentage;
```

## Writing mode

Controls the direction of the text.

```css
/* https://developer.mozilla.org/en-US/docs/Web/CSS/writing-mode */
writing-mode: direction;
```

## Flip image

Flips the image based on the scale.

```css
/* https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/scaleX */
transform: scaleX(-1); /* for horizontal */

/* https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/scaleY */
transform: scaleY(-1); /* for vertical */
```

## Scrolling

### Smooth scrolling

Smoothly scrolls to the anchor.

```css
/* https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-behavior */
scroll-behavior: smooth;
```

### Scroll snapping

Snap to the item element based on the axis and type.

```css
/* https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-snap-type */
.container {
  scroll-snap-type: x mandatory | proximity;
}

/* https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-snap-align */
.container-item {
  scroll-snap-align: center;
}
```

## Resize

Resize a container.

```css
/* https://developer.mozilla.org/en-US/docs/Web/CSS/resize */
.container {
  overflow: auto;
  resize: both | vertical | horizontal | none;
}
```

## Truncate text

Truncates text by using webkit box features.

```css
.text-container {
  display: -webkit-box;
  /* https://developer.mozilla.org/en-US/docs/Web/CSS/-webkit-line-clamp */
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
```

## Gradient text

Make the text have a gradient by making the text transparent while masking the background.

```css
background: linear-gradient(to right, rgb(67, 124, 205), rgb(69, 214, 202));
/* https://developer.mozilla.org/en-US/docs/Web/CSS/background-clip */
-webkit-background-clip: text;
/* https://developer.mozilla.org/en-US/docs/Web/CSS/-webkit-text-fill-color */
-webkit-text-fill-color: transparent;
```

## Image fit

Fill the container with the image without streching it.

```css
.container img {
  object-fit: cover;
}
```

or

```css
.container {
  background-image: url(img.png);
  background-size: cover;
}
```

## Disable cursor

### For animation

Disable cursor for animation.

```css
.title {
  /* https://developer.mozilla.org/en-US/docs/Web/CSS/user-select */
  user-select: none;
  /* https://developer.mozilla.org/en-US/docs/Web/CSS/animation */
  animation: fade 1s ease-in 1s;
}

/* https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes */
@keyframes fade {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
```
