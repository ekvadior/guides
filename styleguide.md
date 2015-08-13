# Writing frontend

*Some smart and witty description of what this is.*

**Table of contents**

* [BEM](#BEM)
  * [blocks](#blocks)
    * [block naming](#blockNaming)
    * [block modifiers](#blockModifiers)
    * [block usage](#blockUsage)
  * [elements](#elements)
    * [element naming](#elementNaming)
    * [element modifiers](#elementModifiers)
    * [element usage](#elementUsage)
  * [other notes](#bemNotes)
* [JavaScript](#javascript)
* [Is utility classes](#isClasses)
* [SASS style guide](#sassGuide)
  * [File organization](#fileOrganization)
    * [vendor and overrides](#vendor)
    * [core and config](#coreConfig)
    * [utils](#utils)
    * [components](#components)
    * [pages](#pages)
  * [Nesting](#nesting)
  * [Rules organization](#rulesOrganization)
  * [Variables](#variables)
    * [colors](#colors)
    * [z-index](#zindex)
    * [font-weight](#fontweight)
    * [line-height](#lineheight)
    * [letter-spacing](#letterspacing)
  * [Polyfills](#polyfills)
  * [Formatting](#formatting)
    * [Spacing](#spacing)
    * [Quotes](#quotes)
  * [Performance](#performance)
    * [Specificity](#specificity)

------------------------------------------------------------------------------------------------------------------------------------------------

<a name="bem"></a>
#BEM

**BEM** stands for block - element - modifier, and is a philosophy of naming and structuring HTML and CSS classes. This guide will not only touch on the SASS side of things, but also on the HTML layout.

Basic idea of BEM is to separate content into managable blocks - a type of componentization.
A block is a piece of content that is encapsulated, meaning it doesn't rely on any other part of the page to be complete. Simplest example of a block could be a form. A block can be repeated endlessly on a site and should be able to fit into any other content without breaking too much (or at all, for that matter).

An element is a part of a block. An element on in its own is meaningless and should always be below a block in the HTML tree. An element describes a part of a block, and by combining multiple elements a block is defined.

A Modifier (or modificator) can be put on both the block and element, and is used to change the visual properties of a block or element. It is used so a block/element can be flexibly reused on other parts of a page, where a small modification is required.

*A simple form block using BEM could look like this:*

```html
<form class="login-form">
  <div class="__row">
    <label class="__label">Username:</label>
    <input class="__input __input--username" />
  </div>
  <div class="__row">
    <label class="__label">Password:</label>
    <input class="__input __input--password" />
  </div>
</form>
```

*Where the class "login-form" is the block name, clasess "__row", "__label" and "__input" are elements and classes "__input--username" and "__input--password" are modifiers.*

<a name="blocks"></a>
## Blocks

Syntax: `<block-name>`

<a name="blockNaming"></a>
### Block naming

Block name is a dash separated list of words describing the block and its use.

Care should be taken not to use too abstract or vague names. Names such as "button" or "list" should be avoided as they couldn't be properly defined to fit everywhere. As such you cannot reuse them properly. Too specific names are also not very useful, as they could prevent reuse on other parts of the page.

```html
<div class="my-block">
  ...
</div>
```

```css
.my-block {
  /* ... */
}
```

*Good names:*
```css
.blog-article,
.breadcrumbs,
.pagination,
.avatar-image,
.language-picker
```

*Bad names:*
```css
.list,
.main,
.content,
.avatar-image,
.footer-language-button,
.header-main-navigation-language-button
```

<a name="blockModifiers"></a>
### Block modifiers

Syntax: `<block-name>--<block-modifier-name>`

A block modifier is a class used to modify the appearance of a block in a certain context. A block modifier name is a dash spearated list of names prefixed with the block name and two dashes.

```css

.my-block {
  /* ... */

  &.my-block--my-modifier {
    /* ... */
  }
}
```

```html
<div class="my-block my-block--my-modifier">
  ...
</div>
```

<a name="blockUsage"></a>
### Block usage

Not everything is a block. This is not minecraft. Just because an HTML element has children doesn't necessarily mean it is, or should be a block. If an HTML element, and its styling makes no sense in the layout anywhere outside its position, it is not a block. If an HTML element cannot be reused, it is probably not a block. If an HTML element has no children, it is probably not a block. There are cases where this is not true, as each case is unique.

*A bad block example*
```html
<div class="main-navigation">
  <ul class="__list link-list">
    <li class="__item item">
      <a href="#" class="__link">
    </li>
    <li class="__item item">
      <a href="#" class="__link">
    </li>
  </ul>
</div>
```

```css
.main-navigation {
  /* ... */

  .__list {
    /* ... */
  }
}

.link-list {
  /* ... */

  .__item {
    /* ... */
  }
}

.item {
  /* ... */

  .__link {
    /* ... */
  }
}
```

*The "item" block is not really a block, it is actually only meaningful under the link-list (or even main-navigation) block. As such it should just be an element, even though it has a child element.*

*Fixed example*
```html
<div class="main-navigation">
  <ul class="__list link-list">
    <li class="__item">
      <a href="#" class="__link">
    </li>
    <li class="__item">
      <a href="#" class="__link">
    </li>
  </ul>
</div>
```

```css
.main-navigation {
  /* ... */

  .__list {
    /* ... */
  }
}

.link-list {
  /* ... */

  .__item {
    /* ... */
  }

  .__link {
    /* ... */
  }
}
```

<a name="elements"></a>
## Elements

Syntax: `<block-name>__<element-name>`
Syntax-shortened: `__<element-name>`

<a name="elementNaming"></a>
### Element naming

Element name is a dash separated list of words describing the element and its use and purpose under its block. It is prefixed with the block name and two underscores or just two underscores as a shorthand.

Unlike blocks, element naming can be abstract and vague, as it is only meaningful under its parent block. Still, care should be taken to have names that describe the element's use in a block. There is no point in using the block name in the element name, as that would only bring redundancy.

```html
<div class="my-block">
  <div class="__my-element">
    ...
  </div>
</div>
```

```css
.my-block {
  /* ... */

  .__my-element {
    /* ... */
  }
}
```

*Good names:*
```css
.__item,
.__link,
.__title,
.__row,
.__column
```

*Bad names:*
```css
.__blog-article-title,
.__header-list,
.__element /* Too vague even for an element */
```

<a name="elementModifiers"></a>
### Element modifiers

Syntax: `<block-name>__<element-name>--<element-modifier-name>`
Syntax-shortened: `__<element-name>--<element-modifier-name>`

An element modifier is a class used to modify the appearance of a element in a certain context. A element modifier name is a dash spearated list of names prefixed with the element name and two dashes.

```css

.my-block {
  /* ... */

  .__my-element {
    /* ... */

    &.__my-element--my-modifier {
      /* ... */
    }
  }
}
```

```html
<div class="my-block">
  <div class="__my-element __my-element--my-modifier">
    ...
  </div>
</div>
```

<a name="elementUsage"></a>
### Element usage

Elements only make sense inside a block. Never should an element be without its block, either in HTML or SASS. When reusing the block, its elements don't necessarily have to be used in the same order, or at all.

*Bad elements example*
```html
<div class="statistics-list">
  <div class="__statistic statistic">
    <span class="__plain-text">Over</span>
    <h1 class="highlighted">
      <span class="__text">22</span>
    </h1>
    <span class="__plain-text">years of experience.</span>
  </div>
  <div class="__statistic __statistic--no-first statistic">
    <h1 class="highlighted">
      <span class="__text">6</span>
    </h1>
    <span class="__plain-text">Countries</span>
  </div>
</div>
```

```css
.statistics-list {
  /* ... */

  .__statistic {
    /* ... */

    &.__statistic--no-first {
      /* ... */
    }
  }
}

.statistic {
  /* ... */

  .__plain-text {
    /* ... */
  }
}

.highlighted {
  /* ... */

  .__text {
    /* ... */
  }
}
```

*The "highlighted" block is not really a block, it is an element, as it can only live inside of the statistic block. Also, the "__text" element it houses also makes sense only inside the statistic, so it is an element of statistic, and not highlighted.*
*The "__statistic--no-first" modifier doesn't make sense, as the modifier should be applied to the statistic block. The "__statistic" element shouldn't worry about its content implementation, it should only have styles based on its position and appearance inside the "statistics-list" block.*

*Fixed example*
```html
<div class="statistics-list">
  <div class="__statistic statistic">
    <span class="__plain-text">Over</span>
    <h1 class="__highlighted">
      <span class="__text">22</span>
    </h1>
    <span class="__plain-text">years of experience.</span>
  </div>
  <div class="__statistic statistic statistic--no-first">
    <h1 class="__highlighted">
      <span class="__text">6</span>
    </h1>
    <span class="__plain-text">Countries</span>
  </div>
</div>
```

```css
.statistics-list {
  /* ... */

  .__statistic {
    /* ... */

  }
}

.statistic {
  /* ... */

  &.statistic--no-first {
    /* ... */
  }

  .__plain-text {
    /* ... */
  }

  .__highlighted {
    /* ... */
  }

  .__text {
    /* ... */
  }
}
```

<a name="bemNotes"></a>
### Other notes

* An HTML element can be a block and an element, and can have any number of modifiers for both of those, but will never be 2 or more blocks, or 2 or more elements.
* A block should never care about its position on the page, only about its apperance. If a block needs to be positioned inside the block it is in, it is probably also an element of the parent block.
* A modifier should not be used for a dynamic way of changing the appearance of a block or an elemnent. An is-class is used for that. As a modifier contains the block name it should not be selected through JavaScript, as well as any other block or element class.


<a name="javascript"></a>
## JavaScript

syntax: `js-<identifier>`

JavaScript specific classes are used to reduce the risk of breaking JavaScript behaviour and/or functionality. A JavaScript specific class should **never have any styling on it**, as it is used only as a selector for JavaScript. Any element that is needed to be selected directly from code should have a JavaScript specific class.

*Example:*
```html
<a href="/login" class="__button __button--login js-login"></a>
```

<a name="isClasses"></a>
## Is utility classes

syntax: `is-<state-name|action-name>`

Is classes should be used to make dynamic changes to the appearance of blocks or elements that are controlled through JavaScript. For example when you have a block that you will be showing and hiding through JavaScript, you would add an "is-shown" class to the block when it is shown.
Is classes unlike js classes should have styling, but they shouldn't be used without JavaScript, as modifiers exist for the static styling.

*Example:*
```html
<a href="/login" class="__button __button--login js-login is-shown is-hovered"></a>
```

------------------------------------------------------------------------------------------------------------------------------------------------

<a name="sassGuide"></a>
## SASS style guide

For a reference of sass functionality refer to this page: [SASS reference](http://sass-lang.com/documentation/file.SASS_REFERENCE.html)

*This should cover almost every case when writing sass, including file structures, imports, code organization and logic*

<a name="fileOrganization"></a>
## File organization

In all web projects we usually have some kind of a css/sass folder, naming of which can sometimes be chosen. Usual names would be:

* Stylesheets
* Styles
* CSS
* SASS

If needed, brush up on sass partials and imports, as those are important to understand the file structure.
If the app is a Rails app, sprockets can be used instead of imports, differences will be noted when necessary.

Because of the BEM philosophy, files will usually follow the strucutre of components (that being blocks). An example full structure would be:

* **vendor**
  * normalize.css
  * other-vendor.css
  * ...
* **overrides**
  * normalize.scss
  * other-vendor.css
  * ...
* **utils**
  * _colors.scss
  * _z-indexes.scss
  * _variables.scss
  * _media.scss
  * _placeholders.scss
  * _mixins.scss
  * _functions.scss
* **components**
  * _header.scss
  * _nav-list.scss
  * _article.scss
  * ...
* **pages**
  * _homepage.scss
  * _about-us.scss
  * _some-other-page.scss
  * ...
* _config.scss
* _core.scss
* application.scss

Only the main file (in example application.scss) should be a normal sass file, everything else should be a partial. This is so only one css file is generated, and when a file is not imported, it will not be unnecessarily compiled.

The application.scss file should only hold imports, no actual rules, styling, variables, mixins etc. can go in it. Usually it would look something like:

```css
@import 'vendor/**/*';
@import 'overrides/**/*'
/* special imports */
@import 'susy'; //for example

@import 'utils/**/*';
@import 'config';

@import 'core';

@import 'components/**/*';
@import 'pages/**/*';
```

If everything is done correctly, load order shouldn't matter, so usage of wildcards is recommended in sass-rails projects. If using gulp-sass it might not work (currently it doesn't) so files need to be imported individualy. A recommended way of solving the libsass problem would be using folder name files to load. For example structure:

* **folder**
  * _first.scss
  * _second.scss
* _folder.scss
* application.scss

The folder scss file would contain:

```css
@import 'folder/first';
@import 'folder/second';
```

And the application scss file would contain:

```css
@import 'folder';
```

If using sprockets, application.scss file would contain the rails way of importing using require. As such variables and other defines won't carry over to files, so they would have to be @imported manually as needed.

<a name="vendor"></a>
### Vendor and overrides

The vendor folder should have css or scss files that are gotten from outside sources. As these files have nothing to do with the application styling, they should be loaded first. Any global overrides for the said files should go into the override folder, with the same name as the vendor file, so they can easily be found when necessary.

<a name="coreConfig"></a>
### Core and config

The config file should contain any configuration for the imported vendors (Usual example would be susy), or other configuration for user defined mixins, functions  etc.

The core file should contain and globaly defined styles for the application. That could mean setting a global font-size, adding global box-sizing and etc. The core file should not contain any configuration or mixins, variables and such, only styling (or includes of such).

<a name="utils"></a>
### Utils

<a name="components"></a>
### Components

<a name="pages"></a>
### Pages

<a name="variables"></a>
## Variables

Syntax: `<property>-<value>[--componentName]`

Variable names in our CSS are also strictly structured. This syntax provides strong associations between property, use, and component.

The following variable defintion is a color property, with the value grayLight, for use with the highlightMenu component.

```css
@color-grayLight--highlightMenu: rgb(51, 51, 50);
```

<a name="colors"></a>
### Colors

When implementing feature styles, you should only be using color variables provided by colors.less.

When adding a color variable to colors.less, using RGB and RGBA color units are preferred over hex, named, HSL, or HSLA values.

**Right:**
```css
rgb(50, 50, 50);
rgba(50, 50, 50, 0.2);
```

**Wrong:**
```css
#FFF;
#FFFFFF;
white;
hsl(120, 100%, 50%);
hsla(120, 100%, 50%, 1);
```

<a name="zindex"></a>
### z-index scale

Please use the z-index scale defined in z-index.less.

`@zIndex-1 - @zIndex-9` are provided. Nothing should be higher then `@zIndex-9`.


<a name="fontweight"></a>
### Font Weight

With the additional support of web fonts `font-weight` plays a more important role than it once did. Different font weights will render typefaces specifically created for that weight, unlike the old days where `bold` could be just an algorithm to fatten a typeface. Obvious uses the numerical value of `font-weight` to enable the best representation of a typeface. The following table is a guide:

Raw font weights should be avoided. Instead, use the appropriate font mixin: `.font-sansI7, .font-sansN7, etc.`

The suffix defines the weight and style:

```CSS
N = normal
I = italic
4 = normal font-weight
7 = bold font-weight
```

Refer to type.less for type size, letter-spacing, and line height. Raw sizes, spaces, and line heights should be avoided outside of type.less.


```CSS
ex:

@fontSize-micro
@fontSize-smallest
@fontSize-smaller
@fontSize-small
@fontSize-base
@fontSize-large
@fontSize-larger
@fontSize-largest
@fontSize-jumbo
```

See [Mozilla Developer Network — font-weight](https://developer.mozilla.org/en/CSS/font-weight) for further reading.


<a name="lineheight"></a>
### Line Height

Type.less also provides a line height scale. This should be used for blocks of text.


```CSS
ex:

@lineHeight-tightest
@lineHeight-tighter
@lineHeight-tight
@lineHeight-baseSans
@lineHeight-base
@lineHeight-loose
@lineHeight-looser
```

Alternatively, when using line height to vertically center a single line of text, be sure to set the line height to the height of the container - 1.

```CSS
.btn {
  height: 50px;
  line-height: 49px;
}
```

<a name="letterspacing"></a>
### Letter spacing

Letter spacing should also be controlled with the following var scale.

```CSS
@letterSpacing-tightest
@letterSpacing-tighter
@letterSpacing-tight
@letterSpacing-normal
@letterSpacing-loose
@letterSpacing-looser
````

<a name="polyfills"></a>
## Polyfills

mixin syntax: `m-<propertyName>`

At Medium we only use mixins to generate polyfills for browser prefixed properties.


An example of a border radius mixin:

```css
.m-borderRadius(@radius) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
          border-radius: @radius;
}
```


<a name="formatting"></a>
## Formatting

The following are some high level page formatting style rules.

<a name="spacing"></a>
### Spacing

CSS rules should be comma seperated but live on new lines:

**Right:**
```css
.content,
.content-edit {
  ...
}
```

**Wrong:**
```css
.content, .content-edit {
  ...
}
```

CSS blocks should be seperated by a single new line. not two. not 0.

**Right:**
```css
.content {
  ...
}
.content-edit {
  ...
}
```

**Wrong:**
```css
.content {
  ...
}

.content-edit {
  ...
}
```


<a name="quotes"></a>
### Quotes

Quotes are optional in CSS and LESS. We use double quotes as it is visually clearer that the string is not a selector or a style property.

**Right:**
```css
background-image: url("/img/you.jpg");
font-family: "Helvetica Neue Light", "Helvetica Neue", Helvetica, Arial;
```

**Wrong:**
```css
background-image: url(/img/you.jpg);
font-family: Helvetica Neue Light, Helvetica Neue, Helvetica, Arial;
```

<a name="performance"></a>
## Performance

<a name="specificity"></a>
### Specificity

Although in the name (cascading style sheets) cascading can introduce unnecessary performance overhead for applying styles. Take the following example:

```css
ul.user-list li span a:hover { color: red; }
```

Styles are resolved during the renderer's layout pass. The selectors are resolved right to left, exiting when it has been detected the selector does not match. Therefore, in this example every a tag has to be inspected to see if it resides inside a span and a list. As you can imagine this requires a lot of DOM walking and and for large documents can cause a significant increase in the layout time. For further reading checkout: https://developers.google.com/speed/docs/best-practices/rendering#UseEfficientCSSSelectors

If we know we want to give all `a` elements inside the `.user-list` red on hover we can simplify this style to:

```css
.user-list > a:hover {
  color: red;
}
```

If we want to only style specific `a` elements inside `.user-list` we can give them a specific class:

```css
.user-list > .link-primary:hover {
  color: red;
}
```
