# SUIT documentation

**Table of contents**

* [Design principles](#design-principles)
* [Architecture](#architecture)
* [Authoring guidelines](#authoring-guidelines)

**Further documentation**

* [Components](components.md)
* [Utilities](utilities.md)
* [Organization](organization.md)
* [Misc](misc.md)

[Code style](code-style.md)


## Design principles

SUIT aims to loosely couple document semantics, presentation, and behaviour so
as to be able to modify any one of them with minimal impact on the others.

1. **Spent less time writing CSS.**

   Codifying common structural and stylistic patterns in a way that allows them
   to be reused in different contexts.

2. **Combine design traits in HTML templates, using classes**

   Trait composition is shifted to the HTML. In general, look to apply classes
   directly to the elements you want to affect.

3. **Write small, independent, content-agnostic packages**

   Solve generic problems in isolation and with few, known dependencies. SUIT
   is philosophically opposed to starting out with a monolithic framework or
   toolkit.

4. **Use clear, structured, and content-independent HTML class names**

   Avoid content-derived HTML class names as these cannot be design- and
   content-agnostic. SUIT relies on a fully documented, structured, semantic class
   name pattern.

These principles are supported by the use of contemporary web site/app
development tools: templating engines, build tools, and the
[Bower](http://bower.io/) package manager.

## Architecture

SUIT relies on a specific architectural approach to work around the current
limits of applying CSS to the DOM (e.g., lack of style encapsulation).

SUIT uses "meaningful hyphens" in HTML class names. At least one hyphen is used
to separate types and names, but not words.

* All component names must use "Pascal case"; all other names must be "Camel case".

Although the major architectural division is between **utilities** and
**components**, a layer of finer separation of responsibilities is build upon
it.

There are 5 distinct prefixes that are used for all class names that aren't for
components. This helps to identify their specific responsibility.

* `u-`: A utility.
* `u-is`: A state-utility.
* `is-`: A custom, component-state (scoped to the component).
* `with-`: A component mixin.
* `js-`: A JavaScript hook (not for CSS use).

The complete set of naming patterns is as follows:

**[Utilities](utilities.md)**:

* `u-utilityName`
* `u-isStateName`

**[Components](components.md)**:

* `ComponentName`
* `ComponentName--modifierName`
* `ComponentName-descendantName`
* `is-stateOfComponent`
* `with-ComponentName`

**[Misc](misc.md)**:

* `js-aName`


## Authoring guidelines

SUIT aims to make it easier for teams to work with, and reuse, HTML/CSS.

* Favour readable and understand class names for components and their
  constituent parts. Use names that are as short as possible but as long as
  necessary.

* Use short, low-specificity CSS selectors.

* Limit the scope of style inheritance in your components, e.g., use `.Grid >
  .Grid-cell` rather than `.Grid .Grid-cell`.

* Use classes rather than tags in selectors, as much as is practical, e.g., use
  `.List-item` rather than `.list li`. This helps make components more reusable
  and robust.

Trait composition takes place on the element:

```html
<div class="ComponentName u-utilityName">
```

If you need to adjust a generic component within your specific component, do
not hijack it or scope the adjustment within your component. Create a new part
of YOUR component that will take care of the adjustment, and then add your hook
to the HTML element.

BAD:

```html
<div class="Tweet">
    <i class="Icon Icon--retweet"></i>
    …
</div>
```

```css
/* In tweet.css */
.Tweet .Icon {
    position: absolute;
    top: 0;
}
```

GOOD:

```html
<div class="Tweet">
    <i class="Tweet-icon Icon Icon--retweet"></i>
    …
</div>
```

```css
/* In tweet.css */
.Tweet-icon {
    position: absolute;
    top: 0;
}
```

If your component can be affected by a state change in a parent component
(e.g., a `:hover` or `:focus` interaction), then you may depend on the parent
component's class in your component's CSS file. In general, a ruleset should
always be in the file of the component that matches the last class in the
selector. For example:

```css
/**
 * These styles are in the 'tweet.css' component file because the last
 * class in each rule is for a descendant element of the 'Tweet' component.
 */

.StreamItem:hover .Tweet-icon {
    color: red;
}

.StreamItem.is-collapsed:hover .Tweet-actions {
    visibility: visible;
}
```

## Importing

Make use of the `@import` directive to include utilities and components in your
main stylesheets. It also provides an interface to specify Media Queries
outside of your component's CSS code:

```css
/* main.css */

/* …other CSS imports … */

@import "core/component/button.css";
@import "core/component/button-group.css";
@import "core/component/grid.css";
@import "core/component/grid-layout-mobile.css";
@import "core/component/grid-layout-desktop.css" (min-width:50em);
```

Your build process should inline the CSS and wrap it any specified Media
Queries (you should create a production build of your CSS that omits Media
Queries if you require IE 8 support).


## Build tools

SUIT relies on some form of build process to produce production-ready code.
Imports should be inlined, CSS should be stripped of comments and minified, and
Media Queries should wrap the code from every import that appends them.


## Related reading

* [About HTML semantics and front-end architecture](http://nicolasgallagher.com/about-html-semantics-front-end-architecture/)
* [SOLID CSS](http://blog.millermedeiros.com/solid-css/)
* [Idiomatic CSS](https://github.com/necolas/idiomatic-css/)
* [Idiomatic HTML](https://github.com/necolas/idiomatic-html/)
