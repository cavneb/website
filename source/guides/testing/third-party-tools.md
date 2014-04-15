Ember gives us a lot of built in tools to help us do testing. What if we want to exercise
our code further, beyond what Ember provides out of the box? This section is an overview
of some third-party tools that we can use in conjunction with the built-in tools to allow
us to test our Ember apps even further.

### Sinon

[Sinon.js] is a swiss army knife testing library that works rather well
with Ember. It provides us a good set of tools that make
[unit-testing](/guides/testing/unit-testing-basics) more complex scenarios easy. We can
touch on a few here.

***Do not use sinon-quint in conjunction with Integration tests. It sandboxes your methods
and will cause Integration Tests to break.***

### Spies

[Spies](http://sinonjs.org/docs/#spies) are a way to test methods that are invoked by other methods.
An example of this may be an `action` method on an Ember controller that wraps other methods to be
executed when called.

```javascript
App.SomeThingController = Ember.Controller.extend({

  delegatedMethod: function () {
    return 'executed by action';
  },

  actions: {

    delegationActionMethod: function () {
      this.delegatedMethod();
    }

  }

});
```
In this example, we may want to test that when our `delegationActionMethod` is called,
that it in turn calls the `delegatedMethod`. Lets look at how we can accomplish this
by using [Sinon.js] spies.

```javascript
module('Unit: SomeThingController');

test('SomeThingController delegationActionMethod calls SomeThingController delegatedMethod', sinon.test(function() {
  expect(1);
  var someThing = App.SomeThingController.create();

  someThing.delegatedMethod = this.spy(someThing, 'delegatedMethod');

  someThing.send('delegationActionMethod');

  ok(someThing.delegatedMethod.called, 'SomeThing.delegationActionMethod calls SomeThing.delegatedMethod');
}));
```

Here we can see that when our `action` is called we can test that the delegated method is in turn called. A few
things to take notice of in this example. First we wrap our normal anonymous callback in the `sinon.test()` method.

```javascript
sinon.test(function() { ...
```

This allows sinon to create a sandbox with the internals of our test and reset any stubbed/spyed methods back to
their original state. Next we create our spy by setting the turning our delegated method into a spy, so when called,
doesn't execute its normal behavior. We pass 2 args. The first being the scope and the second the method we want to
spy on.

```javascript
someThing.delegatedMethod = this.spy(someThing, 'delegatedMethod');
```

We can then call our `action` and it in turn calls the spy. Then we can assert that our delegated method was called.
[Sinon.js] offers a full suite of assertions like `called`, `calledWith`, `calledBefore`, `calledAfter` and many more.
See the [documentation](http://sinonjs.org/docs/#spies) for all the available assertions.

```javascript
  someThing.send('delegationActionMethod');

  ok(someThing.delegatedMethod.called, 'SomeThing.delegationActionMethod calls SomeThing.delegatedMethod');
```
#### Live Example

<a class="jsbin-embed" href="http://emberjs.jsbin.com/rehiwazo/40/embed?output">Testing Ember Methods with Sinon Spies</a>

### Stubs

[Stubs](http://sinonjs.org/docs/#stubs) are `spies` but allow us to use pre-programmed behavior allowing us to control
the code flows.

```javascript
App.SomeThingController = Ember.Controller.extend({

  invokedMethod: function (passedValue) {
    if (this.delegatedMethod(passedValue) === 'someReturnValue') {
      return 'Bob has a cool stash.';
    } else if (this.delegatedMethod(passedValue) === 'someOtherReturnValue') {
      return 'Eric is the Testing Santa!';
    } else {
      return 'You are not a super cool Testing Santa!';
    }
  },

  delegatedMethod: function (passedValue) {
      if (passedValue === 'Bob') {
        return 'someReturnValue';
      } else if (passedValue === 'Eric') {
        return 'someOtherReturnValue';
      } else {
        return 'not so cool';
      }
  }

});
```

Here we have a method `invokedMethod` that returns different values based upon the return value of another method.
Our goal is to test this method based on the return values but without executing the internal methods.

```javascript
module('Unit: SomeThingController');

test('SomeThing delegationActionMethod with controlled return values', sinon.test(function() {
  expect(3);
  var someThing = App.SomeThingController.create();

  someThing.delegatedMethod = this.stub(someThing, 'delegatedMethod');
  someThing.delegatedMethod.withArgs("Bob").returns("someReturnValue");
  someThing.delegatedMethod.withArgs("Eric").returns("someOtherReturnValue");

  equal(someThing.invokedMethod('Bob'), "Bob has a cool stash.", 'SomeThing.delegatedMethod is called and returns correct value');
  equal(someThing.invokedMethod('Eric'), "Eric is the Testing Santa!", 'SomeThing.delegatedMethod is called and returns correct value');
  equal(someThing.invokedMethod('Someone Else'), 'You are not a super cool Testing Santa!', 'SomeThing.delegatedMethod throws with incorrect values');
}));
```

In the example above you can see that by calling `invokedMethod` with our `delegatedMethod` stubbed out with
specific return values. We can test the code fork results without the need to actually invoke the internal methods.
Another example of the powerful use of stubs would be if you needed to stub `ajax` calls without wanting to invoke them.

#### Live Example

<a class="jsbin-embed" href="http://emberjs.jsbin.com/pixiw/36/embed?output">Testing Ember Methods with Sinon Stubs</a>

We have only scratched the surface of what [Sinon.js] can do for us when testing our Ember apps. It offers us a larger
tool set for a very robust testing suite.

[Sinon.js]: http://sinonjs.org/
