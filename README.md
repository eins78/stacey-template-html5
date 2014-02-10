stacey-template-html5
=====================

html5 template for stacey (https://github.com/kolber/stacey)

download: <https://github.com/eins78/stacey-template-html5/archive/master.zip>

demo: <http://mfa.178.is>

This is a **blank** template (no style), made to be [customizable, extensible and forkable](#template-structure).

- **html5** (semantic, [valid](http://validator.w3.org/check?uri=http%3A%2F%2Fmfa.178.is%2F;outline=1))!
- **style it just with CSS**!
- lots of **optional metadata**! ([read more](#html-structure))
- **configureable** with data
- 1 `base.html` template, all other templates [extend](http://twig.sensiolabs.org/doc/templates.html#template-inheritance) it ([read more](#template-structure))


## HTML Structure

If you want to style it with `CSS`, this is the list of `element`s, `.class`es and `#id`s, and how they are nested.
The structure aims to be minimal, with just enough markup to make the most common layouts possible in pure CSS.

- `head`
- `body`
    - `div.wrapper` 
        - `header`
        - `nav#primary`
        - `article`
    - `footer`

## Template Structure

If you want to change something about the template not possible in `CSS`, it is easy to extend and modify it in a maintainable way.

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

- ["I am serious artist but would prefer this to be paper" (`sans-serif`)](http://www.google.com/fonts/#ReviewPlace:refine)

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

- if language: `<html lang="">`
- feed title in head