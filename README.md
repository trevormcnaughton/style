# PL Framework & Style Guide
## Codename: Barbara

## Outline<a name='outline'> </a>

  1. [Resources & Background](#resources)
  1. [Project Settings](#project_settings)
  1. [HTML Markup](#markup)
  1. [CSS](#css)
      - [Preprocessors](#preprocessors)  
      - [Variable Naming](#css-variable-naming)  
      - [File Prep](#css-file-prep)
      - [Patterns](#css-patterns)  
  1. [Javascript](#js)
      - [Patterns](#js-patterns)
      - [Frameworks & Libraries](#js-libraries)
  
***

## Project Settings <a name='project_settings'> </a>

  The project scaffolding will vary depending on the type of application and its scope. The client-side code should however, remain consistent.
  
  The following demonstrates a basic WCST project structure:
```unicode
├── public
│   ├── css
│   │   ├── app
│   │   │   └── app.css
│   │   └── scss
│   │       ├── _settings.scss
│   │       └── app.scss
│   ├── img
│   │   └── ui
│   ├── index.html
│   ├── js
│   │   ├── lib
│   │   └── templates
│   └── templates
```
**<sub>[Back to top](#outline)</sub>**

***


* * *
  CSS selector naming should be `-` dash separated and give preference to classes over ID's
```html
<!-- Nein -->
<div class="theBlueBox"></div>
<div class="theBluebox"></div>
<div class="the_blue_box"></div>

<!-- Ja -->
<div class="the-blue-box"></div>
```
**<sub>[Back to top](#outline)</sub>**

* * *

## CSS Coding Style <a name='css'> </a>
  
Following [OOCSS](http://oocss.org/) principles improves code comprehension, and improves team-wide maintainability. Concerns for both styling and behavior as dictated by the CSS should be encapsulated and kept D.R.Y.!

### Other Rules
- Use soft-tabs with a two space indent.
- Put spaces after : in property declarations.
- Put spaces before { in rule declarations.
- Use hex color codes #000 unless using rgba.
- Use // for comment blocks (instead of /* */).
  
* * *
  
  When defining styles of a selector, separate layout/positioning, presentation/type, and functionality within tags. This is a good example (see more about [SASS Code Style](#sass-practices) Below).
```css
  .mr-box {
    position: relative;
    margin: 10% auto;
    width: 25%;
    height: 2em;
    float: left;
    
    font-size: 1.4em;
    font-weight: 200;
    text-transform: uppercase;
    text-align: center;
    
    opacity: 0.5;
    
    transform: translate3d(0, 2%, 0);
    transition: transform 122ms ease-out;
  }
  
  .mr-box:hover {
    opacity: 1;
    
    transform: translate3d(0, 4%, 0);
  }
```

**<sub>[Back to top](#outline)</sub>**
      
* * *
## Pixels vs. Ems
This one is for Jacob

Use px for font-size, because it offers absolute control over text. Additionally, unit-less line-height is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the font-size.

### Preprocessors & Frameworks <a name='preprocessors'> </a>
  We use SASS as a Preprocessor and Compass to make it more awesomer.

**<sub>[Back to top](#outline)</sub>**

* * *

### SASS Best Practices & Coding Style <a name='sass-practices'> </a>
Inspired by on Chris Coyer and Github

- Any $variable or @mixin that is used in more than one file should be put in globals/. Others should be put at the top of the file where they're used.
- As a rule of thumb, don't nest further than 3 levels deep. If you find yourself going further, think about reorganizing your rules (either the specificity needed, or the layout of the nesting).

#### List @extends First
```scss
.foo {
  @extends %thing; 
  ...
}
```
#### Then Normal CSS
```scss
.foo {
  @extends %thing;

	color: #FFF;
}
```

#### Then @includes
```scss
.foo {
  @extends %thing;

	color: #FFF;

	@include box-sizing( border-box );
	@include border-radius( $global-radius );
}
```
Like the OOCSS keep @extend's and @include's grouped.

#### List nested selectors last
```scss
.foo {
  @extends %thing;

	color: #FFF;

	@include border-radius( $global-radius );
}
```
List nothing else after nested selectors. Except selectors of the same level.

#### Maximum Nesting: Three Levels Deep
```scss
.weather {
  .cities {
    li {
      // best not get more specific!
    }
  }
}
```



#### Variable Naming <a name='css-variable-naming'> </a>
  
```scss
  
  /**
  * Colors
  * Use generic, yet descriptive color declarations
  * Do not use standard CSS color names (http://www.w3schools.com/cssref/css_colornames.asp)
  * Use dashed syntax for consistency with CSS style
  **/
  
  /* Bad */
  $white: white;
  $white: #FFF;
  
  $coolblue: blue;
  $coolblue: #0066FF;
  
  /* Good */
  $base-white: #FEFEFE;
  $brand-blue: #0066FF;
  
  /* -------- */
  
  /**
  * Measurements
  * Use specific and descriptive names for measurements
  **/
  
  /* Bad */
  $f-height: 6em;
  $scrollersize: 50%;
  
  /* Good */
  $footer-height: 6em;
  $hero-scrollwer-width: 50%;

```
**<sub>[Back to top](#outline)</sub>**

* * *

#### File Prep <a name='css-file-prep'> </a>
List Vendor/Global Dependancies First, Then Author Dependancies, Then Patterns, Then Parts

```scss

//Libraries
@import "compass";
@import "compass/css3";

//Normalizer
@import "normalizer";

//Settings & Utilities
@import "global-settings";
@import "global-utilities";

//PL Boilerplates
@import "pl-forms";
@import "pl-buttons";
@import "pl-grid";

//Theme Files
@import "pl-sell-site";

//Shame on you
@import "shame";

```
**<sub>[Back to top](#outline)</sub>**

* * *


## Javascript <a name='js'> </a>
  *[ S e p a r a t e ]   [ c o n c e r n s ]   [ i n t o ]  [ m o d u l e s ]  [ ! ]*
  
  * * *
  
### Patterns <a name='js-patterns'> </a>
  
  Avoid pollution of the global namespace as much as possible! If using AMD, consider tying application level-events and configurations to a re-useable `app` object which can then be exported and passed around as needed.
  
``` javascript

  // app.js
  define(['someDependency'], function () {
    
    var app = {
      api: {
        urls: {
          'people': {url:'/people'},
          'places': {url: '/places'},
          'things': {url: '/things'}
        }
      },
      
      getApiUrl: function (req) {
        return this.api.urls[req].url;
      }
    };
    
    // Export the app object
    return app;
  });
  
  
  // Other modules throughout your app can then use app
  define(['app'], function(app) {
      console.log( app.getApiUrl('people') ); // '/people'
  });

```

  If not using AMD, attach a single namespace reference to the window object for app-level access. Our example above could be re-written as:

``` javascript

  // app.js
  (function (window) {
    
    var app = window.app || {};
    
    app.api = {
      urls: {
        'people': {url:'/people'},
        'places': {url: '/places'},
        'things': {url: '/things'}
      }
    };
    
    app.getApiUrl = function (req) {
      return this.api.urls[req];
    };
    
  })(window, undefined);
  
  // Other JS files (added *after* app.js) can then access the app object
  (function (window) {
    console.log( app.getApiUrl('things') ); // '/things'  
  })(window, undefined);
  

```
**<sub>[Back to top](#outline)</sub>**

* * *
