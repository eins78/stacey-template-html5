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

If you want to style it with `CSS`, this is the list of `element`s, `.class`es and `#id`s, and how they are nested:

- `head`
- `body`
    - `div.container`
        - `header`
        - `ul#navigation`
        - `div#content`
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



## TODO

- if language: `<html lang="">`
- feed title in head