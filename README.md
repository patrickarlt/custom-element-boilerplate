# Custom Elements

Simple boilerplate for custom elements, using ES6 but without the overhead of polymer.

For details, see the [custom elements W3C spec](http://w3c.github.io/webcomponents/spec/custom/), or some [other good posts](http://h3manth.com/new/blog/2015/custom-elements-with-es6/)

```html
<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" href="calcite-web.css">
    <script src="item-rating.js"></script>
  </head>
  <body>
    <h1>My Item</h1>

    <item-rating id="item-rating" itemid="30e5fe3149c34df1ba922e6f5bbf808f" numratings="5" rating="4.25">

    <script>
      var rating = document.getElementById('item-rating');
      rating.addEventListener('rateitem', function(e) {
        console.log('Rate Item:', e.detail.rating);
      });
    </script>

  </body>
</html>
```

## Get going

- `npm install`
- `npm start`
- [Declarative API](examples/declarative.html)
- [Programatic API](examples/programatic.html)
- [With Backbone](examples/backbone-app/index.html)
- [With Angular 1.0](examples/angular-1-app/index.html)
- [With Angular 2.0](examples/angular-2-app/app.ts)
- [With Ember 2.0](examples/ember-app/app/components/item-rating.js)
- [With Aurelia](examples/aurelia-app/src)
- [JS API](examples/js-api.html)

You may also want to look at the [source code](src/item-rating.js) for the component or the [PDF presentation](https://github.com/patrickarlt/custom-element-boilerplate/raw/master/Cross-Framework%20UI%20Components.pdf).

## Notes

### Strictly Adhere to DOM Standards

As a rule for maximum compatibility with the largest number of tools you should keep the following standards in mind.

1. Always use DOM style event names, all lowercase, with no special characters. This helps keep any kind of event mapping magic (Ember) consistent.
2. Each attribute should always have a corresponding getter and getter that matches the attribute name.
3. Attributes should always adhere to DOM standards, all lowercase with no special characters. This combined with the above point is essential for Aurelia.
4. You should always bubble your custom events. This is necessary for Ember (since it uses event delegation to listen to events) and for components to nest inside each other and listen to events

### Required Polyfills

* `CustomEvent` for IE 9 and 10 - https://github.com/webmodules/custom-event and https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent/CustomEvent
* `document.RegisterElement` - https://github.com/WebReflection/document-register-element and http://caniuse.com/#feat=custom-elements
* Patch `HTMLElement` so it can be extended by Babel in [Safari](src/polyfills/html-element.js)
* The [Babel polyfill](http://babeljs.io/docs/usage/polyfill/)

All of these polyfills come out to about 25kb minified and gzipped. Most of that is the Babel polyfill which is 22kb minified and gzipped. You can save 5kb by using [the shim from Core JS directly](https://github.com/zloirock/core-js#basic) directly and could likely save even more with a [custom build](https://github.com/zloirock/core-js#custom-build).

You can also reduce the needs for polyfills by restricting features (for example using only `class`, arrow functions and multiline srings) and [using loose mode in Babel](https://babeljs.io/docs/advanced/loose/).

### Browser support

With the [required polyfills](#required-pollyfills) in place this should work in all modern browsers back to IE 9. However this needs testing.

### React

Currently React 0.13 uses a [whitelist of valid HTML attributes](https://github.com/facebook/react/issues/140) so you cannot use non-standard attributes in React. This will be fixed for custom elements with React 0.14. With this change custom elements should work totally out of the box.

### Ember

Currently in addition to [wrapping the custom element as a component](examples/ember-app/app/components/item-rating.js) you must [register any custom events with Ember](examples/ember-app/config/environment.js#L16-L23). Eventually the component restriction will go away with the move to angle bracket components in Ember 2.2, see the [RFC](https://github.com/emberjs/rfcs/pull/60) and the [PR](https://github.com/emberjs/ember.js/pull/12011).

The restriction to add custom event to the global registry will still remain. However the author of a proposed event [refactoring RFC](https://github.com/emberjs/rfcs/pull/86) is interested in removing the requirement.

### Angular 2.0

Works totally out of the box.

### Angular 1.0

Attribute binding works fine out of the box but in order to bind custom events you must [write a small custom directive](/examples/angular-1-app/index.html#L28-L44).

### Backbone

Works totally out of the box.

### Aurelia

Works totally out of the box, as long as you obey the [DOM standards](#strictly-adhere-to-dom-standards), particularly with regards to properties and attributes. If you want to break standards for more readable attribute names or some other reason you can extend Aurelia's parser to support your use case.

## License

MIT
