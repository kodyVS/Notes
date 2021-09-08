## General
- Bootsrap is deisgned to be mobile first
- Use small zindexs when layering components (less than 1000)
- Deafult box sizing is border-box
- They have tables (learn when needed)
- 


## Classes
`container` class holds the rows. Also adds the gutter spacing?
`container-fluid` class will make the container span the entire viewport.
`col` without any modifers can be used, and depending on how many `.col` are in an row it will split the row accordingly
`col-` applies to 576px screens and larger
`col-sm-` applies to screens at 576px
`col-md-` applies to screens 768px and smaller
`col-lg-` applies to screens 992px and larger
`col-xl-` applies to screens 1200 pixles and larger
`col-(number)` will give you the length of 12/(number) 
`col-breakpoint-auto` will size the column depending on the natural size of the content
`w-100` will create a breakpoint that will jump the rest of the content to the next row. It will expand the content to 100% of the container
`align-items-(start/center/end` on a `row` will act align the items vertically in the row
`align-self-(start/center/end` on a `col` will align the specific col vertically in a row
`justify-content-(start/center/end/around/between)` will horizontally align the items in a row
`no-gutters` on a `row` will drop the 30px spacing around the cols
`offset-breakpoint-number` on a `col` will offset the col by the number
`ml-auto` or `mr-auto` on a `col` will create an automargin on that side of the `col` which will push the `col` as far as possible
`img-fluid` is used to make images responsive, will scale with the parent element
`figure, figure-img and figure-cap` are all used with figures to make an image with some text below
>When using figures make sure to apply the img-fluid tag to the image



### Typography
`text-muted` gives a faded secondary text
`display-(number)` is similiar to h1,h2 etc but with different styles
`lead` will make a paragraph stand out
`blockquote` creates a blockquote
`blockquote-footer` creates a name for the blog quote (cite)
`text-(center/left/right)` will align text 



[[Inline styles.png ]]





## Breakpoints
Bootstrap primarily uses the following media query ranges—or breakpoints—in our source Sass files for our layout, grid system, and components.
```scss
// Extra small devices (portrait phones, less than 576px)
// No media query since this is the default in Bootstrap

// Small devices (landscape phones, 576px and up)
@media (min-width: 576px) { ... }

// Medium devices (tablets, 768px and up)
@media (min-width: 768px) { ... }

// Large devices (desktops, 992px and up)
@media (min-width: 992px) { ... }

// Extra large devices (large desktops, 1200px and up)
@media (min-width: 1200px) { ... }

// Extra small devices (portrait phones, less than 576px)
@media (max-width: 575.98px) { ... }

// Small devices (landscape phones, less than 768px)
@media (max-width: 767.98px) { ... }

// Medium devices (tablets, less than 992px)
@media (max-width: 991.98px) { ... }

// Large devices (desktops, less than 1200px)
@media (max-width: 1199.98px) { ... }

// Extra large devices (large desktops)
// No media query since the extra-large breakpoint has no upper bound on its width
```

Since we write our source CSS in Sass, all our media queries are available via Sass mixins:

**Mobile First**
```scss
@include media-breakpoint-up(xs) { ... }
@include media-breakpoint-up(sm) { ... }
@include media-breakpoint-up(md) { ... }
@include media-breakpoint-up(lg) { ... }
@include media-breakpoint-up(xl) { ... }

// Example usage:
@include media-breakpoint-up(sm) {
  .some-class {
    display: block;
  }
}
```

**Desktop First**
```scss
@include media-breakpoint-down(xs) { ... }
@include media-breakpoint-down(sm) { ... }
@include media-breakpoint-down(md) { ... }
@include media-breakpoint-down(lg) { ... }
```

## Grid

-   Containers provide a means to center and horizontally pad your site’s contents. Use `.container` for a responsive pixel width or `.container-fluid` for `width: 100%` across all viewport and device sizes.
- Only columns can be immediate children of rows
- -   Thanks to flexbox, grid columns without a specified `width` will automatically layout as equal width columns. For example, four instances of `.col-sm` will each automatically be 25% wide from the small breakpoint and up. See the [auto-layout columns](https://getbootstrap.com/docs/4.0/layout/grid/#auto-layout-columns) section for more examples.
-   Column classes indicate the number of columns you’d like to use out of the possible 12 per row. So, if you want three equal-width columns across, you can use `.col-4`.
-   Column `width`s are set in percentages, so they’re always fluid and sized relative to their parent element.
- -   olumns have horizontal `padding` to create the gutters between individual columns, however, you can remove the `margin` from rows and `padding` from columns with `.no-gutters` on the `.row`.
-   To make the grid responsive, there are five grid breakpoints, one for each [responsive breakpoint](https://getbootstrap.com/docs/4.0/layout/overview/#responsive-breakpoints): all breakpoints (extra small), small, medium, large, and extra large.
-   Grid breakpoints are based on minimum width media queries, meaning **they apply to that one breakpoint and all those above it** (e.g., `.col-sm-4` applies to small, medium, large, and extra large devices, but not the first `xs` breakpoint).
- grid has 12 columns with a 30px (15 on each side) gutter width

## Page Defaults
The `<html>` and `<body>` elements are updated to provide better page-wide defaults. More specifically:

-   The `box-sizing` is globally set on every element—including `*::before` and `*::after`, to `border-box`. This ensures that the declared width of element is never exceeded due to padding or border.
    -   No base `font-size` is declared on the `<html>`, but `16px` is assumed (the browser default). `font-size: 1rem` is applied on the `<body>` for easy responsive type-scaling via media queries while respecting user preferences and ensuring a more accessible approach.
-   The `<body>` also sets a global `font-family`, `line-height`, and `text-align`. This is inherited later by some form elements to prevent font inconsistencies.
-   For safety, the `<body>` has a declared `background-color`, defaulting to `#fff`.

### Responsive Typography
```scss
html {
  font-size: 1rem;
}

@include media-breakpoint-up(sm) {
  html {
    font-size: 1.2rem;
  }
}

@include media-breakpoint-up(md) {
  html {
    font-size: 1.4rem;
  }
}

@include media-breakpoint-up(lg) {
  html {
    font-size: 1.6rem;
  }
}
```


