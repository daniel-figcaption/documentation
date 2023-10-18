
# CSS

## Introduction

At NV, CSS is compiled through [SASS](https://sass-lang.com/). We structure our SASS according to the [BEM (Block, Element, Modifier) methodology](http://getbem.com/introduction/), and the way SASS files are organised is based on [ITCSS](https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/).

A very simple but valid block of NV SASS code following BEM principles might look as follows:

```
.c-header {
    background: $dark-blue;
    color: #fff;
    display: flex;
    flex-flow: row;
    padding: 2rem;
}

.c-header--invert {
    background: #fff;
    color: $dark-blue;
}

.c-header__logo-svg {
    display: block;
    fill: currentColor;
    height: 3rem;
    width: 8rem;
}
```

The above describes a heading **component** (denoted by the `c-` prefix) which can have its colours inverted by adding another class (`.c-header--invert`) and can include a logo within it.

- `.c-header` is our **block**.
- `.c-header__logo-svg` is an **element** within our block.
- `.c-header--invert` is a **modifier** for our main block.

A resulting HTML implementation might look as follows:

```
<header class="c-header  c-header--invert">
    <svg class="c-header__logo-svg"><!-- SVG data --></svg>
</header>
```
Our component SASS would be live in a file of its own within the components folder: `css/scss/6.components/_components.header.scss`.

## File Organisation Concepts

Not all of our styling work will be within components. Our ITCSS-inspired file structure is outlined below:

1.  __Settings__  
    This is where our global SASS variables live for things like colours, font families, <mark>z-index values</mark> and breakpoints. You can also include some CSS settings such as `@keyframes` and `@font-face`.

2.  __Tools__  
    Globally used SASS mixins and functions. Apart from CSS `@` rules, no CSS should be compiled from these first 2 layers.

3.  __Generic__  
    This layer provides us a good blank slate from which to work by eliminating minor browser inconsistencies and zeroing-out margins and paddings so that HTML elements are as plain as can be. This is also where you can find blanket updates to `box-sizing` to make our lives easier.

4.  __Elements__  
    This is where we can add a bit of basic flavour to our HTML elements before classes are applied. Unless it makes sense to apply these styles across the board to all instances of an HTML element, then it is better to put your styles somewhere else (such as within a component or in the typography folder).

5.  __Objects__  
    This is where we define undecorated design patterns, e.g. our grid system. Another example could be the media object known from [OOCSS](https://github.com/stubbornella/oocss/wiki). Classes here should be organised according to NV's BEM methodology.

6. __Typography__
   These classes define common text styles present in the design which can be applied as headings, labels, body text etc. These classes free us to be able to style a leading paragraph (using a `<p>` tag) identically to a particular subheading (which would probably use an `<h3>` or `<h4>` tag.). Rich text styles should be located here using a wrapper class.

7.  __Components__  
    This layer contains all our specific UI components. This is where majority of our work takes place. Our UI components are often composed of objects and components working together. Classes here should be organised according to NV's BEM methodology.

8.  __Utilities__  
    Utilities and helper classes with ability to override anything which comes before, e.g. margin and padding classes to add space between our components.

## SASS Code Formatting

When writing SASS (or CSS directly in rare cases), format your code blocks as follows:

- Set up your text editor to indent your code using **4 spaces**.
- Selectors should be followed by one space before opening the curly brace.
- Property names should be directly followed by a colon and then one space.
- Values should be directly followed by a semi-colon.
- <mark>Property names should always be ordered in **alphabetical order** to avoid accidental duplicates.</mark>
- Multiple selectors should be separated by a comma and space, or comma and line break if it aids readability.
- SASS `@extend` and `@include` lines should typically appear first in the list of CSS properties if they can be overridden, or after if they are meant to override other properties.
- Nested code blocks should be preceded by an empty line.
- <mark>Nested code blocks should be ordered as follows:</mark>
   1. Media query modifiers (`@media`).
   2. Pseudo-class selectors (e.g. `:hover`, `:not()`, `:last-child`).
   3. Attribute selectors (e.g. `&[hidden]`, `&[data-label="Easy"]`)
   4. Generic modifier classes (e.g. `&.is-active`).
   5. Most pseudo-elements including `::before`.
   6. Child elements, ordered according to how they would typically be in markup.
   7. The `::after` pseudo-element.
- Top-level code blocks should be separated by an empty line.
- Pseudo-element selectors should use the standard two colons instead of one (e.g. `::before`).

Example:
```
.c-responsive-video {
    background: $pale-grey;
    opacity: 0.5;
    position: relative;

    &:hover {
        opacity: 1;
    }
    
    &::before {
        content: '';
        display: block;
        padding-top: 60%;
    }

    iframe, video {
        display: block;
        height: 100%;
        left: 0;
        position: absolute;
        top: 0;
        width: 100%;
    }
}

.c-responsive-video--youtube {

    &::before {
        padding-top: 70%;
    }
}
```

### CSS Attributes & Values Styleguide
- Avoid vendor prefixing your attributes if possible. This is handled automatically by the task runner which automatically prefixes properties according to a set of supported browsers. Where they are necessary, include them alongside the other attributes in alphabetical order according to the base property name (without prefix).

- <mark>Measurement values (other than percentages) should generally all be specified in **rem** units.</mark> The base font size is generally set as 16px by default for easy fractions. The helper SASS function `rem(pixelValue)` can be used to easily convert pixel values to rem units. **em** units should be reserved for cases where the measurement needs to be kept in sync with the font size, such as keeping an icon the same size as the label next to it.

- Decimal values less than zero should begin with zero, i.e. `padding: 0.5rem` instead of `padding: .5rem`.
Where the number is zero, the unit should be omitted, i.e. `margin: 0` instead of `margin: 0rem`.

- Lists of comma-separated values should be separated by a comma and a space after it. For example:
  - `linear-gradient(to right, #000, #fff)` (within a CSS function)
  -  `rgba(#000, 0.5)` (within a SASS function)
  -  `font-family: 'Roboto', sans-serif` (normal comma-separated value)

- Use single quote marks where required, e.g. `content: ''` instead of `content: ""`, and `[type='text']` instead of `[type="text"]`. Use quotes around names of font families (even "web safe" fonts such as _Verdana_) to distinguish them from keyword values such as `sans-serif`. Don't quote URL values in CSS unless necessary for parsing.

- Use CSS entities when dealing with unusual characters in strings, e.g. `content: '\2022'` for a bullet point (`&bull;`).

- When `!important` is used (sparingly) precede it with a single space, e.g. `color: inherit !important`.

- Separate operators in equations with spaces, e.g. `calc(100vh - 10rem)` or `rem(100 + $header-height)`.

- In CSS shorthand properties such as `border` the order of values is pretty flexible, but if you can, try to adopt a common approach to ordering found on [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/border), e.g. `0.25rem solid #000` instead of  `#000 solid 0.25rem`.

- <mark>Hexidecimal colour values should be lowercase, and shortened to three or four characters where appropriate</mark>, e.g. `#fff` instead of `#FFFFFF`. In most cases other than perhaps the common full white and full black these colour values should be defined as variables within `_settings.colors.scss`.

- Where values are especially long, they can optionally span two lines for readability, with the second line indented:
  ```
  box-shadow: 1rem 1rem 0.5rem 0.25rem $dark-blue,
              inset 0 0 1rem 1rem;
  ```
- <mark>Use comments (sparingly) where properties or values are not self-explanatory</mark>:
  ```
  overflow: hidden; // Used to trigger our ellipsis.
  ```

### <mark>SASS Naming Conventions</mark>

- SASS variable, mixin, and function names should always be lowercase and words should be comma-separated, e.g. `$dark-blue`.
- Variable names should be unambiguous when viewed out of context, e.g. rather than `$box-shadow` a better alternative would be `$default-subtle-box-shadow`.

### Comments

Each SASS file should begin with a comment block with the name in all caps and a brief description, formatted as follows:
```
/**
 * COMPONENTS.SOCIAL-ICONS
 *
 * @description : Set of icons linking to social media pages.
 *
 */
```

### Class Naming Conventions

- Where component names are made up of more than one word, separate using hyphens.
  - Class names: `.c-social-icons`, `.c-social-icons__icon-tooltip`, `.c-social-icons--sm-icons`
  - File name: `_components.social-icons.scss`

- Keep names reasonably short while avoiding contractions which could be potentially confusing when seen out of context.
  - Acceptable names: `.c-header`, `.c-search-results`, `.c-date-picker`
  - Acceptable names: `.c-mobile-nav`, `.c-stats`, `.c-img-block`, `.c-cta`, `.c-info-box`. (Common abbreviations used in FED.)
  - Acceptable names: `.c-header--sm`, `.c-button--lg`. (Size abbreviations found throughout NV code, such as in the grid system.)
  - Too ambiguous: `.c-res`, `.c-news-itm`, `.c-mod`, `.mob-bar` (Better to use the full word, i.e. result, item, module, mobile.)
  - <mark>**Note for Matt**: Is "btn" or "button" better? "cta" or "call-to-action"? I would pick "button" and "cta".</mark> 

- Component names should describe the core function of a component rather than its content. For example, if the design brief contains a set of FAQs presented in an accordion, it's better to name the component `.c-accordion` rather than `.c-faqs` because it's likely the client will be interested in using the same accordion UI component for other purposes. If the FAQ information changed in future to use another UI, a new component could be created if necessary. In some cases a UI component may be tied to its content (such as an event card which features special time/date/location formatting) in which case a name like `.c-event-card` could be the most appropriate.

- <mark>Avoid names that are too generic</mark> which could be mistaken for other potential future components. For example, a name like `.c-form` for a typical form layout made up of label/input pairs could become confusing if a separate UI component was ever needed for a one-line search bar form or email signup form. A name like `.c-search` could become confusing if other search-related components were ever needed.
    - Possibly too generic: `.c-form`, `.c-table`, `.c-search`, `.c-box`.
    - Better names: `.c-field-set`, `.c-long-form`, `.c-data-table`, `.c-search-toggle`, `.c-highlight-box`.

  **Note:** Generic names can be acceptable if new designs would likely only produce variations on the same component. For example, `.c-button` is generic but would likely be extended with modifiers like so: `.c-button--solid`, `c.-button--with-icon`, `.c-button--outlined-red`.

- <mark>Try not to go out of your way to use specific HTML element names</mark> in your class names unnecessarily. An exception to this might be where you want to make it clear that a class should only be applied to a particular HTML element.
    - Acceptable names: `.c-data-table__td` `.c-date-picker__input`, `.c-large-icon__svg`
      (The implementer should be guided to use these classes on these specific elements.)
    - Acceptable names: `.c-photo-card__header`, `.c-footer`, `.c-social-icon__label`.
      (Contain HTML element names by chance but are the most suitable descriptor.
    - Too specific: `.c-content-box__h2`, `.c-accordion__ul`, `.c-contact-details__p`.
      (The implementer could reasonably use another tag.)

## BEM Best Practices

Below are some [BEM](http://getbem.com/introduction/) best practices to keep in mind when coding for NV.

- Avoid nesting elements within other elements of a block, i.e. your class names should be made up of the component name, one optional element name, and one optional modifier name. Name ideas like `.c-search-results__list__item` should be reworked to `.c-search-results__list-item`, or if it makes sense to reuse the list elsewhere, potentially spun off as a separate component.

- Blocks (components or objects) should have no external geometry (i.e. no margins, no absolute/fixed positioning) such that they can be easily reused and included in another parts of the site. External geomtery can be managed through a containing block or with helper classes.

  Simple example implementation:

  ```
  <div class="o-main-layout">
      <header class="c-header  o-main-layout__header">
          <!-- ... -->
      </header>
      <main class="o-main-layout__main-content">
          <div class="c-toolbar  u-mb-md"></div>
          <!-- ... -->
      </main>
  </div>
  ```

  SASS code:
  ```
  /*** in _objects.main-layout.scss ***/
  .o-main-layout {
      height: 100vh;
  }

  .o-main-layout__header {
      height: 6rem;
      left: 0;
      position: fixed;
      right: 0;
      top: 0;
  }

  .o-main-layout__main-content {
      height: calc(100vh - 6rem);
  }

  /*** in _components.header.scss ***/
  .c-header {
      background: #000;
      color: #fff;
  }

  /*** in _components.toolbar.scss ***/
  .c-toolbar {
      border-bottom: 0.25rem solid;
      padding: 1rem;
  }

  /*** in _utilities.layout.scss (???) ***/
  .u-mb-md {
      margin-bottom: 2rem;
  }
  ```

- Keep nesting to a minimum in your SASS to avoid creating CSS selectors which are unnecessarily specific. Instead of nesting classes, leverage BEM to create another element with its own class.

  A new component that needs to be reworked:
  ```
  .c-file-picker {
      border: 0.125em solid;
      position: relative;

      .c-file-picker__button {    // <-- BEM elements shouldn't be nested.
          background: $med-blue;

          &:hover {
              background: $dark-blue;
          }
      }

      input {    // <-- We should give this HTML element a class name
          position: absolute;
      }
  }
  ```

  Much better:
  ```
  .c-file-picker {
      border: 0.125em solid;
      position: relative;
  }

  .c-file-picker__button {
      background: $med-blue;

      &:hover {
          background: $dark-blue;
      }
  }

  .c-file-picker--input {
      position: absolute;
  }
  ```

- Occasionally you may need to change an element's style when the parent block is in a different state. <mark>In this case, scope your styles within the element that changes, not the parent block.</mark>

  This needs to be reworked:
  ```
  .c-menu {
      background: #fff;

      &:hover .c-menu__link {
          color: $dark-blue;
      }
  }

  .c-menu__link {
      color: $medium-blue;
  }
  ```

  Much better:
  ```
  .c-menu {
      background: #fff;
  }

  .c-menu__link {
      color: $medium-blue;

       .c-menu:hover & {
          color: $dark-blue;
      }
  }
  ```

## General Styling Best Practices

### Making Your Site Responsive

Typically, website designs by NV presented to the client will show the website and a selection of key pages as they appear on desktop. Often there are also one or two screens showing how the website would scale down to a mobile phone, but the main selling point is how the website will appear on a large desktop screen.

This can make it a challenge designing new sites in a mobile-first way, as the client's expectation will be a polished view on their desktop, and they may not get around to checking on their phone until the website is live or nearly live.

<mark>As much as possible, try to develop your site from the outset using a mobile-first approach, or start with a desktop-first approach with an eye towards responsiveness, and then later when focusing on responsiveness, amend your SASS code to a more mobile-first approach.</mark>

Breakpoints in media queries should always be handled using the SASS library "[Breakpoint](https://github.com/at-import/breakpoint/wiki/Basic-Media-Queries)" (included by default in the base template). These normally compile to `min-width` media queries:

For example, the following code:

```
@include breakpoint($md) {
    display: block;
}
```
...compiles to something like: 
```
@media (min-width: 768px) {
    display: block;
}
```
where `$md` is one of our globally-defined breakpoints defined in `_settings.breakpoints.scss`.

An example of quickly turning a design component (often desktop-focused) into code:
```
.c-toolbar {
    display: flex;
    flex-flow: row;
}
```
Later amending to be more responsive using breakpoints:
```
.c-toolbar {
    display: flex;
    flex-flow: row wrap;

    @include breakpoint($md) {
        flex-wrap: nowrap;
    }
}
```

### Taking Care to Define Fallbacks

Take care to develop sites with progressive enhancement and graceful degradation in mind. In other words, your site should still be pleasant to use in instances where fonts and background images take a while to load, or fail to load at all.

In particular:
- Make sure `font-family` values don't just contain named fonts. Ensure there is always a fallback keyword (usually `serif` or `sans-serif`) so that the brand is respected as much as possible.
- Select a sensible [font loading strategy](https://www.zachleat.com/web/css-tricks-web-fonts/) for custom fonts so that the user has the best experience possible. Often this will be out of your hands if using a third party like [Google Fonts](https://fonts.google.com/) or [Adobe Fonts](https://fonts.adobe.com/), but keep your options in mind.
- Ensure don't look broken before images and other media such as videos or maps have loaded, or if they fail to load. Try to at least set a subtle `background-color` behind media to keep the layout grounded in these scenarios. This is especially important where you have white text over an image, which will be unreadable until the image appears.
- Anticipate the dimensions of your images to avoid layout jankiness when the images eventually load. This can be as simple as setting the `width` or `height` in your SASS, or controlling the aspect ratio for a more responsive solution using the [padding trick](https://css-tricks.com/aspect-ratio-boxes/).

### Content Flexibility

Design your SASS in such a way that it is flexible to reasonable changes in content. This will help to reduce the need for code tweaks down the line based on content updates. A classic example where this concern should be paramount is the main site navigation, which can sometimes break catastrophically when one extra item is added if not styled in a reasonably flexible manner. 

You also need to handle reasonable amounts of missing content. A safe approach is to structure your SASS treating most elements as optional.

Some things to think about:

- If there is an event item with no categories, will the margins clash when the categories element no longer exists?
- If a blog post card contains no preview text, will it line up as expected with the other cards?
- If this element is no longer wrapped in an optional link (`<a>`) tag, will the layout break?
- Could the client want to make this line of plain text richly editable with bold text or links? Should we reasonably allow for that?
- Does this table work with two extra columns, and if not, how do we handle that in a way that doesn't break?

### Component Flexibility

In order to make our components as reuable as possible we should take care to ensure they lend themselves to anticipated modifications. <mark>If your component has a set height of `8rem` and it needs to be increased to `10rem`, would it be easily updatable in one place, or would a host of nested items require fine tuning?</mark>

Here's one example of a difficult component to update where the client would like the header to be fixed and narrower:
```
.c-header {    // <-- Unspecified height.
    background: $med-red;
    display: flex;
    flex-flow: row;
}

.c-header__logo-img {
    height: auto;  // <-- Header height might depend on this image's dimensions.
    margin: rem(32) 0; // <-- We will need to take care updating this.
    width: rem(100);
}

.c-header__nav-list {
    line-height: rem(50); // <-- We will need to take care updating this.
}

.c-header__nav-item {
    padding: rem(17) 0; // <-- We will need to take care updating this.
}
```

A solution which lends itself better to editing could be:
```
.c-header {
    align-items: center;
    background: $med-red;
    display: flex;
    flex-flow: row;
    height: rem(100); // <-- We can just update this and the layout will update as expected.
}

.c-header__logo-img {
    height: rem(20);
    width: rem(100);
}

.c-header__nav-list {}

.c-header__nav-item {}
````

### Dealing with Rich Text Content

Using the BEM methodology you should generally never need to use an HTML element selector in your SASS, because each element that needs styling within a block should have a class name of its own.

Rich text content, such as that coming from Umbraco, is one case where you have limited control over the HTML output. Generally the typographical elements coming from rich text editors are free of class names.

To style these elements, make sure to wrap all rich text output in HTML with the class `.t-rich-text` and then customise the display of this content in `_typography.rich-text.scss`. This file is not only compiled with your main stylesheet, but also to the rich text editor stylesheet `RichTextEditorStylesheet.css` used within the Umbraco backend, ensuring that the appearance is always in sync.
- Take care to ensure your wrapper `.t-rich-text` has no **external geometry**, i.e. element margins should be clipped at the edges of the wrapper element. Usually this will mean zeroing top margins for `:first-child` elements, and vice-versa for `:last-child`.

- Avoid defining rich text styles in your **elements** SASS folder. <mark>If the paragraph style within `.t-rich-text` is shared with that in some other context (such as in a component) it's better to wrap the latter paragraph instance also in `.t-rich-text`, or refactor those styles into a SASS mixin</mark>.

- Make sure your styles are flexible enough to handle unusual element combinations, such as two headings in a row or a paragraph within a list item. If you don't want to deal with some elements (like tables) make sure they are switched off in the Umbraco rich text editor data type settings.

### <mark>Icons</mark>

Avoid icon libraries such as FontAwesome if possible. It's common for NV designs to include icons from more than one set (such as Material Icons), and some may even be custom. To manage all these icons in a consistent way, try to include them in your project using inline SVG. This way the colour can be easily customised using `fill` and `stroke`, there are no network requests, and they can be styled in a reasonably uniform way irrespective of where they came from.

If there is a different approach better-suited to a particular project you are free to change things up, so long as it is consistent.

### <mark>Colours</mark>

<mark>**Note to Matt**: There isn't much of a standard way we do this right now so we can change and expand upon this.</mark>

Aside from full black and full white (`#000`/ `#fff`), all colours you use should be defined in `_settings.colors.scss` in hexadecimal format. Start with brand colours and other colours featured in the design, and then add different shades of these colours where necessary. For example the design might call for the brand's unique shade of blue in most cases, but you need a darker shade for a hover state.

```
$medium-blue: #3274e0; // <-- Our original colour.
$dark-blue: #245ebd;
$darkest-blue: #10408f;
```
You can also generate these programmatically using a SASS function:
```
$medium-blue: #3274e0;
$dark-blue: darken($medium-blue, 10%);
```
For occasional transparent shades, use SASS's `rgba` function where it's needed, which compiles to CSS `rgba`:
```
.c-header {
    background: rgba($medium-blue, 0.9); // Compiles to rgba(50, 116, 224, 0.9)
}
```

### Custom Fonts

When a custom font is required, store your `@font-face` code in its own settings file, e.g. `_settings.font.helvetica-neue.scss`.

When dealing with multiple font files for weight and style variations, make sure the font-family name is **the same** across them all. Be aware that some font services will provide `@font-face` code where the font-family names are augmented with these settings e.g. `'HelveticaNeue_Bold'`. These suffixes need to be stripped in order for the font to respond to CSS properties like `font-weight` and `font-style`.

### Helper Classes for Margins/Paddings

<mark>There is no typical way of doing this yet (changes from project to project). :(</mark>

### Handling Third Party CSS

<mark>There is no typical way of doing this yet (changes from project to project). :(</mark>
