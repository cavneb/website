Components can be testing using the `moduleForComponent` helper. Here is a simple Ember component:

```javascript
App.PrettyColorComponent = Ember.Component.extend({
  classNames: ['pretty-color'],
  attributeBindings: ['style'],
  style: function() {
    return 'color: ' + this.get('name') + ';';
  }.property('name')
});
```

with an accompanying Handlebars template:

```handlebars
Pretty Color: {{name}}
```

Unit testing this component can be done as follows:

```javascript
moduleForComponent('pretty-color', 'moduleForComponent with pretty-color');

test('renders the template', function() {
  expect(3);

  // the context for the component
  var context = { name: 'green' };

  // the handlebars template used for the view rendering the component
  var template = "{{pretty-color name=name}}".compile();

  // build the component using the subject helper along with the content
  var component = this.subject({ template: template }, context);

  equal(component.state, 'preRender');

  // render the component to the view
  this.append();

  equal(component.state, 'inDOM');

  // assert that the content rendered to the DOM is correct
  equal(component.$().html(), 'Pretty Color: green');
});
```

or more simply:

```javascript
moduleForComponent('pretty-color', 'moduleForComponent with pretty-color');

test('renders the template', function() {
  expect(3);
  var component = this.subject({
    template: "{{pretty-color name=name}}".compile()
  }, { name: 'green' });
  equal(component.state, 'preRender');
  this.append();
  equal(component.state, 'inDOM');
  equal(component.$().html(), 'Pretty Color: green');
});
```