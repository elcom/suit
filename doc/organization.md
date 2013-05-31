# SUIT organization

To faciliate code reuse, ease refactoring, and to better separate components:

1. Use a new file or package for each new type of utility.

2. Use a new file or package for each independent component.

3. Separate app-level CSS and theming from the generic structural and
   functional utilities and components.

This distinction is reified only via 2 top level directories that separate
files based on a 'core' and a 'theme'.

Files that are particularly important and/or fundamental to the app structure
can be placed in 'core'. When diffs contain changes to 'core' it should
accurately reflect the potential for a significant, structural part of the UI
to be affected. As such, files in 'core' should end up being modified at a
slower rate than those in 'theme'.

For example:

```
.
├── main.css
├── theme
|   ├── util
|   |   ├── link.css
|   |   └── text.css
|   └── component
|       ├── button.css
|       └── stream-item.css
└── core
    ├── util
    |   ├── dimension.css
    |   ├── display.css
    |   ├── layout.css
    |   └── text.css
    └── component
        ├── button.css
        ├── button-group.css
        └── grid.css
```

You'll notice that both 'core' and 'theme' contain 'util' and 'component'
directories. To change a file from 'theme' to 'core' (and vice versa) requires
only a change of top-level directory. Therefore, it's easy to move files
between the 'theme' and 'core' when appropriate. If a component in 'theme'
turns out to be better suited in 'core', then making the change shouldn't be
too difficult.

All 'core' files of a particular type must be referenced before the 'theme' files:

```css
/* Utilities */

@import "core/util/display.css";
@import "core/util/layout.css";
@import "core/util/dimension.css";
@import "core/util/text.css";

@import "theme/util/link.css";
@import "theme/util/text.css";

/* Components */

@import "core/component/button.css";
@import "core/component/button-group.css";
@import "core/component/grid.css";

@import "theme/component/button.css";
@import "theme/component/steam-item.css";
```
