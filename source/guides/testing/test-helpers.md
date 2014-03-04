By the end of this document, the reader should understand:

* What are custom test helpers
* Sync vs Async test helpers
* How to register their custom test helpers

---

Ember includes several helpers to facilitate integration testing. These helpers are "aware" of (and wait for) asynchronous behavior within your application, making it much easier to write deterministic tests.

### Interaction Helpers

* `visit(url)`
 - Visits the given route and returns a promise that fulfills when all resulting async behavior is complete.
* `find(selector, context)`
 - Finds an element within the app's root element and within the context (optional). Scoping to the root element is especially useful to avoid conflicts with the test framework's reporter.
* `fillIn(input_selector, text)`
 - Fills in the selected input with the given text and returns a promise that fulfills when all resulting async behavior is complete.
* `click(selector)`
  - Clicks an element and triggers any actions triggered by the element's `click` event and returns a promise that fulfills when all resulting async behavior is complete.
* `keyEvent(selector, type, keyCode)`
  - Simulates a key event type, e.g. `keypress`, `keydown`, `keyup` with the desired keyCode on element found by the selector.

### Location Helpers

* `currentPath`
 - Returns the current path.
* `currentRouteName`
 - Returns the currently active route name.
* `currentURL`
 - Returns the current URL.

### Custom Test Helpers

`Ember.Test.registerHelper` and `Ember.Test.registerAsyncHelper` are used to register test helpers that will be injected when `App.injectTestHelpers` is called. The difference between `Ember.Test.registerHelper` and `Ember.Test.registerAsyncHelper` is that the latter will not run until any previous async helper has completed and any subsequent async helper will wait for it to finish before running.

The helper method will always be called with the current Application as the first parameter. Helpers need to be registered prior to calling `App.injectTestHelpers()`.

Here is an example of a non-async helper:

```javascript
Ember.Test.registerHelper('shouldHaveElementWithCount', 
  function(app, selector, n, context) {
    var el = findWithAssert(selector, context);
    var count = el.length;
    equal(n, count, 'found ' + count + ' times');
  }
);

// shouldHaveElementWithCount("ul li", 3);
```

Here is an example of an async helper:

```javascript
Ember.Test.registerAsyncHelper('dblclick', 
  function(app, selector, context) {
    var $el = findWithAssert(selector, context);
    Ember.run(function() {
      $el.dblclick();
    });
  }
);

// dblclick("#person-1")
```

Async helpers also come in handy when you want to group interaction
into one helper. For example:

```javascript
Ember.Test.registerAsyncHelper('addContact',
  function(app, name, context) {
    fillIn('#name', name);
    click('button.create');
  }
);

// addContact("Bob");
// addContact("Dan");
```