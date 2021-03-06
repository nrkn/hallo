Hallo - contentEditable for jQuery UI
=====================================

Hallo is a very simple in-place rich text editor for web pages. It uses jQuery UI and the [HTML5 contentEditable functionality](https://developer.mozilla.org/en/rich-text_editing_in_mozilla) to edit web content.

The widget has been written as a simple and liberally licensed editor. It doesn't aim to replace popular editors like [Aloha](http://aloha-editor.org), but instead provide a smaller and less full-featured option. It is also intended to serve as a testbed for Aloha's [jQuery UI migration](https://github.com/alohaeditor/Aloha-Editor/issues/55), and so ideas and code from Hallo may flow back to the Aloha project.

## Using the editor

You need jQuery and jQuery UI loaded. An easy way to do this is to use Google's JS service:

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.5.2/jquery.min.js"></script>
<script src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.8.12/jquery-ui.min.js"></script>
```

The editor toolbar is using jQuery UI theming, so you'll probably also want to [grab a theme](http://jqueryui.com/themeroller/) that fits your needs.

Then include Hallo itself:

```html
<script src="hallo.js"></script>
```

Editor activation is easy:

```javascript
jQuery('p').hallo();
```

You can also deactivate the editor:

```javascript
jQuery('p').hallo({editable: false});
```

Hallo itself only makes the selected DOM elements editable and doesn't provide any formatting tools. Formatting is accomplished by loading plugins when initializing Hallo:

```javascript
jQuery('.editable').hallo({
  plugins: {
    'halloformat': {}
  }
});
```

This example would enable the simple formatting plugin that provides functionality like _bold_ and _italic_. You can include as many Hallo plugins as you want, and if necessary pass them options.

Hallo has got more options you set when instantiating. See the hallo.coffee file for further documentation.

## Plugins

* halloformat - Adds Bold, Italic, StrikeThrough and Underline support to the toolbar. (Enable/Disable with options: "formattings": {"bold": true, "italic": true, "strikeThough": true, "underline": false})
* halloheadings - Adds support for H1, H2, H3. You can pass a headings option key "headers" with an array of header sizes (i.e. headers: [1,2,5,6])
* hallojustify - Adds align left, center, right support
* hallolists - Adds support for ordered and unordered lists (Pick with options: "lists": {"ordered": false, "unordered": true})
* halloreundo - Adds support for undo and redo
* hallolink - Adds support to add links to a selection (currently not working)

## Licensing

Hallo is free software available under the [MIT license](http://en.wikipedia.org/wiki/MIT_License).

## Contributing

Hallo is written in [CoffeeScript](http://jashkenas.github.com/coffee-script/), a simple language that compiles into JavaScript. To generate the JavaScript code to `examples/hallo.js` from Hallo sources, run:

    $ cake build

If you want to also generate a minified version, run:

    $ cake min

Hallo development is coordinated using Git. Just fork the [Hallo repository on GitHub](https://github.com/bergie/hallo) and [send pull requests](http://help.github.com/pull-requests/).

### Writing plugins

Hallo plugins are written as regular [jQuery UI widgets](http://semantic-interaction.org/blog/2011/03/01/jquery-ui-widget-factory/).

When Hallo is loaded it will also load all the enabled plugins for the element, and pass them some additional options:

* `editable`: The main Hallo widget instance
* `toolbar`: Toolbar jQuery object for that Hallo instance
* `uuid`: unique identifier of the Hallo instance, can be used for element IDs

A simplistic plugin would look like the following:

```coffeescript
#    Formatting plugin for Hallo
#    (c) 2011 Henri Bergius, IKS Consortium
#    Hallo may be freely distributed under the MIT license
((jQuery) ->
  jQuery.widget "IKS.halloformat",
    boldElement: null

    options:
      uuid: ''
      editable: null

    _create: ->
      # Add any actions you want to run on plugin initialization
      # here

    populateToolbar: (toolbar) ->
      # Create an element for holding the button
      @boldElement = jQuery '<span></span>'

      # Use Hallo Button
      @boldElement.hallobutton
        uuid: @options.uuid
        editable: @options.editable
        label: 'Bold'
        # Icons come from Font Awesome
        icon: 'icon-bold'
        # Commands are used for execCommand and queryCommandState
        command: 'bold'

      # Append the button to toolbar
      toolbar.append @boldElement

    cleanupContentClone: (element) ->
      # Perform content clean-ups before HTML is sent out

)(jQuery)
```
