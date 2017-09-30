+++
    title = "Computed Properties"
    date = 2017-09-29T22:45:05+05:30
    draft = false
    Description = "computed properties"
    Tags = ["computed properties"]
    Categories = ["computed properties"]
+++

In a nutshell, computed properties let you declare functions as properties. You create one by defining a computed property as a function, which Ember will automatically call when you ask for the property. You can then use it the same way you would any normal, static property.

To define a computed property (CP), we need to call <code>Ember.computed</code>. The first parameters passed to it are the dependent keys of the CP. The last parameter is the function that computed the value of the CP. The value of the CP changes if and only if the value of any of the dependent keys changes, and is cached between such changes.

When the value of a dependent key changes, the CP is marked as stale. The next time we access its value, the computing function is run and its returned value becomes the new value of the CP.

### Computed properties in action

```
import Ember from 'ember':

const Pizza = Ember.Object.extend({
  type: null,
  price: null,
  name: null,

  detials: Ember.computed('type', 'price', 'name', function() {
      return `Name: ${Ember.get(this, 'name')}, Type: ${Ember.get(this,'type')}, Price:${Ember.get(this, 'price')}`
  })
})

const VegPizza = Pizza.create({
  type: 'Veg',
  price: 500,
  name: 'Veggie Paradise'
});

Ember.get(VegPizza, 'detials') // Name: Veggie Paradise, Type: Veg, Price:500
```
In the above example, detials is dependent on type, price and name, so it watches those properties, and when either of their values change, its function is invoked, thereby returning the updated detials property for our beloved Veg Pizza.

#### Short-hand syntax
We can also use a short-hand syntax called brace expansion to declare the dependents. You surround the dependent properties with braces <code>({})</code>, and separate with commas, like so:
```
import Ember from 'ember':

...
detials: Ember.computed({type,price,name}, function() {
    return `Name: ${Ember.get(this, 'name')}, Type: ${Ember.get(this,'type')}, Price:${Ember.get(this, 'price')}`
})
...
```
<code>[Twiddle Link](https://ember-twiddle.com/f457c11f89066ff939fc2683763bd910?openFiles=controllers.application.js%2C)</code>

<br>
This is especially useful when you depend on properties of an object, since it allows you to replace:

```
import Ember from 'ember':

let obj = Ember.Object.extend({
  baz: {foo: 'BLAMMO', bar: 'BLAZORZ'},

  something: Ember.computed('baz.foo', 'baz.bar', function() {
    return `${this.get('baz.foo')} ${this.get('baz.bar')}`;
  })
});
```

With:

```
import Ember from 'ember':

let obj = Ember.Object.extend({
  baz: {foo: 'BLAMMO', bar: 'BLAZORZ'},

  something: Ember.computed('baz.{foo,bar}', function() {
    return `${this.get('baz.foo')} ${this.get('baz.bar')}`;
  })
});
```

<!--### Computed property macros-->