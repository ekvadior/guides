# SASS style guide

*Some smart and witty description of what this is.*

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
* [functions](#utilsFunctions)

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

```css

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

------------------------------------------------------------------------------------------------------------------------------------------------
