### Code style

- Use 'hyphenated-case' for scss variables, functions, selectors, classes and ids.

- Use four spaces for property indentation.

- Keep the opening curly brace in the same line as the selector and the closing curly on its own ending line.

Instead of:

```
$infoButtonColor: #ffa600;
.InfoButton { color: $infoButtonColor; font-size: 3em; }
```

Format it like:

```
$info-button-color: #ffa600;

.info-button {
	color: $info-button-color;
	font-size: 3em;
}
```

##### Specific naming

Do not use over generalise SCSS variable names.

Instead of:

```
$link: #ffa600;
$width: 300px;
$length: 200px;
$radius: 5px;
```

Be more specific:

```
$link-primary: #ffa600;
$info-button-width: 300px;
$home-sidebar-length: 200px;
$tab-radius: 5px;
```

### Order properties by Context

In css selectors containing many properties order them by context.
To improve readability organise properties from this context:

1.  Layout **(position, float, clear, display)**
2.  Box Model **(width, height, margin, padding)**
3.  Visual **(color, background, border, box-shadow)**
4.  Typography **(font-size, font-family, text-align)**
5.  Misc **(cursor, overflow, z-index)**

So for example, instead of no order:

```
.button {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    margin: 1em 0;
    text-transform: uppercase;
    padding: 1em 4em;
    color: #fff;
    border: 0.25em solid #196e76;
    text-decoration: none;
    background: #196e76;
    font-size: 3em;
    display: inline-block;
    text-align: center;
}
```

Use the Context provided:

```
.button {
    display: inline-block;
    margin: 1em 0;
    padding: 1em 4em;

    color: #fff;
    background: #196e76;
    border: 0.25em solid #196e76;

    font-size: 3em;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    text-align: center;
    text-transform: uppercase;
    text-decoration: none;
}
```

### Order css styles in files from their visual position

Where possible, it improves readability to order css selectors from their "Top Down" visual order.

For example:

Instead of no order:

```
.main-content {}
.footer {}
.section-a {}
.header-nav {}
.sidebar {}
.header {}
.section-c {}
.footer-nav {}
.section-b {}
```

Follow their visual order:

```
.header {}
.header-nav {}
.sidebar {}
.main-content {}
.section-a {}
.section-b {}
.section-c {}
.footer-nav {}
.footer {}
```

### CSS feature support

The minimum supported browser version determines the features you can use.
To verify any feature make use of compatibility charts as below.

#### Compatibility charts

*   [caniuse.com](http://www.caniuse.com)
*   [quirksmode.org](http://www.quirksmode.org/)
*   [MDN css reference](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference)
*   [status.modern.ie](https://status.modern.ie/)
*   [chromestatus](https://www.chromestatus.com/features)

### Vendor prefixes

Vendor prefixes are best to implement inside SCSS @mixins.

Instead of:

```
p {
    -webkit-hyphens: auto;
    -ms-hyphens: auto;
    -moz-hyphens: auto;
    -o-hyphens: auto;
    hyphens: auto;
}
```

Use a mixin:

```
p {
    @include vendor-prefix(hyphens, auto);
}
```

### Use SCSS operators

Do not hard code calculations for css values. If something is required to for example determine
half of a given value or convert px to percentage, use SCSS variables and operators instead.

Instead of:

```
$home-available-width = 960px;
$home-section = 480px; // half the value of $home-available-width
```

Use SCSS operators:

```
$home-available-width = 960px;
$home-section = $home-available-width/2;
```

### Limit Selector nesting

Overly nested scss nesting leads to complex stylesheets and inheritance issues.
When using nesting levels more than 3 deep should be rethought.

Instead of:

```
nav {
    ul {
        margin: 0;

            li {
                display: inline-block;

                a {
                    display: block;
                    padding: 6px 12px;
                    text-decoration: none;
                }
            }
    }
}
```

Use less redundant nesting:

```
nav {
    ul {
        margin: 0;
    }

    li {
        display: inline-block;
    }

    a {
        display: block;
        padding: 6px 12px;
        text-decoration: none;
    }
}
```

### Use @mixins only when parameters are required

Using mixins that do not require parameters just bloats the compiled css code.
When no parameters are required instead use @extend or fragments.

For example bg-image has no parameters and just adds properties with static values:

```
@mixin button() {
    width: 100%;
    background-position: center center;
    background-size: cover;
    background-repeat: no-repeat;
}

.image-one {
    @mixin bg-image;
    background-image:url("/images/image-one.jpg");
}

.image-two {
    @mixin bg-image;
    background-image:url("/images/image-two.jpg");
}
```

Instead use @extend:

```
.bg-image {
    width: 100%;
    background-position: center center;
    background-size: cover;
    background-repeat: no-repeat;
}

.image-one {
    @extend .bg-image;
    background-image:url("/images/image-one.jpg");
}

.image-two {
    @extend .bg-image;
    background-image:url("/images/image-two.jpg");
}
```

Which compiles to more optimised css:

```
.bg-image, .image-one, .image-two {
    width: 100%;
    background-position: center center;
    background-size: cover;
    background-repeat: no-repeat;
}

.image-one {
    background-image:url("/images/image-one.jpg");
}

.image-two {
    background-image:url("/images/image-two.jpg");
}
```

### Folder structure

Compose the main stylesheet from a single composition of partials using @import.
Follow the SASS folder name pattern of using a prefixed underscore for these partials.

Example filename:

	`_layout.scss` imported as `@import 'layout';`.

Example stylesheet composition:

```
// Vendor and frameworks like bootstrap
@import 'normalize';
@import 'framework-x-y';

// Variables for colors fonts etc.
@import 'config';

// Custom utilities, mixins
@import 'layout';
@import 'mixins';

// Components/directive stylesheets
@import 'buttons.css';
@import 'forms.css';

// Feature based partials
@import 'homepage';
@import 'about';
```
