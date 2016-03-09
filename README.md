# CSS/HTML Styleguide

- Sections, components, atoms, modifiers

## CSS
- components are top level wrappers around atoms. Their classes will sometimes be prefixed with `es-` if used in a client project, or have regular, semantic names like `header-hero` `nav` `sidebar-cta` `button`
- The atoms inside the components all have concise names beginning with a -. So `-title` `-content` and so on. These can be generalizations and not overly specific (i.e. not BEM style)
- modifiers can be global or related to an atom. For instance `button` can have `-outline` `-blue` `-active`

- Use semantic HTML as often as possible and avoid styling based on html tags at all costs (within our own projects especially)

- Create a new component file for every component made.

- avoid @extends because they create unnecessary bloat.

- Use mixing wherever needed but not to the point of overcomplicating/obfuscating the code
