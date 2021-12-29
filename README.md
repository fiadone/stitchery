# @fiad/stitchery

A sharp collection of SCSS utilities

---

## Get started

#### Installation
```
npm i @fiad/stitchery
```

#### Usage
```scss
/* main.scss */
@import '@fiad/stitchery';
```


## Functions list

### map-get-default

A simple high order function that wraps *map-get* but returns a fallback value instead of *null* when the map does not have a value associated with the key being searched.

__Definition__:
```scss
map-get-default($map, $key, $default)
```

__Usage__:
```scss
color: map-get-default($colors, 'white', '#FFFFFF');
```


### map-get-recursive

It provides a recursive version of *map-get* that allows you to perform a deep-search over the map.

__Definition__:
```scss
map-get-recursive($map, $keys...)
```

__Usage__:
```scss
color: map-get-recursive($theme, 'colors', 'primary');
```


### rem

It provides a *px* to *rem* conversion.

__Definition__:
```scss
rem($px, $context, $threshold)
```

| Argument | Type | Description | Default |
| --- | --- | --- | --- |
| $px | *number* | The size to be converted from *px* to *rem*. | - |
| $context | *number* | The conversion unit, i.e. the amount of *pixels* equivalent to the base value (1) of the target unit. | *16px* |
| $threshold | *number* | The limit below which the conversion is ignored in order to prevent rendering issues due to subpixels division. | *2px* |

__Basic usage__:
```scss
font-size: rem(18px);
```

__Advanced usage__:
```scss
padding: rem(12px 16px);
box-shadow: rem(0 4px 10px rgba(0, 0, 0, 0.25));
```


### str-replace

It provides a simple string replacement.

__Definition__:
```scss
str-replace($string, $search, $replacement)
```

__Basic usage__:
```scss
background-image: url('#{str-replace($imageUrl, 'http:', '')}');
```

__Advanced usage (multiple entries)__:
```scss
background-image: url('#{str-replace($imageUrl, ('https:', 'http:'), '')}');
```


### strip-unit

It strips unit from the given value.

__Definition__:
```scss
strip-unit($value)
```

__Usage__:
```scss
$someSize: 512px;

strip-unit($someSize) // 512
```


## Mixin list

### aspect-ratio

It provides a set of rules that preserves the original aspect ratio of an element while it's scaled according to the sizes of a parent container.

```scss
.video {
  @include aspect-ratio(16, 9); // using well-known lowest terms
}

.image {
  @include aspect-ratio(480, 320); // using explicit sizes
}
```


### atomic-helpers

It generates a set of *functional CSS classes*.

__Definition__:

```scss
@mixin atomic-helpers($base-class, $specs);
```

__Usage__:

SCSS:

```scss
@include atomic-helpers("m", (
  "property": (
    "_": "margin",        // .m-
    "t": "margin-top",    // .mt-
    "r": "margin-right",  // .mr-
    "b": "margin-bottom", // .mb-
    "l": "margin-left"    // .ml-
  ),
  "value": (
    "0": 0,               // .m[property]-0
    "1": 8px,             // .m[property]-1
    "2": 16px,            // .m[property]-2
    "3": 24px,            // .m[property]-3
    "4": 32px,            // .m[property]-4
    "5": 40px,            // .m[property]-5
    "6": 48px,            // .m[property]-6
    "7": 56px,            // .m[property]-7
    "8": 64px             // .m[property]-8
  ),
  "breakpoints": (
    "sm",                 // .sm:m[property]-[value]
    "md",                 // .md:m[property]-[value]
    "lg"                  // .lg:m[property]-[value]
  )
));
```

> ⚠️ __Notice__: the keys listed by the "breakpoints" property refer to the ones defined in a global *$breakpoints* variable, that is supposed to be a map containing a key-value entry for each breakpoint handled in your project. By the way, the *atomic-helpers* mixin comes along with a default value for that variable, corresponding to the following:
>```scss
>$breakpoints: (
>  "sm": 768,
>  "md": 1024,
>  "lg": 1366
>);
>```
>In order to use different breakpoint values and/or different prefixes for the generated selectors, overwrite it according to your needs.

HTML:

```html
<h1 class="m-2">Title</h1>
<p class="mt-6 sm:mt-4 md:mt-3 lg:mt-2">Lorem ipsum</p>
```

CSS OUTPUT:

```css
.m-0 {
  margin: 0;
}
/* [...] */
.m-8 {
  margin: 64px;
}
.mt-0 {
  margin-top: 0;
}
/* [...] */
.mt-8 {
  margin-top: 64px;
}
/* [...] */
@media screen and (min-width: 768px) {
  .sm\:m-0 {
    margin: 0;
  }
  /* [...] */
  .sm\:m-8 {
    margin: 64px;
  }
}
/* [...] */
```


### auto-scaling

It provides an auto-scaling system based on *relative units*.

__Definition__:

```scss
@include auto-scaling($naturalMobileSize, $naturalTabletSize, $naturalDesktopSize, $mobileBreakpoints, $tabletBreakpoints, $desktopBreakpoints);
```

