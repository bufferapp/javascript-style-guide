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
  4. [Whitespace](#whitespace)
  5. [jQuery](#jquery)
  6. [Scope & this](#scope--this)

*Future sections may include: Commas+Semicolons, Comments, 
Conditional Expressions, Variables, Properties*

## Naming Conventions

  - Use camelCase variables and function names. use CAPS_UNDERSCORE for constants.

    ```javascript
    var updateText = 'Point Break is the best movie ever.';
    
    function setUpdateText(arg) { //...
    
    // Constants
    var MAX_TWEET_LENGTH = 140;
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

  - Use a leading _ to denote private methods

    ```javascript
    MyClass.prototype._privateMethod = function(){ //...
    ```

  - Be descriptive in the variables purpose.

    ```javascript
    var isFacebook = profile.get('service') === 'facebook';
    // over...
    var facebook = profile.get('service') === 'facebook';
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

## Whitespace

  - *Currently all of the JavaScript uses tabs, but we may switch to spaces - Discussion pending*

  - Use whitespace with operators

    ```javascript
    var count = x + 2;
    // instead of 
    var count=x+2;
    ```

  - Use indentation for longer method chaining

    ```javascript
    $update
      .text(newText)
      .removeClass('editing')
      .addClass('saved');

    // Instead of
    $update.text(newText).removeClass('editing').addClass('saved');
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

  - Use Promises with $.ajax. jQuery's docs use them as the new default. Use `.then` or `.always` over `.done` and `.fail`

    ```javascript
    $.ajax({ url: '/updates.json' })
      .then(function(data){
        renderUpdates(data.updates);
      }, function(){
        alert('Request failed');
      });

    // Promises also work nicely for wrapping a more complex $.ajax request
    function saveUpdateText(updateId, text) {
      return $.ajax({
        type: 'post'.
        url: '/updates/' + updateId + '.json',
        data: {
          body: text
        }
      });
    }

    // The response
    function showSuccessNotification(data) {
      new Notification(data.message);
    }

    // Usage is very clean and easy to read
    saveUpdateText(123, 'My new update text')
      .then(showSuccessNotification);

    ```

## Scope & this

  - Use `.bind(this)` to pass scope to functions if possible. Use `var self = this;` if that approach works better for your use case

    ```javascript
    someMethod: function(data) {
      this.saveMyUpdate(data)
        .then(function(data){
          this.collection.add(data);
          this.showSavedNotification();
        }.bind(this));
    }

    var myUpdates = updates.filter(function(update){
      return update.type === this.getCurrentType();
    }.bind(this));    
    ```
