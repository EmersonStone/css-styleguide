# CSS/HTML Styleguide

Initial pieces ganked from [Airbnb CSS Styleguide](https://github.com/airbnb/css)

- Sections, components, atoms, modifiers

## CSS
- components are top level wrappers around atoms. Their classes will sometimes be prefixed with `es-` if used in a client project, or have regular, semantic names like `header-hero` `nav` `sidebar-cta` `button`
- The atoms inside the components all have concise names beginning with a -. So `-title` `-content` and so on. These can be generalizations and not overly specific (i.e. not BEM style)
- modifiers can be global or related to an atom. For instance `button` can have `-outline` `-blue` `-active`

- Use semantic HTML as often as possible and avoid styling based on html tags at all costs (within our own projects especially)

- Create a new component file for every component made.

- avoid @extends because they create unnecessary bloat.

- Use mixing wherever needed but not to the point of overcomplicating/obfuscating the code

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

Use JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="button -primary js-request-to-book">Request to Book</button>
```


### Nested selectors

**Do not nest selectors more than three levels deep!**

- Nested selectors should be succinct. If they get too complex look into making a mixin or breaking them out into a component.

```scss
.page-container {
  .-content {
    .-avatar {
      // STOP!
    }
  }
}
```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`). Keep variable names semantic, (e.g. `$color-blue`, `$nav-z`, `breakpoint-mobile`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.
