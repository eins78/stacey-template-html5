# `stacey-template-html5`

html5 template for stacey (https://github.com/kolber/stacey)

download: <https://github.com/eins78/stacey-template-html5/archive/master.zip>

demo: <http://mfa.178.is>

This is a **blank** template (no style), made to be [customizable, extensible and forkable](#template-structure).

- **html5** (semantic, [valid](http://validator.w3.org/check?uri=http%3A%2F%2Fmfa.178.is%2F;outline=1))!
- **style it just with CSS**!
- lots of **optional metadata**! ([read more](#html-structure))
- **configureable** with data
- 1 `base.html` template, all other templates [extend](http://twig.sensiolabs.org/doc/templates.html#template-inheritance) it ([read more](#template-structure))


## Table of Contents

- [Goals](#goals)
- [Styling with `CSS`](#styling-with-css)
    - [HTML Structure](#html-structure)
    - [Example: *sidebar layout*](#example-sidebar-layout)
    - [Example: *style specific parts only*](#example-style-specific-parts-only)
- [Templating](#templating)
    - [HTML*5*?](#html5)
    - [Template Structure](#template-structure)
    - [inheritance](#inheritance)
    - [blocks](#blocks)
        - [insert blocks](#insert-blocks)
        - [partials](#partials)
    - [overview](#overview)
    - [Examples](#examples)
        - [override and insert blocks](#override-and-insert-blocks)
- [*Pro-Tips®*](#pro-tips®)
    - [webfonts](#webfonts)
    - [debugging](#debugging)
- [TODO](#todo)


## Goals 

(not reached yet)

- Minimum possible markup
- Use semantic `HTML5` elements where applicable
- Only add `class`es and `id`s where neccessary for eindeutigkeit or styling
- Common template options can be set from config (`shared.yml` or any sub-page…)
- Ship some default styles: 1-3 main stylesheets inspired by the old stacey templates which build on (`@import) common modules.
- Fully commented and documented `CSS` source
- For all interesting data points in listings, add appropriate classes (for use with `list.js`)
- Don't rely on JavaScript for anything - js plugins extending the functionality should also extend the markup themselfes <small>(looking at you, old .gallery-nav)</small>
- Use `HTML5`'s `data-foo` properties



## Styling with `CSS`

The structure aims to be minimal, with just enough markup to make the most common layouts possible in pure CSS.

There are **two ways** to customize your site:

1. **Use the default style, make small adjustments** (recommended for end-users)
  - there is already a **'`user.css`'** where you can fill out declarations for common things like fonts, colors
  - some things can also be configured, look at the example `_shared.yml`
  - add anything else you might need
  
2. **Built you own stylesheet from scratch.**
  - the default style is split up into small modules, so you can pick and choose and not really need to start from scratch
  - you probably want to `@import` at least `base.css`, possibly also `typo.css` and `colors.css`
  - in any case, you can use the default styles to look up some declarations



### HTML Structure

For reference, his is the list of `element`s, `.class`es and `#id`s, and how they are nested. **Bold** elements are the ones you are most likely to use for making a layout.

- `head`
- **`body`**
    - `div#wrapper` 
        - **`header`**
            - `hgroup`
                - `h1`
                  - `small`
                - `div.email`
        - `hr`
        - **`nav#primary`** (e.g. sidebar)
            - `ul`
                - `li`
                    - `ul` … (child menu)
        - **`article`**
            - `section#main`
                - (main content)
            - `aside#subpages-top`
            - `section#media`
              - `figure`
                - `img`
                - `figcaption`
            - `aside#subpages-bottom`
            - `hr`
    - **`footer`**

The list above uses `CSS`-style identifiers, so you can just use (or combine) them in your stylesheet as needed.

### Example: *sidebar layout*

````css
nav#primary {
  width: 300px;
}
article {
  width: 700px;
}
````

### Example: *style specific parts only*

````css
/* square marker: for ALL lists */
ul {
  list-style-type: square;
}
/* no markers: ONLY in navigation*/
nav ul {
  list-style-type: none;
}
/* use monospace captions*/
figure figcaption {
  font-family: monospace;
}
````

## Templating

If you want to change something about the template not possible in `CSS`, it is easy to extend and modify it in a maintainable way.

### HTML*5*?

If you use `HTML` but think you don't "know" `HTML`**5**, here is everything you need to know (in this context):  
We now have more (semantic) elements and properties, so we need fewer classes and other tricks, and there is less "cruft" in general (like the crazy doctype).

Wording Cheatsheet: ""

- Valid `HTML`**4**:

    ````html
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
      "http://www.w3.org/TR/html4/strict.dtd">
    <html>
      <head></head>
      <body>
        <div class="header">
        Hello World!
      </div> <!-- end header -->
      <div class="post">
        <span class="time">31. 12. 1999
          <span class="timestamp hidden">1999-31-12</span>
        </span>
        Lorem Ipsum
      </div> <!-- end post -->
    </body>
    ````

- Valid `HTML`**5**:

    ````html
    <!DOCTYPE html>
    <body>
      <header>Hello World!</header>
      <article>
        <time datetime="1999-31-12">31. 12. 1999</time>
        Lorem Ipsum
      </article>
    </body>
    ````

Also, you are allowed to "invent" your own properties, as long as they start with `data-`. 
One *could* use it for styling, but it is most useful for scripting. 
This is another reason we need fewer classes, 
and it also potentially makes the markup less confusing 
for designers since all the classes that remain have a semantic meaning
(an are only used when there is no equivalent element).

Example: 

*`HTML`:* (a very simple list)

````html
<ul data-gallery="carousel">
  <li><img src="foo.jpg"></li>
  <li><img src="foo.jpg"></li>
</ul>
<script type="text/javascript">
$('[data-gallery="carousel"]').each(function () {
  alert($(this));
});
</script>
````

*`CSS`:* 

(This is how you could style your own data – but most likely these kind of styles will already be included with the javascript that needs them.)

````css
[data-gallery="carousel"] li {
  list-item-style: none;
  display: inline;
}
[data-gallery="carousel"] img{
  float: left;
}
````


### Template Structure

### inheritance

The base structure for all other templates is defined in **`base.html`**.
Other templates then *inherit* from it by declaring `{% extends "base.html" %}`.

### blocks

The base template is structured into `block`s, which follow the semantic structure of the document. 
They can be overridden by child templates, by defining a `block` with the same name.

#### insert blocks

There are additional blocks to enable easy inserting of content from child templates without overriding the original block. 
Just a define a block by the same name, followed by `_before` or `_after` for inserting outside it; and `_head` or `_tail` for inserting inside it.

#### partials

Blocks that have a more complicated inner structure themselves, are broken out into `partials`, and `includ`ed in the templates.
Partials that are directly used in `base.html` start with an underscore (i.e. `_header`, `_footer`).


### overview

- `<head>`
    - *(default head stuff)*
    - `block head_tail` *(insert more stylesheets etc. here…)*
- `</head>`

- `<body>`
    - `block container_before`
    - `<div.container>`
      - `block container_head`

      - `block header_before`
      - **partial: `_header.html`**
        - `block header_head`
        - *default header stuff*
        - `block header_tail`

      - `block header_after`
        - default: `<hr>`

      - `block navigation_before`
      - `block navigation` (includes `partials/_navigation.html`)
      - `block navigation_after`

      - `block content_before`
      - `<div id="content">`
        - `block content_head`
        - `block content` ***(mainly for usage in child templates)***
        - `block content_tail`
      - `</div>`
      - `block content_after`
        - default: `<hr>`

      - `block footer_before`
      - **partial: `_footer.html`**
          - `<footer>`
            - `block footer_head`
            - **`block footer`**
            - `block footer_tail`
          - `</footer>`
      - `block footer_after`

    - `</div> <!-- end container  -->`
  - `block container_after` *(include more scripts etc. here…)*
    
- `</body>`


### Examples 

#### override and insert blocks

The content section in pages is overriden by defining a new `block content`.

**page.html**:

````html5
<!-- extend base template -->
{% extends "base.html" %}

<!-- override "content" block -->
{% block content %}
  <p id="date">&para;</p>
  <div id="description">
    <h2 id="title"><a href="{{ page.root_path }}">{{ page.title }}</a></h2>
    {% include 'partials/assets/images.html' %}
    {{ page.content }}
    {% include 'partials/assets/pdfs.html' %}
  </div>
{% endblock %}

<!-- insert scripts after "container" -->
{% block container_after %}
  <script src="{{ page.root_path }}/public/docs/js/jquery-1.3.2.js" type="text/javascript" charset="utf-8"></script>
{% endblock %}
````

## *Pro-Tips®*

### webfonts

If you want to use a font in your style tht is not normally installed on any computer, you have to embed that font. google has a [catalogue of fonts](http://www.google.com/webfonts/). 

Be careful to choose readable font for the main text and use more fancy fonts for titles and navigation only.  

Some choices:

- ["I am serious artist" (`sans-serif`, all families have lot's off different weigths)](http://www.google.com/fonts/#ReviewPlace:refine/Collection:Alegreya+Sans+SC|Source+Sans+Pro|Lato|Alegreya+Sans|Exo+2|Roboto|Titillium+Web|Open+Sans|Roboto+Condensed)

- ["I am serious artist but would prefer this to be paper" (`sans-serif`)](http://www.google.com/fonts/#ReviewPlace:refine/Collection:Neuton|Alegreya:400italic,400|Crimson+Text:400,400italic|Vollkorn:400italic,400|Gentium+Book+Basic:400,400italic|Volkhov)

- ["I am different" (for titles etc.)](http://www.google.com/fonts/#ReviewPlace:refine/Collection:Nova+Round|Kelly+Slab|Nova+Flat|Offside|Supermercado+One|Source+Code+Pro|Nova+Square|Megrim|Exo+2|Pathway+Gothic+One|Geo|Montserrat) 

- ["I am playfull" (for titles etc.)](http://www.google.com/fonts/#ReviewPlace:refine/Collection:Knewave|Sarina|Hanalei+Fill|Erica+One|Caesar+Dressing|Corben|Fugaz+One|Sniglet|Passero+One|Fredoka+One|UnifrakturCook:700|Dynalight|Comfortaa|Titan+One|Bowlby+One+SC|Changa+One|Patua+One|Righteous|Carter+One|Oregano|Vampiro+One|VT323|Seaweed+Script)

How to use them: you get a small snippet from google and put that in the first line of your css. 

- After you've selected your fonts (like from any link above), click on 'Use' in the bottom right.
- Step 1: Select the exact weight of every font family you want to use, and scroll down to.
- Step 2: Select any more languages you might need
- Step 3: click on the '@import' tab and copy the `CSS` snippet


### debugging

- `<code>{{ _context|json_encode(constant('JSON_PRETTY_PRINT')) }}</code>`: dump the data a template receives in JSON format



## TODO

- if language: `<html lang="{{ lang }}">`
- feed title in head
- nav#primary: mark li.active

- stacey split:
    - clone stacey
    - git-split folders: 
        - app -> stacey-php(app?)
        - template -> template-html5
        - public -> to templates???
        - content -> to fixture/example_content?
    - new base repo uses submodules (git-hooks.sh?)
    - set version to 5 ;)
    - script: make a starterkit.zip
        - attach to every github release
    
- follow aria recommendations (accessibility)