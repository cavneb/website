By the end of this document, the reader should understand:

* ??? input from @ryanflorence is requested

```javascript
moduleForComponent('x-foo', 'moduleForComponent with x-foo');

test('renders', function() {
  expect(2);
  var component = this.subject();
  equal(component.state, 'preRender');
  this.append();
  equal(component.state, 'inDOM');
});

test('yields', function() {
  expect(2);
  var component = this.subject({
    template: "yield me".compile()
  });
  equal(component.state, 'preRender');
  this.append();
  equal(component.state, 'inDOM');
});

test('can lookup components in its template', function() {
  expect(1);
  var component = this.subject({
    template: "{{x-foo id='yodawg-i-heard-you-liked-x-foo-in-ur-x-foo'}}".compile()
  });
  this.append();
  equal(component.state, 'inDOM');
});

test('clears out views from test to test', function() {
  expect(1);
  var component = this.subject({
    template: "{{x-foo id='yodawg-i-heard-you-liked-x-foo-in-ur-x-foo'}}".compile()
  });
  this.append();
  ok(true, 'rendered without id already being used from another test');
});
```