| Argument | Description | Default |
| --- | --- | --- |
| $naturalPhoneSize | The width of the given phone layout (.psd, .sketch, etc). | *375* |
| $naturalTabletSize | The width of the given tablet layout (.psd, .sketch, etc). | *768* |
| $naturalDesktopSize | The width of the given desktop layout (.psd, .sketch, etc). | *1440* |
| $phoneBreakpoints | The phone breakpoints to be handled. | *(320, 360, 375, 414)* |
| $tabletBreakpoints | The tablet breakpoints to be handled. | *(600, 768, 800, 962)* |
| $desktopBreakpoints | The desktop breakpoints to be handled. | *(1024, 1152, 1280, 1366, 1440, 1536, 1600, 1920)* |

__Usage__:

SCSS:

```scss
html {
  @include auto-scaling();
}

.container {
  max-width: 80rem; // this size will proportionally scale across the defined breakpoints
}
```

CSS OUTPUT:

```css
@media screen and (min-width: 320px) {
  html {
    font-size: 85%;
  }
}

@media screen and (min-width: 360px) {
  html {
    font-size: 96%;
  }
}

@media screen and (min-width: 375px) {
  html {
    font-size: 100%;
  }
}

@media screen and (min-width: 414px) {
  html {
    font-size: 110%;
  }
}

@media screen and (min-width: 600px) {
  html {
    font-size: 78%;
  }
}

@media screen and (min-width: 768px) {
  html {
    font-size: 100%;
  }
}

@media screen and (min-width: 800px) {
  html {
    font-size: 104%;
  }
}

@media screen and (min-width: 962px) {
  html {
    font-size: 125%;
  }
}

@media screen and (min-width: 1024px) {
  html {
    font-size: 71%;
  }
}

@media screen and (min-width: 1152px) {
  html {
    font-size: 80%;
  }
}

@media screen and (min-width: 1280px) {
  html {
    font-size: 88%;
  }
}

@media screen and (min-width: 1366px) {
  html {
    font-size: 94%;
  }
}

@media screen and (min-width: 1440px) {
  html {
    font-size: 100%;
  }
}

@media screen and (min-width: 1536px) {
  html {
    font-size: 106%;
  }
}

@media screen and (min-width: 1600px) {
  html {
    font-size: 111%;
  }
}

@media screen and (min-width: 1920px) {
  html {
    font-size: 133%;
  }
}
```

__How it works__:

The mixin generates a *font-size* rule for each defined breakpoint, setting a proportional percentage value compared to the given natural one (phone, tablet, or desktop, according to the current viewport width). This way, a proportional *font-size* context is created and it's possible to auto-scale elements sizes by simply using *relative units*.

It's highly recommended to implement a unique global auto-scaling context by appending the mixin directly to the *html* styles and by setting all DOM elements width, height, paddings, margins, and other sizes in *rem*.
However, if for any reason a specific application's part only is required to be auto-scaled, you can also create a "local" auto-scaling context by just applying the mixin to the target wrapper and by setting all its children sizing and spacing values in *em*.

> ⚠️ __Notice__: since the mixin widely uses the *font-size* rule, pay attention when styling typography if you choose a "local" context approach. In this case, try to apply typographic rules as deeply as possible, avoiding parent styles inheriting.

> 💡 __Tips__: since *px-to-(r)em* conversion may produce many decimal places and the auto-scaling system is based on percentage values, you may encounter some rendering issues due to subpixel division. This usually happens when dealing with odd values or trying to scale too small values. As a best practice, try to round sizes to powers of 2, expecially for horizontal dimensions (e.g. margin, padding, width, etc), and avoid to scale sizes lower than 4px, including borders.


### grid

It provides a simple grid system based on *CSS Grid*.

__Definition__:

```scss
grid($columns, $gap)
```

| Argument | Description | Default |
| --- | --- | --- |
| $columns | The grid size, i.e. the number of column subdivisions. | *12* |
| $gap | The gap between columns and rows. | *1rem* |

__Usage__:

SCSS:

```scss
@include grid();
```

HTML:

```html
<div class="row">
  <!--
    12 columns cells under 768px
    6 columns cells under 1024px
    4 columns cells under 1366px
    3 columns cells from 1366px
  -->
  <div class="col sm:span-6 md:span-4 lg:span-3"></div>
  <div class="col sm:span-6 md:span-4 lg:span-3"></div>
  <div class="col sm:span-6 md:span-4 lg:span-3"></div>
  <div class="col sm:span-6 md:span-4 lg:span-3"></div>
</div>
<div class="row">
  <!--
    12 columns cell under 1024px
    10 columns cell with 1 column offset under 1366px
    8 columns cell with 2 columns offset from 1366px
  -->
  <div class="col md:span-10 md:start-2 lg:span-8 lg:start-3"></div>
</div>
```

> ⚠️ __Notice__: the keys used as class prefixes for the breakpoint-targeted properties (sm, md, lg) refer to the ones defined in a global *$breakpoints* variable, that is supposed to be a map containing a key-value entry for each breakpoint handled in your project. By the way, the *grid* mixin comes along with a default value for that variable, corresponding to the following:
>```scss
>$breakpoints: (
>  "sm": 768,
>  "md": 1024,
>  "lg": 1366
>);
>```
>In order to use different breakpoint values and/or different prefixes for the generated selectors, overwrite it according to your needs.
