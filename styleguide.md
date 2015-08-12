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
* [JavaScript](#javascript)
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
  …
</div>
```

```css
.my-block {
  /* … */
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

A block modifier is a class used to modify the appearance of a block in a certain context. A block modifier name is a dash spearated list of names.

```css

.my-block {
  /* … */

  &.my-block--my-modifier {
    /* … */
  }
}
```

```html
<div class="my-block my-block--my-modifier">
  …
</div>
```

<a name="componentName-descendantName"></a>
### componentName-descendantName

A component descendant is a class that is attached to a descendant node of a component. It's responsible for applying presentation directly to the descendant on behalf of a particular component. Descendant names must be written in camel case.

```html
<article class="tweet">
  <header class="tweet-header">
    <img class="tweet-avatar" src="{$src}" alt="{$alt}">
    …
  </header>
  <div class="tweet-body">
    …
  </div>
</article>
```

<a name="is-stateOfComponent"></a>
### componentName.is-stateOfComponent

Use `is-stateName` for state-based modifications of components. The state name must be Camel case. **Never style these classes directly; they should always be used as an adjoining class.**

JS can add/remove these classes. This means that the same state names can be used in multiple contexts, but every component must define its own styles for the state (as they are scoped to the component).

```css
.tweet { /* … */ }
.tweet.is-expanded { /* … */ }
```

```html
<article class="tweet is-expanded">
  …
</article>
```

<a name="javascript"></a>
## JavaScript

syntax: `js-<identifier>`

JavaScript specific classes are used to reduce the risk of breaking JavaScript behaviour and/or functionality. A JavaScript specific class should **never have any styling on it**, as it is used only as a selector for JavaScript. Any element that is needed to be selected directly from code should have a JavaScript specific class.

*Example:*
```html
<a href="/login" class="__button __button--login js-login"></a>
```

<a name="variables"></a>
## Variables

Syntax: `<property>-<value>[--componentName]`

Variable names in our CSS are also strictly structured. This syntax provides strong associations between property, use, and component.

The following variable defintion is a color property, with the value grayLight, for use with the highlightMenu component.

```CSS
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
  …
}
```

**Wrong:**
```css
.content, .content-edit {
  …
}
```

CSS blocks should be seperated by a single new line. not two. not 0.

**Right:**
```css
.content {
  …
}
.content-edit {
  …
}
```

**Wrong:**
```css
.content {
  …
}

.content-edit {
  …
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
