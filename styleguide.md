# SASS style guide

*Guides are not rules and should not be followed blindly. Use your head and think.*

**Table of contents**

* [SASS style guide](#sassGuide)
  * [File organization](#fileOrganization)
    * [vendor and overrides](#vendor)
    * [core and config](#coreConfig)
    * [components](#components)
    * [pages](#pages)
    * [utils](#utils)
      * [colors](#utilsColors)
      * [z-indexes](#utilsZIndex)
      * [variables](#utilsVariables)
      * [media](#utilsMedia)
      * [placeholders](#utilsPlaceholders)
      * [mixins](#utilsMixins)
      * [functions](#utilsFunctions)
  * [Code quality](#codeQuality)
    * [Nesting](#nesting)
    * [Specificity](#specificity)
    * [Rule order](#ruleOrder)
    * [Formatting](#formatting)

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

Because of the BEM philosophy, files will usually follow the strucutre of components (that being blocks). An example of a structure would be:

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
  * _shared-variables.scss
  * _media.scss
  * _placeholders.scss
  * _mixins.scss
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

@import 'config';
@import 'utils/**/*';

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

The core file should contain and globally defined styles for the application. That could mean setting a global font-size, adding global box-sizing and etc. The core file should not contain any configuration or mixins, variables and such, only styling (or includes of such).

<a name="components"></a>
### Components

The components folder contains blocks that are shared between pages. Those could be form definitions, button definitions and such. Each components has to be in its own file, with the name of the file matching that of the block name.

<a name="pages"></a>
### Pages

The pages folder contains an scss file for each page on a site. The styles inside should be specific to that page, while still following the bem philosophy. Any blocks that would be shared between pages fit into the components folder in a separate file, and not inside a specific page. The name of the file should match the page handle.

<a name="utils"></a>
### Utils

* [colors](#utilsColors)
* [z-indexes](#utilsZIndex)
* [shared variables](#utilsVariables)
* [media](#utilsMedia)
* [placeholders](#utilsPlaceholders)
* [mixins](#utilsMixins)

<a name="utilsColors"></a>
### Colors

The colors file should only contain color variables of the base and global kind.
All colors for blocks or elements should go into their local file on the top.

Base colors are actual color values used on the site, they will be used by both global colors and block-elements colors. Syntax and example:

Syntax: `base-[value]-color`

```css
$base-red-color: #FF0000;
```

Only in base colors is the name of the color ok to use.

Global colors are color to be used for block or element level colors. They should contain base colors.

Syntax: `[primary, secondary, ternary ...]-<property>-color`

```css
$primary-color: $base-red-color;
$primary-text-color: $base-light-gray-color;
```

Block element colors go into their respective files on the top, and are defined as such:
Syntax: `[block-name][__element-name]-<property>-color`

```css
$some-block-background-color: $base-red-color;
$some-block__some-element-color: $primary-text-color;
```

Text is the only property that can be omitted, as it the property name in css is color as well.

<a name="utilsZIndex"></a>
### Z-indexes

When using z-indexes, use z-index variables defined in z-indexes file.
```css
$z-base: inital;
$z-modal: 10;
$z-loader: 20;
$z-modal-raised: 30;
$z-notification: 40;
$z-debug: 1000;

//For use in local situations only
$z-1: 1;
$z-2: 2;
$z-3: 3;
$z-4: 4;
$z-5: 5;
```

<a name="utilsVariables"></a>
### Shared variables

This file should only contain variables that will be shared across scss files, and doesn't include colors, z-indexes, media query or similiar.
Example:

```css
$header-height: 250px;
```

<a name="utilsMedia"></a>
### Media queries

Media file should contain the variables and mixins used to define the media queries and breakpoints on the site. Usage of the media mixin on
*link-to-mixin* is highly recommended. Otherwise, syntax is as follows.

```css
$breakpoint-tablet: '(max-width: 991px)';

@mixin tablet() {
  @media #{$breakpoint-tablet} {
    @content
  }
}
```

<a name="utilsPlaceholders"></a>
### Placeholders

The placeholders file should contain globaly shared pseudclasses (or placeholders). Pseudoclasses in sass are classes beginning with %, which generate no css
output, but can be extended using sass directive @extend.
Example usage of a global placeholder.

```CSS
%container {
  @include container;
  width: $content-width-large;

  @include desktop {
    width: $content-width-desktop;
  }
  @include tablet {
    width: $content-width-tablet;
  }
  @include mobile {
    width: $content-width-mobile;
  }
}

//Or with media mixin
%container {
  @include container;
  width: $content-width-large;

  @include media(desktop) {
    width: $content-width-desktop;
  }
  @include media(tablet) {
    width: $content-width-tablet;
  }
  @include media(mobile){
    width: $content-width-mobile;
  }
}

````

<a name="utilsMixins"></a>
### Mixins

The mixins file should contain any global mixin that can help organize sass better.
For instance generating font styles.

```css
@mixin font-title() {
  font-size: 38px;
  font-family: Arial;
  font-weight: bold;
}
```

<a name="codeQuality"></a>
## Code quality

Use of scss-lint is very recommended to force nesting, specificity, rule order and other rules in the following chapter.

* [Nesting](#nesting)
* [Specificity](#specificity)
* [Rule order](#ruleOrder)

<a name="nesting"></a>
### Nesting

Nesting improves code readability, but must be taken into moderation. Usual rule is to nest no more than 3 times, with few exceptions.
Usual example of three level nesting is seen in a simple block element with a modifier.

```css
.block {
  //level 1 nesting

  .block__element {
    //level 2 nesting

    &.block__element--modifier {
      //level 3 nesting
    }
  }
}
```

<a name="specificity"></a>
### Specificity

Specificity is a bit more complex than nesting. With specificity you have to take into account the generated css output. Each level of nesting
adds a level of specificity to a selector. Adding multiple selectors on a single line also adds to specificity, such as:

```css
  //level 1 specificity
  .button {

  }

  //level 2 specificity
  .button.button-white {

  }
  //or
  .button {
    .button-white {

    }
  }

  //level 3 specificity
  .button.button-white.button-really-white {

  }
  //or
  .button.button-white {
    .button-really-white {

    }
  }
```

Specificity should be 4 levels maximum.

<a name="ruleOrder"></a>
### Rule order

Rule order inside a selector are as follows:

* extends
* includes without @content
* properties
* nested properties
* includes with content
* pseduoclasses (e.g. :hover)
* pseudoelements
* parent selector modifiers (e.g. &.is-active)
* children element selectors

------------------------------------------------------------------------------------------------------------------------------------------------
