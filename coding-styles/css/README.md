# Factory Media CSS / SCSS Style Guide

Inspired by: [GitHub](http://primercss.io/guidelines/), [Trello](https://gist.github.com/bobbygrace/9e961e8982f42eb91b80), [Airbnb](https://github.com/airbnb/css), [ITCSS](https://www.youtube.com/watch?v=1OKZOV-iLj4), [BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) & [@mdo's Code Guide](http://codeguide.co/#css-syntax)

  1. [Basics](#basics)
  1. [Formatting](#formatting)
  1. [Declaration order](#declaration-order)
  1. [Comments](#comments)
  1. [BEM And OOCSS](#bem-and-oocss)
  1. [SCSS Ordering Of Property Declarations](#scss-ordering-of-property-declarations)
  1. [Mixins](#mixins)
  1. [Placeholders](#placeholders)
  1. [Namespacing CSS With Prefixes](#namespacing-css-with-prefixes)
  1. [JavaScript](#javascript)
  1. [Media Query Placement](#media-query-placement)
  1. [Mobile First Media Queries](#mobile-first-media-queries)
  1. [Browser Prefixes](#browser-prefixes)
  1. [Shorthand Notation](#shorthand-notation)
  1. [Operators In Sass](#operators-in-sass)
  1. [Units](#units)
  1. [Miscellanious](#miscellanious)


## Basics
+ We use SASS, specifically the [**SCSS** flavour](http://sass-lang.com/guide), never the sass syntax.
+ To keep our CSS readable, we try and keep our CSS very vanilla (plain).
+ We use **SCSS**, but only `@import`s, `$variables`, and some `@mixin`s.
+ We use `@import`s to break up our files into more manageable components. `$variables` and `@mixin`s are available everywhere and it all outputs to a single file. We occasionally use nesting, but only for very shallow things like `&:hover` & media query mixins. We don't use more complex functions like loops and very rarely use `@extends`.
+ We use **<abbr title="Block Element Module">BEM</abbr>** class names and organise our projects with **<abbr title="Inverted Triangle CSS">ITCSS</abbr>** in mind.


## Formatting

+ Use two space indents, not tabs.
+ Use dashes, not camelCasing in class names. Underscores are only OK for BEM Modules `.main-nav__item {}` (see [BEM and OOCSS](#bem-and-oocss) below).
+ Do not use ID selectors `#hash-is-bad {}`.
+ Put spaces after `:` in property declarations `color:∙#f00;`.
+ Put spaces before `{` in rule declarations `.selector∙{}`.
+ Put line breaks between rulesets.
+ Grouped selectors should be on their own single line.
+ Place the `}` closing braces of declaration blocks on a new line.
+ Each declaration should appear on its own line for more accurate error reporting.
+ End all declarations with a semi-colon `;`. The last declaration's is optional, but your code is more error prone without it.
+ Comma-separated property values should include a space after each comma `box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;`.
+ Don't include spaces after commas within: `rgb()`, `rgba()`, `hsl()`, `hsla()`, or `rect()` values. This helps differentiate multiple color values (comma, no space) from multiple property values (comma with space) `background-color: rgba(0,0,0,.5);`.
+ Don't prefix property values or color parameters with a leading zero: `.5` instead of `0.5` and `-.5px` instead of `-0.5px`.
+ Don't include units for zero values: `margin: 0;` instead of `margin: 0px;`.
+ Lowercase all hex values: `#fff`. Lowercase letters are much easier to discern when scanning a document as they tend to have more unique shapes.
+ Use shorthand hex values where available, `#fff` instead of `#ffffff`.
+ Quote attribute values in selectors, using double quotes `input[type="text"]`.
+ Never reference `.js-` prefixed class names in CSS. `.js-` are used **exclusively in JavaScript** files.

##### Bad
```scss
.avatar{
  border-radius:50%;
  border:2px solid white; }
.no, .nope, .not_good {
  // ...
}
#oi-definitely-no-IDs {
  // ...
}
.js-prefix-not-in-css {
  // ...
}
```

##### Good
```scss
.avatar {
  border-radius: 50%;
  border: 2px solid #fff;
}

.one,
.selector,
.per-line {
  // ...
}
```


## Declaration Order
Related property declarations should be grouped together following the order:

1. Positioning
2. Box model
3. Typographic
4. Visual

Positioning comes first because it can remove an element from the normal flow of the document and override box model related styles. The box model comes next as it dictates a component's dimensions and placement.

Everything else takes place inside the component or without impacting the previous two sections, and thus they come last.

```scss
.declaration-order {
  // Positioning
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;

  // Box-model
  display: block;
  float: right;
  width: 100px;
  height: 100px;

  // Typography
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;

  // Visual
  background-color: #f5f5f5;
  border: 1px solid #e5e5e5;
  border-radius: 3px;

  // Misc
  opacity: 1;
}
```
**TODO**: include linter like RECESS / SassBeautify / CSScomb


## Comments
Comments rarely hurt. If you find an answer on Stack Overflow or in a blog post, add the link to a comment so future people know what’s up. It’s good to explain the purpose of the file in a comment at the top.

+ Prefer line comments (`//...` in Sass-land) to block comments `/* ... */`.
+ Write detailed comments for code that isn't self-documenting:
  + Uses of z-index
  + Compatibility or browser-specific hacks


## BEM And OOCSS
+ It helps create clear, strict relationships between CSS and HTML
+ It helps us create reusable, composable components
+ It allows for less nesting and lower specificity
+ It helps in building scalable stylesheets

**BEM**, or “Block-Element-Modifier”, is a naming convention for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.
+ CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
+ Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

**OOCSS**, or “Object Oriented CSS”, is an approach for writing CSS that encourages you to think about your stylesheets as a collection of “objects”: reusuable, repeatable snippets that can be used independently throughout a website.
+ Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
+ Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/an-introduction-to-object-oriented-css-oocss/)


**Example**
```html
<article class="listing-card listing-card--featured">

  <h1 class="listing-card__title">Adorable 2BR in the sunny Mission</h1>

  <div class="listing-card__content">
    <p>Vestibulum id ligula porta felis euismod semper.</p>
  </div>

</article>
```

```scss
.listing-card { }
.listing-card--featured { }
.listing-card__title { }
.listing-card__content { }
```

+ `.listing-card` is the **Block** and represents the higher-level component.
+ `.listing-card__title` is an **Element** and represents a descendant of `.listing-card` that helps compose the block as a whole.
+ `.listing-card--featured` is a **Modifier** and represents a different state or variation on the `.listing-card` block.


## SCSS Ordering Of Property Declarations
1. `@extend` declarations
    Just as in other OOP languages, it's helpful to know right away that this `class` inherits from another.

    ```scss
    .btn-green {
      @extend %btn;
      // ...
    }
    ```

2. Property declarations
    Now list all standard property declarations, anything that isn't an `@extend`, `@include`, or a nested selector.

    ```scss
    .btn-green {
      @extend %btn;
      background: green;
      font-weight: bold;
      // ...
    }
    ```

3. `@include` declarations
    Grouping `@include`s at the end makes it easier to read the entire selector, and it also visually separates them from `@extend`s.

    ```scss
    .btn-green {
      @extend %btn;
      background: green;
      font-weight: bold;
      @include clearifix;

      @include breakpoint-up(md) {
        font-size: 1.2em;
      }
      // ...
    }
    ```

4. Nested selectors
    Try not to nest selectors where possible. _If necessary_, they should go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .btn {
      @extend %btn;
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      &:hover {
        background: #000;
      }

      .icon {
        margin-right: 10px;
      }
    }
    ```

## Mixins
Mixins, defined via `@mixin` and called with `@include`, should be used sparingly and only when function arguments are necessary. A mixin without function arguments (i.e. `@mixin hide { display: none; }`) is better accomplished using a placeholder selector (see below) in order to prevent code duplication.

## Placeholders
Placeholders in Sass, defined via `%selector` and used with `@extend`, are a way of defining rule declarations that aren't automatically output in your compiled stylesheet. Instead, other selectors “inherit” from the placeholder, and the relevant selectors are copied to the point in the stylesheet where the placeholder is defined. This is best illustrated with the example below.

Placeholders are powerful but easy to abuse, especially when combined with nested selectors. **As a rule of thumb, avoid creating placeholders with nested rule declarations, or calling `@extend` inside nested selectors.** Placeholders are great for simple inheritance, but can easily result in the accidental creation of additional selectors without paying close attention to how and where they are used.

**Sass**

```sass
// Unless we call `@extend %icon` these properties won't be compiled!
%c-icon {
  font-family: "fontawesome";
}

.c-icon--error {
  @extend %c-icon;
  color: red;
}

.c-icon--success {
  @extend %c-icon;
  color: green;
}
```

**CSS**

```css
.c-icon--error,
.c-icon--success {
  font-family: "fontawesome";
}

.c-icon--error {
  color: red;
}

.c-icon--success {
  color: green;
}
```


## Namespacing CSS With Prefixes
We prefix almost all CSS classes with a single character to quickly differentiate them
+ `.c-`: **Component** - This is a concrete, implementation-specific piece of UI. All of the changes you make to its styles should be detectable in the context you’re currently looking at. Modifying these styles should be safe and have no side effects. `.c-btn`
+ `.o-`: **Object** - It may be used in any number of unrelated contexts to the one you can currently see it in. Making modifications to these types of class could potentially have knock-on effects in a lot of other unrelated places. Tread carefully. `.o-grid`
+ `.u-`: **Utillity** - It has a very specific role (often providing only one declaration) and should not be bound onto or changed. It can be reused and is not tied to any specific piece of UI. `.u-clearfix`
+ `.js-`: **JavaScript** - Signify that this piece of the DOM has some behaviour acting upon it, and that JavaScript binds onto it to provide that behaviour. If you’re not a developer working with JavaScript, leave these well alone. `.js-toggle-primary-nav`
+ `.is-, .has-`: **State** -  It tells us that the DOM currently has a temporary, optional, or short-lived style applied to it due to a certain state being invoked. `.is-open`

For more info [read this in-depth namespacing guide](http://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/) by Harry Roberts


## JavaScript
Separate style and behavior concerns by using `.js-` prefixed classes for behavior.

For example:

``` HTML
<div class="content-nav">
  <a href="#" class="content-nav__button js-open-content-menu">
    Menu
  </a>
</div>
```

``` JavaScript
$('.js-open-content-menu').on('click', function(e){
  openMenu();
});
```

Why do we want to do this? The `.js-` class makes it clear to the next person changing this template that it is being used for some JavaScript event and should be approached with caution.

Be sure to **use a descriptive class name**. The intent of `.js-open-content-menu` is more clear than `.js-menu`. A more descriptive class is less likely to conflict with other classes and it’s a lot easier to search for. The class should almost always include a verb since it’s tied to an action.

**`.js-` classes should never appear in your stylesheets**. They are for JavaScript only. Inversely, there is never a reason to see presentation classes like `.content-nav__button` in JavaScript. You will see state classes like `.is-open` in your JavaScript and your stylesheets as `.c-component.is-open`.


## Media Query Placement
Place media queries as close to their relevant rule sets whenever possible. Don't bundle them all in a separate stylesheet or at the end of the document. Doing so only makes it easier for folks to miss them in the future. Here's a typical setup.

Use a  `@mixin` like: `@include media-breakpoint-up(lg)` / `@include media-breakpoint-down(md)` / `@include breakpoint(tablet)` within the element rule set

```scss
.element { ... }
.element-avatar { ... }
.element-selected { ... }

@media (min-width: 480px) {
  .element { ...}
  .element-avatar { ... }
  .element-selected { ... }
}

// OR

.element {
  font-size: 1.5em;

  @include media-breakpoint-up(md) {
    font-size: 2rem;
  }
 }
```

## Mobile First Media Queries
Always style for mobile first. Media queries should be used to adjust for larger devices, so generally you should almost always use the '-up' media queries `@include media-breakpoint-up()` not '-down'.


## Browser Prefixes
**Don't do it!** [Autoprefixer](https://www.npmjs.com/package/gulp-autoprefixer) should be added to the project to take care of this.

Autoprefixer, as the name suggests will automatically add all relevant browser prefixes (`-moz`, `-webkit`, `-o`, `-ms`), so keep them out of your source files.


## Shorthand Notation
Strive to limit use of shorthand declarations to instances where you must explicitly set all the available values. Common overused shorthand properties include:

+ `padding`
+ `margin`
+ `font`
+ `background`
+ `border`
+ `border-radius`

Often times we don't need to set all the values a shorthand property represents. For example, HTML headings only set top and bottom margin, so when necessary, only override those two values. Excessive use of shorthand properties often leads to sloppier code with unnecessary overrides and unintended side effects.

The Mozilla Developer Network has a great article on [shorthand properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Shorthand_properties) for those unfamiliar with notation and behavior.

```css
/* Bad example */
.element {
  margin: 0 0 10px;
  background: red;
  background: url("image.jpg");
  border-radius: 3px 3px 0 0;
}

/* Good example */
.element {
  margin-bottom: 10px;
  background-color: red;
  background-image: url("image.jpg");
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
}
```

## Operators In Sass
For improved readability, wrap all math operations in parentheses with a single space between values, variables, and operators.

**Bad**
```css
.element {
  margin: 10px 0 @variable*2 10px;
}
```

**Good**
```css
.element {
  margin: 10px 0 (@variable * 2) 10px;
}
```

## Units
Try to avoid using fixed pixel values and use REMs or EMs where possible. In order of preferred use:
+ **rem**: `margin`, `padding`, `font-size`
+ **%**: `width`, `position`
+ **em**: `font-size`, `line-height`
+ **vh**, **vw**: `height`, `width`
+ **px**: `border-width`, `border-radius`, `box-shadow`


## Miscellanious
**Leave the place nicer than you found it**. Our CSS isn't always in great shape so when updating CSS you should rewrite sections according to these rules.

Some additional things to keep in mind:

+ In your markup, order classes: `<div class="c-component c-component--modifier u-util-class is-state js-action"></div>`.
  + Block / Element
  + Modifiers
  + Utils
  + State
  + JS

+ Explicitly write out class names in selectors. Don’t concatenate strings or use preprocessor trickery to build a class name. We want to be able to search for class names. This goes for `.js-` classes in JavaScript, too.
+ If you are worried about long selector names making our CSS huge, don’t be. Compression takes care of this.
