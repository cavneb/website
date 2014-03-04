Unit tests are generally used to test a small piece of code and ensure that it is doing what was intended. Unline integration tests, they are narrow in scope and do not require the Ember application to be running.

***To perform unit tests, the application must be written using modules. ______EXPLAIN______***

***Explain that ember qunit is used to perform unit tests, but it can be converted to other testing frameworks***

### Unit Helpers

* `moduleFor(fullName, description, callbacks, delegate)`
 - Lorem ipsum dolor.
* `moduleForComponent(fullName, description, callbacks)`
 - Lorem ipsum dolor.
* `moduleForModel(fullName, description, callbacks)`
 - Lorem ipsum dolor.

### Registry

***Explain this:***

```javascript
var registry = {
  'component:x-foo': Ember.Component.extend(),
  'route:foo':       Ember.Route.extend(),
  'controller:bar':  Ember.Controller.extend()
};
```