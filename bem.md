# BEM

*Guides are not rules and should not be followed blindly. Use your head and think.*

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
  <div class="login-form__row">
    <label class="login-form__label">Username:</label>
    <input class="login-form__input login-form__input--username" />
  </div>
  <div class="login-form__row">
    <label class="login-form__label">Password:</label>
    <input class="login-form__input login-form__input--password" />
  </div>
</form>
```

*Where the class "login-form" is the block name, clasess "login-form__row", "login-form__label" and "login-form__input" are elements and classes "login-form__input--username" and "login-form__input--password" are modifiers.*

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
  <ul class="main-navigation__list link-list">
    <li class="link-list__item item">
      <a href="#" class="item__link">
    </li>
    <li class="link-list__item item">
      <a href="#" class="item__link">
    </li>
  </ul>
</div>
```

```css
.main-navigation {
  /* ... */

  .main-navigation__list {
    /* ... */
  }
}

.link-list {
  /* ... */

  .link-list__item {
    /* ... */
  }
}

.item {
  /* ... */

  .item__link {
    /* ... */
  }
}
```

*The "item" block is not really a block, it is actually only meaningful under the link-list (or even main-navigation) block. As such it should just be an element, even though it has a child element.*

*Fixed example*
```html
<div class="main-navigation">
  <ul class="main-navigation__list link-list">
    <li class="link-list__item">
      <a href="#" class="link-list__link">
    </li>
    <li class="link-list__item">
      <a href="#" class="link-list__link">
    </li>
  </ul>
</div>
```

```css
.main-navigation {
  /* ... */

  .main-navigation__list {
    /* ... */
  }
}

.link-list {
  /* ... */

  .link-list__item {
    /* ... */
  }

  .link-list__link {
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

Element name is a dash separated list of words describing the element and its use and purpose under its block. It is prefixed with the block name and two underscores or just two underscores as a shorthand. Usage of full name is recommended as overriding behaviour might occur otherwise.

Unlike blocks, element naming can be abstract and vague, as it is only meaningful under its parent block. Still, care should be taken to have names that describe the element's use in a block. There is no point in using the block name in the element name, as that would only bring redundancy.

```html
<div class="my-block">
  <div class="my-block__my-element">
    ...
  </div>
</div>
```

```css
.my-block {
  /* ... */

  .my-block__my-element {
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

  .my-block__my-element {
    /* ... */

    &.my-block__my-element--my-modifier {
      /* ... */
    }
  }
}
```

```html
<div class="my-block">
  <div class="my-block__my-element my-block__my-element--my-modifier">
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
  <div class="statistics-list__statistic statistic">
    <span class="statistic__plain-text">Over</span>
    <h1 class="highlighted">
      <span class="highlighted__text">22</span>
    </h1>
    <span class="statistic__plain-text">years of experience.</span>
  </div>
  <div class="statistics-list__statistic statistics-list__statistic--no-first statistic">
    <h1 class="highlighted">
      <span class="highlighted__text">6</span>
    </h1>
    <span class="statistic__plain-text">Countries</span>
  </div>
</div>
```

```css
.statistics-list {
  /* ... */

  .statistics-list__statistic {
    /* ... */

    &.statistics-list__statistic--no-first {
      /* ... */
    }
  }
}

.statistic {
  /* ... */

  .statistic__plain-text {
    /* ... */
  }
}

.highlighted {
  /* ... */

  .highlighted__text {
    /* ... */
  }
}
```

*The "highlighted" block is not really a block, it is an element, as it can only live inside of the statistic block. Also, the "__text" element it houses also makes sense only inside the statistic, so it is an element of statistic, and not highlighted.*
*The "__statistic--no-first" modifier doesn't make sense, as the modifier should be applied to the statistic block. The "__statistic" element shouldn't worry about its content implementation, it should only have styles based on its position and appearance inside the "statistics-list" block.*

*Fixed example*
```html
<div class="statistics-list">
  <div class="statistics-list__statistic statistic">
    <span class="statistic__plain-text">Over</span>
    <h1 class="statistic__highlighted">
      <span class="statistic__text">22</span>
    </h1>
    <span class="statistic__plain-text">years of experience.</span>
  </div>
  <div class="statistics-list__statistic statistic statistic--no-first">
    <h1 class="statistic__highlighted">
      <span class="statistic__text">6</span>
    </h1>
    <span class="statistic__plain-text">Countries</span>
  </div>
</div>
```

```css
.statistics-list {
  /* ... */

  .statistics-list__statistic {
    /* ... */

  }
}

.statistic {
  /* ... */

  &.statistic--no-first {
    /* ... */
  }

  .statistic__plain-text {
    /* ... */
  }

  .statistic__highlighted {
    /* ... */
  }

  .statistic__text {
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
