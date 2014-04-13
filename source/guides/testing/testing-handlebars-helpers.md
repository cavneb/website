By the end of this document, the reader should understand:

* *** Special test helpers for testing handlebars helpers should be in place ***
* when they would want to test handlebars helpers individually
* how to test handlebars [helpers](/guides/templates/writing-helpers/).


## Testing helpers (with template rendering)

Let's assume you have a helper that will wrap some text in a `span`
while adding a `class` attribute of `highlight`.

```javascript
Ember.Handlebars.helper("highlight", function(value, options) {
  var escaped = Handlebars.Utils.escapeExpression(value);
  return new Handlebars.SafeString('<span class="highlight">' + escaped + '</span>');
});
```

See the [guide on writing helpers](/guides/templates/writing-helpers)
for more info about the details of writing helpers.

So, if a template contained this:

```javascript
{{highlight "This text is highlighted"}}
```

The helper would produce this:

```html
<span class="highlight">This text is highlighted</span>
```


Here's a simple test for that helper:

```javascript
module("helpers", "", {});


test("highlight", function() {
  expect(2);

  var views;
  Ember.run(function () {
    view = Ember.View.create({
      template: Ember.Handlebars.compile("{{highlight 'just a test'}}")
    });
    view.appendTo("#qunit-fixture");
  });
  ok(view.$()[0].innerHTML.match(/<span class="highlight">/), "the highlight span should be present");
  equal(view.$().text(), "just a test", "the input text should be present");
});
```

Notice that when we test the rendered view of the template we are using
`view.$()[0]`.  Using only `view.$()` would return an array, which would
not have the `text` method.  `view.$()[0]` returns the first (and only)
element of the array.

#### Example

<a class="jsbin-embed" href="http://jsbin.com/bofep/1/embed?js,output">Unit Testing Helpers (Unbound (Rendering))</a><script src="http://static.jsbin.com/js/embed.js"></script>




## Testing helpers (with `_rawFunction`)

Sometimes rendering templates might be too heavy for testing helpers,
especially if you have dozens or hundreds of helpers to test.  An
alternative is to dive into the Ember internals to get a hold of the
helper function itself, and then testing it's output to given inputs.

*Note:* Please be aware that using internal Ember functions is generally
discouraged.  We are including this information because there is some
performance penalty incurred by rendering templates in order to test helpers. If
you're testing many helpers this could make your test suite slower than
it needs to be. This section is likely to change in the near future as
`ember-qunit` evolves.

Let's assume you have a helper that will wrap some text in a `span`
while adding a `class` attribute of `highlight`.

```javascript
Ember.Handlebars.helper("highlight", function(value, options) {
  var escaped = Handlebars.Utils.escapeExpression(value);
  return new Handlebars.SafeString('<span class="highlight">' + escaped + '</span>');
});
```

See the [guide on writing helpers](/guides/templates/writing-helpers)
for more info about the details of writing helpers.

So, if a template contained this:

```javascript
{{highlight "This text is highlighted"}}
```

The helper would produce this:

```html
<span class="highlight">This text is highlighted</span>
```

Here's a simple test for that helper:

```javascript
module("helpers", "", {});

// Creating a helper function ensures that we only have to
// update one thing if the Ember internals change.
function helperByName(helperName){
  Ember.Handlebars.helpers[helperName]._rawFunction;
}

test("highlight", function() {
  expect(2);

  var helper = helperByName("highlight");
  var result = helper("just a test").toString();
  var $result = $(result);
  ok($result.hasClass("highlight"), "the highlight span should be present");
  equal($result.text(), "just a test", "the input text should be present");
});
```

#### Example

<a class="jsbin-embed" href="http://jsbin.com/nubat/1/embed?js,output">Unit Testing Helpers (Unbound)</a><script src="http://static.jsbin.com/js/embed.js"></script>
