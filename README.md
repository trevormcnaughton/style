# WCST Style Guide & Patterns

## Hi, there!
*This is how we do it*
...kind of. The following is an overview of our general front end workflow here at [We Can't Stop Thinking](http://wcst.com). You will find a few design patterns, syntax rules, and code snippets that are the result of several years of trial and error.

***

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
  
***

## Resources & Background <a name='resources'> </a>
Instead of re-writing a bunch of well-done guides and ideas, we decided to point you to them directly. Here are a few style-guides and pattern references to look over at some point.

  - Javascript
    - [Airbnb JS Style Guide](https://github.com/airbnb/javascript)
    - [Addy Osmani - Learning Javascript Design Patterns](http://addyosmani.com/resources/essentialjsdesignpatterns/book/)
  - CSS
    - [OOCSS](http://oocss.org/) 
  - PHP:
    - [Fig Standards](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md)

**<sub>[Back to top](#outline)</sub>**

***
## Project Settings <a name='project_settings'> </a>

  The project scaffolding will vary depending on the type of application and its scope. The client-side code should however, remain consistent.
  
  The following demonstrates a basic WCST project structure:

``` unicode
.
├── public
│   ├── css
│   │   ├── app
│   │   │   └── app.css
# If using SCSS/SASS :
│   │   └── scss
│   │       ├── _settings.scss
│   │       └── app.scss
# Or, if using LESS :
│   │   └── less
│   │       ├── app.less
│   ├── img
│   │   └── ui
│   ├── index.html
│   ├── js
│   │   ├── lib
│   │   └── templates
│   └── templates

```

**<sub>[Back to top](#outline)</sub>**

***

## HTML Markup <a name='markup'> </a>
  Selectors that are intened for consumption via JS only shoule be prefixed with an `_` underscore
```html
<!-- Nope -->
<div id="selectorName"></div>
<div id="selector_name"></div>

<!-- Yep -->
<div id="_selector_name"></div>
```

**<sub>[Back to top](#outline)</sub>**

* * *

  Presentational classes and ids should be `_` underscore separated
```html
<!-- Nein -->
<div id="_redbox"></div>
<div id="_redBox"></div>
<div id="redBox"></div>
<div id="redbox"></div>

<div class="theRedBox"></div>
<div class="theredbox"></div>
<div class="_theredbox"></div>

<!-- Ja -->
<div id="red_box"></div>
<div class="the_red_box"></div>
```

* * *

## CSS <a name='css'> </a>
  
  Following [OOCSS](http://oocss.org/) principles not only improves code comprehension, but it improves team-wide maintainability. Concerns for both styling and behavior as dictated by the CSS should be encapsulated and kept DRY!
  
* * *
  
  When defining styles of a selector, separate layout/positioning, presentation/type, and functionality within tags
```css
  #charlie_conway_bio {
    position: relative;
    margin: 10% auto;
    width: 25%;
    height: 2em;
    float: left;
    
    font-size: 1.4em;
    font-weight: 200;
    text-transform: uppercasel
    text-align: center;
    
    opacity: 0.5;
    
    transform: translate3d(0, 2%, 0);
    transition: transform 122ms ease-out;
  }
  
  #charlie_conway_bio:hover {
    opacity: 1;
    
    transform: translate3d(0, 4%, 0);
  }
```  
  * * *
  
### Preprocessors & Frameworks <a name='preprocessors'> </a>
  While a majority of our projects use [LESS](http://lesscss.org/) for CSS pre-compilation, [SCSS/SASS](http://sass-lang.com) has been introduced into the workflow. WCST generally uses [Foundation](http://foundation.zurb.com) and in some instances [Twitter bootstrap](http://twitter.github.io/bootstrap) when intiating a new project or application.
  
#### Variable Naming <a name='css-variable-naming'> </a>
  
```scss
  
  /**
  * Colors (LESS or SCSS)
  * Use generic, yet descriptive color declarations
  * Do not use standard CSS color names (http://www.w3schools.com/cssref/css_colornames.asp)
  * Use camelCase for color names to ensure differentiation for CSS defaults
  * **Note** this snippet uses the `$` to denote a SCSS variable, LESS uses `@`
  **/
  
  /* Bad */
  $white: white;
  $white: #FFF;
  
  $coolblue: blue;
  $coolblue: #0066FF;
  
  /* Good */
  $baseWhite: #FEFEFE;
  $coolBlue: #0066FF;
  
  /* -------- */
  
  /**
  * Measurements
  * Use specific and decriptive names for measurements
  * Use camelCase
  **/
  
  /* Bad */
  $fHeight: 6em;
  $scrollersize: 50%;
  
  /* Good */
  $footerHeight: 6em;
  $heroScrollwerWidth: 50%;

```

#### File Prep <a name='css-file-prep'> </a>
Whether using a collection of individual files and compiling via `@import()` of `$include`, keep similar rules and styles grouped.  
  Define each segment of styles with `/* === <name> === */` and insert at least 5 lines above
```scss
/* === Globals === */
html, body {
  height: 100%;
}

h1 {
  font-weight: 200;
}





/* ===
*
* Example Of
* A multi-line CSS comment
*
=== */
#app {
  transform: rotate(98deg);
}




/* === Nav View (#nav_view) === */
#nav_view {
  #nav_view_slider {
    width: 50%;
    a {
      text-decoration: none;
    }
  }
}




/* === Footer (footer) === */
footer {
  /* footer styles here */
}

```

#### Patterns <a name='css-patterns'> </a>

  When using LESS, serve @2x sprites for retina displays. Create a global class of `.sprite` and define rules at the introduction of your document:
  
```css
@sprite: '/path/to/sprite.png';
@sprite2x: '/path/to/sprite@2x.png';

/* Declare .sprite class */
.sprite{
  display: block;
  background: url(@sprite) no-repeat;
  text-indent: -9999em;
}

/* Retina display media query */
@media only screen and (-webkit-min-device-pixel-ratio: 1.5),
       only screen and (min--moz-device-pixel-ratio: 1.5),
       only screen and (min-resolution: 240dpi){

  .sprite{
    background-image: url(@sprite2x);
    background-size: 440px 280px; /* 1/2 the size of the original */
  }
}
```

**<sub>[Back to top](#outline)</sub>**

* * *

## Javascript <a name='js'> </a>
  *Modularity and separation of concerns is key!*
  
  * * *
  
### Patterns
  
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
  * * *
  
### Frameworks & Libraries
  Depending on the scope of the project and the depth of requirements, the set of libraries and frameworks will change. The followin list represents libraries most commonly used in WCST project and applications.
  
  **DOM access & manipulation (default to vanilla JS first)**  
    - [jQuery](http://jquery.com)  
    - [Zepto](http://zeptojs.com)
  
  **Utility**  
    - [LoDash](http://lodash.com) – Drop in replacement for Underscore.js  
    - [Underscore.js](http://underscorejs.org)
    
  **Architecture/Routing/Events**  
    - [Backbone.js](http://backbonejs.org)  
    - [RequireJS](http://requirejs.org)
    
  **UI**  
    - [jQuery.transit](http://ricostacruz.com/jquery.transit)
    
  **Templating**  
    - [Handlebars.js](http://handlebarsjs.com)
    
  **Scaffolding & Build**  
    - [Grunt](http://gruntjs.com)  
    - [Yeoman](http://yeoman.io)
  
  **Node.js**  
    - Typically, our Node.js applications use [Express](http://expressjs.com) to fire up HTTP servers
