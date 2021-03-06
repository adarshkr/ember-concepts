+++
    title = "Ember Object Model"
    date = 2017-09-26T22:36:21+05:30
    draft = false
    Description = "Ember Object Model"
    Tags = ["object model"]
    Categories = ["object model"]
+++

Ember implements its own object system. The base object is Ember.Object. All of the other objects in Ember extend `Ember.Object`.

`Ember.Object` also provides a class system, supporting features like mixins and constructor methods. Some features in Ember's object model are not present in JavaScript classes or common patterns, but all are aligned as much as possible with the language and proposed additions.

Ember also extends the JavaScript `Array` prototype with its Ember.Enumerable interface to provide change observation for arrays.


### Creating Objects
You can instantiate a basic object like this:

```javascript
const Pizza = Ember.Object.create({
  category: "Food"
});
```
You can get a property from the object by calling .get on it and passing the string name of the property:

```javascript
const Pizza = Ember.Object.create({ category: 'Food' })
Pizza.get('category') is 'Food'
```

#### Defining Objects and Instances

To define a new Ember class, call the `extend()` method on Ember.Object:

Feeling hungry ? Why dont we have some pizza ?

```javascript
const Pizza = Ember.Object.extend({
  category: "Food"
});

const NonVeg = Pizza.extend({
  type: "Non Veg",
})

const Veg = Pizza.extend({
  type: "Veg"
})
```

### Initialization (and a common mistake!)

One of the most common mistakes for beginners to Ember is to think they're passing properties to an instance instead of a prototype. For example:

```javascript
const Pizza = Ember.Object.extend({
  toppings: ["Babycorn"] // CAREFUL !!!!!
});

const Veg = Pizza.create();
Veg.get("toppings").push("Jalapeno");

const NonVeg = Pizza.create();
NonVeg.get("toppings").push("Chicken Tikka");

// Veg and NonVeg are all mixed up?!?!?!?!
console.log( Veg.get("toppings") );  // ["Babycorn", "Jalapeno", "Chicken Tikka"]
console.log( NonVeg.get("toppings") ); // ["Babycorn", "Jalapeno", "Chicken Tikka"]
```

Why did this toppings mutation happen? The problem started when we added an array to our prototype when defining the Pizza class. This array was then shared with each object instantiated from Pizza. Pizza lovers may not like `this`.

### Now you might be thinking how to keep pizza lovers happy without messing up with toppings ? 


```javascript
const Pizza = Ember.Object.extend({
  toppings: null,
  init: function() {
    this._super();
    this.set("toppings", ["Black Olive"]); // everyone gets at least one Black Olive toppings
  }
});

const VeggieParadise = Pizza.create();
VeggieParadise.get("toppings").push("Jalapeno"); // Viggies also get a Jalapeno toppings

const NonVegSupreme = Pizza.create();
NonVegSupreme.get("toppings").push("Chicken Tikka"); // Non veg lovers also get a Chicken Tikka toppings

// Hurray - everyone gets their own toppings!
console.log( VeggieParadise.get("toppings") );  // ["Black Olive", "Jalapeno"]
console.log( NonVegSupreme.get("toppings") ); // ["Black Olive", "Chicken Tikka"]
```

<code>[Ember Twiddle Link](https://ember-twiddle.com/c4016dce4607dfc1758456ff17983571)</code>

<br>
When declaring objects or arrays in your classes, you'll typically want to initialize them along with each instance in the `init()` function. In this way, each of your objects will receive its own unique instances of objects and arrays. Also remember to call `this._super()` from within `init()` so that `init()` will be called all the way up the prototype chain.

Of course, there's nothing wrong with keeping objects or arrays directly in your prototypes if they are meant to remain constant across instances. In fact, one common pattern is to keep a default setting in the prototype that's then duplicated for each instance in `init()`. These kinds of patterns are easy to implement once you realize how objects are created and initialized.


### Reopening Objects

You don't need to define a class all at once. You can reopen a class and define new properties using the `reopen()` method.

```javascript
const Pizza = Ember.Object.extend({
  category: "Food"
});

const NonVeg = Pizza.extend({
  type: "Non Veg",
})

NonVeg.reopen({
  size: "Large"
})

const NonVegSupreme = new NonVeg()

console.log(NonVegSupreme.get('size')) // Large
```

If you want to add properties directly to a class, use reopenClass():

```javascript
const Pizza = Ember.Object.extend({
  size: "Large"
});

Pizza.reopenClass({
  updateSize: function(attributes) {
    return Pizza.create(attributes);
  }
});

const size = Pizza.updateSize({size: "Medium"});
console.log(size) // {size:"Medium"}
```
