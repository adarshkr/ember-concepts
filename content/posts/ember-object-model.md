+++
    title = "Ember Object Model"
    date = 2017-09-26T22:36:21+05:30
    draft = false
    Description = "Ember Object Model"
    Tags = ["object model"]
    Categories = ["object model"]
+++

Ember implements its own object system. The base object is Ember.Object. All of the other objects in Ember extend <code>Ember.Object</code>.

<code>Ember.Object</code> also provides a class system, supporting features like mixins and constructor methods. Some features in Ember's object model are not present in JavaScript classes or common patterns, but all are aligned as much as possible with the language and proposed additions.

Ember also extends the JavaScript <code>Array</code> prototype with its Ember.Enumerable interface to provide change observation for arrays.


### Creating Objects
You can instantiate a basic object like this:

```
const Pizza = Ember.Object.create({
  category: "Food"
});

```
You can get a property from the object by calling .get on it and passing the string name of the property:

```
const Pizza = Ember.Object.create({ category: 'Food' })
Pizza.get('category') is 'Food'

```

#### Defining Objects

To define a new Ember class, call the <code>extend()</code> method on Ember.Object:

Feeling hungry ? Why dont we have some pizza ?

```
const Pizza = Ember.Object.extend({
  category: "Food"
});

const NonVegSupreme = Pizza.extend({
  type: "Non Veg",
})

const VeggieParadise = Pizza.extend({
  type: "Veg"
})
```

### Initialization (and a common mistake!)

One of the most common mistakes for beginners to Ember is to think they're passing properties to an instance instead of a prototype. For example:

```
const Pizza = Ember.Object.extend({
  toppings: ["Babycorn"] // CAREFUL !!!!!
});

const VeggieParadise = Pizza.create();
VeggieParadise.get("toppings").push("Jalapeno");

const NonVegSupreme = Pizza.create();
NonVegSupreme.get("toppings").push("Chicken Tikka");

// VeggieParadise and NonVegSupreme are all mixed up?!?!?!?!
console.log( VeggieParadise.get("toppings") );  // ["Babycorn", "Jalapeno", "Chicken Tikka"]
console.log( NonVegSupreme.get("toppings") ); // ["Babycorn", "Jalapeno", "Chicken Tikka"]

```

Why did this toppings mutation happen? The problem started when we added an array to our prototype when defining the Pizza class. This array was then shared with each object instantiated from Pizza. Pizza lovers may not like <code>this</code>.

Now you might be thinking how to keep pizza lovers happy without messing up with toppings ? 


```
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

When declaring objects or arrays in your classes, you'll typically want to initialize them along with each instance in the <code>init()</code> function. In this way, each of your objects will receive its own unique instances of objects and arrays. Also remember to call <code>this._super()</code> from within <code>init()</code> so that <code>init() </code> will be called all the way up the prototype chain.

Of course, there's nothing wrong with keeping objects or arrays directly in your prototypes if they are meant to remain constant across instances. In fact, one common pattern is to keep a default setting in the prototype that's then duplicated for each instance in <code>init()</code>. These kinds of patterns are easy to implement once you realize how objects are created and initialized.
