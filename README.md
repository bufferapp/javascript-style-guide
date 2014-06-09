# Buffer JavaScript Style Guide

*A work in progress inspired by [AirBnB's Style Guide](https://github.com/airbnb/javascript).*

## Goal

To establish more consistency and readability across the Buffer team and our
open source projects.

## Ideals

> **Have a bias toward clarity** - from [The Buffer Values](http://www.slideshare.net/Bufferapp/buffer-culture-04)

  - We should always aim to write code that is clear and readable.
  - Use whitespace. Add comments liberally where needed, but strive to write code that's clear and self documenting
  - Always try to write code that clearly demonstrates and communicates it's intent.

## Table of Contents

  1. [Naming Conventions](#naming-conventions)
  2. [Strings](#strings)
  3. [Blocks](#blocks)
  4. [jQuery](#jquery)

*Future sections may include: Whitespace, Commas+Semicolons, Comments, 
Conditional Expressions, Variables, Properties*

## Naming Conventions

**TODO** *Currently the codebase is split between camelCase and under_score. 
We should push towards a unified style.*

  - Be descriptive in the variables purpose.

    ```javascript
    var isFacebook = profile.get('service') === 'facebook';
    // over...
    var facebook = profile.get('service') === 'facebook';

    var 
    ```

  - Use PascalCase or CapFirst when creating a constructor or extending a 
    Backbone class. Avoid using a capital first letter unless it's one of these.

    ```javascript
    function Profile(service){
      this.service = service;
    }

    var ProfileView = Backbone.View.extend({
      events: {}
    });
    ```

## Strings

  - Use single quotes `''` over double quotes for strings.

    ```javascript
    var update = 'Some text';
    ```

  - Strings longer than 80 characters should be written across multiple lines 
    using string concatenation. Use `+` for concatenation, avoid escaping EOLs
    with `\`. 

    ```javascript
    var update = 'Grounds, half and half, affogato sit medium, decaffeinated ' +
        'cortado, acerbic whipped grinder cultivar aftertaste. Sugar, wings ' +
        'robusta barista, seasonal robust, mazagran qui blue mountain organic ' +
        'breve arabica.';

    // or use Array#join
    var update = [
        'Grounds, half and half, affogato sit medium, decaffeinated ',
        'cortado, acerbic whipped grinder cultivar aftertaste. Sugar, wings ',
        'robusta barista, seasonal robust, mazagran qui blue mountain organic ',
        'breve arabica.'
    ].join('');
    ```

## Blocks

  - Use curlys `{}` for multi-line blocks, add spacing around statements to 
    encourage readability.

    ```javascript
    if ( update.get('text').length < 140 ) {
      update.save();
    } else {
      $('.js-warning').show();
    }

    // It's ok to drop the braces if it's a short one-liner, try to keep it 
    // under ~80 characters long so it's readable
    if ( update.get('text') > 140 ) this.showLengthWarning();

    ```

## jQuery

  - Use `.js-` prefixed class selectors. This prevents confusion between 
    classes needed for design and ones used to reference the DOM from js.

    ```html
    <div class="update js-update-content">Some update text</div>
    ```

    ```javascript
    var $content = $('.js-update-content');
    $content.text('New text');
    ```

  - Prefix jQuery object variables with a `$`.

    ```javascript
    var $sidebar = $('.js-sidebar');
    ```

  - Cache jQuery lookups.

    ```javascript
    // Use this...
    var $sidebar = $('.js-sidebar');
    $sidebar.addClass('hidden');
    $sidebar.data('id', '1234');

    // Instead of
    $('.js-sidebar').addClass('hidden');
    $('.js-sidebar').data('id', '1234');
    ```
