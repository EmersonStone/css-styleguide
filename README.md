# Emerson Stone CSS / Sass Styleguide

*A mostly reasonable approach to CSS and Sass*

## Table of Contents

1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [Naming Convention](#naming-convention)
    - [ID Selectors](#id-selectors)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
    - [Global Selectors](#global-selectors)
1. [Sass](#sass)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Nested selectors](#nested-selectors)
    - [File Structure](#file-structure)

## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

**[⬆ back to top](#table-of-contents)**

## CSS

### Formatting

* Use soft tabs (2 spaces) for indentation
* Prefer dashes over camelCasing in class names.
* Do not use ID selectors
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line
* Put blank lines between rule declarations

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* Prefer line comments (`//` in Sass-land) to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

### Naming Convention

We created our own combination of OOCSS, BEM, and SMACSS, lovingly called ESCSS for these reasons:

  * It helps create clear, strict relationships between CSS and HTML
  * It helps us create reusable, composable components
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets

**Example**

```html
<section class="hero hero-bg-video">

  <div class="hero-filter"></div>

  <div class="hero-video">
    <div class="hero-poster"></div>
    <video></video>
  </div>

  <div class="container">
    <div class="row">

      <div class="hero-content -center">
        <h1 class="hero-title">This is a dope page</h1>
        <p class="hero-subtitle">And here is some sweet subtitle text</p>
        <button class="button button-cta"></button>
      </div>

    </div>
  </div>

</section>
```

```css
/* _hero.scss */
.hero {

  &-bg-video {
    
  }

  &-filter {

  }

  &-video {
    
    video {

    }
  }

  &-poster {
    
  }

  &-content {
    
    &.-center {
      text-align: center;
    }
  }

  &-title {
    
  }

  &-subtitle {
    
  }
}
```

  * `.ListingCard` is the “block” and represents the higher-level component
  * `.ListingCard__title` is an “element” and represents a descendant of `.ListingCard` that helps compose the block as a whole.
  * `.ListingCard--featured` is a “modifier” and represents a different state or variation on the `.ListingCard` block.

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript hooks

When tying Javascript functionality to a DOM element, avoid using classes or data attributes. Instead, use IDs that are camelCased.

```html
<nav id="mobileMenu">Request to Book</nav>
```

### Border

Use `0` instead of `none` to specify that a style has no border.

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```

### Global Selectors
There are times when we need to differention the entire page from others. Whether it be for specific page overrides, theming, or accessibility and compatibility issues.

Put these classes on the body element and nowhere else. THen scope the styles in their respective files as outlined in the [File Structure](#file-structure) section.

**[⬆ back to top](#table-of-contents)**

## Sass

### Syntax

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

### Ordering of property declarations

1. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` declarations

    Grouping `@include`s at the beginning makes it easier to override the included styles where needed. Try to avoid this pattern, but if you are including something like font-styles, that need minor tweaking, like line-height, go for it.

    ```scss
    .btn-green {
      @include button-text;
      background: green;
      font-weight: bold;
      line-height: 1.3;
      // ...
    }
    ```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins. FOR THE LOVE OF GOD DON'T USE THESE!!!1!

### Nested selectors

**Do not nest selectors more than three levels deep!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable


Again: **never nest ID selectors!**

If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.

### File Structure

We follow a file structure inspired heavily by SMACSS. Most work is done in the `components` folder. Each folder has an `_all.scss` that houses the includes for all of the subfiles. Then, the `style.scss` file includes the all files. Other than the primary style.scss file, always prepend your filenames with an underscore to tell the compiler to ignore direct comilation of that file.

resources
|- scss
  |- global // houses variables, resets, any default styling, and functions
  |- layout // the is where the grid/container lives
  |- components // here is all of the dope stuff you build!
  |- mixins // any tools we use, like breakpoint mixins
  |- typography // all font-face declarations and type mizins
  |- pages // if we need page specific styling
  |- theme // if we have a global theme swap (this is rare)

**[⬆ back to top](#table-of-contents)**
