# (Learn Layout)[http://learnlayout.com/]

## Display Property

### block

* `div`
* `p`
* `form`
* HTML5
    * `header`
    * `section`
    * `footer`

### inline

* `span`
* `a`
* …

### inline-block

### flex

## max-width

`max-width`: makes block responsive

## box-sizing

With the following property, the padding and border of that element no longer increase its width.

```
  -webkit-box-sizing: border-box;
     -moz-box-sizing: border-box;
          box-sizing: border-box;
```

## Position

### static

The default. A `static` element is said to be **not positioned** and an element with its position set to anything else is said to **be positioned**.

### relative

`relative` behaves the same as `static` unless you add some extra properties.

> A `relative` positioned element is positioned relative to its normal position.

### fixed

A fixed element is positioned relative to the **viewport**, which means it always stays in the same place even if the page is scrolled. As with relative, the `top`, `right`, `bottom`, and `left` properties are used.

### absolute

`absolute` behaves like `fixed` except relative to the **nearest positioned ancestor** instead of relative to the viewport.

Remember:

>a "positioned" element is one whose position is anything except `static`.

So the combination of `relative` and `absolute` can be used for layout.

## float

`float` is intended for wrapping text around images.

### clearfix hack

The image is taller than the element containing it, and it's floated, so it's overflowing outside of its container!

Hack:

```
.clearfix {
  overflow: auto;
}
```

## media queries

For "Responsive Design".

* `max-width`
* `min-width`
* [others](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries)

## inline-block

More easier way of layout:

```
nav {
  display: inline-block;
  vertical-align: top;
  width: 25%;
}
.column {
  display: inline-block;
  vertical-align: top;
  width: 75%;
}
```

`inline-block` elements are affected by the `vertical-align` property, which you probably want set to `top`.

**NOTE**: IE6&7 not supported

## column

Make multi-column text easier.

```
.three-column {
  padding: 1em;
  -moz-column-count: 3;
  -moz-column-gap: 1em;
  -webkit-column-count: 3;
  -webkit-column-gap: 1em;
  column-count: 3;
  column-gap: 1em;
}
```

**NOTE**: <=IE9 or Opera mini not supported



