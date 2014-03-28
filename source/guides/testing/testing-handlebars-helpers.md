By the end of this document, the reader should understand:

* *** Special test helpers for testing handlebars helpers should be in place ***
* when they would want to test handlebars helpers individually
* how to test handlebars [[helpers]]

## Testing unbound helpers

Let's assume you have a helper that will wrap some text in a `span`
while adding a `class` attribute of `highlight`.

```javascript
Ember.Handlebars.helper('highlight', function(value, options) {
  var escaped = Handlebars.Utils.escapeExpression(value);
  return new Handlebars.SafeString('<span class="highlight">' + escaped + '</span>');
});
```

Here's a simple test for that helper.

```javascript
emq.globalize();
setResolver(App.__container__);
App.setupForTesting();
App.injectTestHelpers();

module('helpers', '', {});


test('highlight', function() {
  var helper = Ember.Handlebars.helpers.highlight._rawFunction;
  var result = helper('just a test').toString();
  var $result = $(result);
  ok( $result.hasClass('highlight'), 'the highlight span should be present' );
  equal( $result.text(), 'just a test', 'the input text should be present' );
});
```

#### Example

<a class="jsbin-embed" href="http://jsbin.com/nubat/1/embed?js,output">Unit Testing Helpers (Unbound)</a><script src="http://static.jsbin.com/js/embed.js"></script>
