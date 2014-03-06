Unit tests are generally used to test a small piece of code and ensure that it is doing what was
intended. Unlike integration tests, they are narrow in scope and do not require the Ember
application to be running.

### Globals vs Modules

In the past, it has been difficult to test portions of your Ember application without loading the
entire application. By having your application written using modules (CJS, AMD, etc), you are able
to require the code that is to be tested without having to pluck the pieces off of your global
application.

### Unit Testing Helpers

[Ember-QUnit](https://github.com/emberjs/ember-qunit) is the default *unit* testing helper suite for
Ember. It can and should be used as a template for other test framework helpers.

* [Ember-QUnit](https://github.com/emberjs/ember-qunit) - Unit test helpers written for QUnit
* [Ember-Mocha](#) - Unit test helpers written for Mocha (to be written)
* [Ember-Jasmine](#) - Unit test helpers written for Jasmine (to be written)

***The unit testing section of this guide will use the Ember-QUnit library, but the concepts and
examples should translate easily to other frameworks.***

### Available Helpers

By including [Ember-QUnit](), you will have access to a number of test helpers.

* `moduleFor(fullName, description, callbacks, delegate)`
 - description goes here ....
* `moduleForComponent(fullName, description, callbacks)`
 - description goes here ....
* `test`
 - description goes here ....
* `setResolver`
 - description goes here ....

### Unit Testing Global Applications

You are still able to use the unit test helpers on your global applications. To do so, you
must globalize the qunit helpers in your test setup:

```javascript
emq.globalize();
```

This will make the above helpers available globally.
