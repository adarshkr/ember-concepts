+++
    title = "Ember Object Model"
    date = 2017-09-26T22:36:21+05:30
    draft = false
    Description = "Ember Object Model"
    Tags = ["object model"]
    Categories = ["object model"]
+++

JavaScript objects don't support the observation of property value changes. Consequently, if an object is going to participate in Ember's binding system you may see an <code>Ember.Object</code> instead of a plain object.

<code>Ember.Object</code> also provides a class system, supporting features like mixins and constructor methods. Some features in Ember's object model are not present in JavaScript classes or common patterns, but all are aligned as much as possible with the language and proposed additions.

Ember also extends the JavaScript <code>Array</code> prototype with its Ember.Enumerable interface to provide change observation for arrays.

### Classes and Instances

#### Defining Classes

To define a new Ember class, call the extend() method on Ember.Object:

```
const Person = Ember.Object.extend({
  say(thing) {
    alert(thing);
  }
});
```
This defines a new Person class with a say() method.