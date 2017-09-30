+++
    title = "Useful Functions"
    date = 2017-09-30T21:42:22+05:30
    draft = false
    Description = "useful emberjs functions"
    Tags = ["ember"]
    Categories = ["ember"]
+++

The following functions are defined in packages/ember-runtime/lib/ and packages/ember-metal/lib/ respectively.

### Ember

#### isNone

Returns `true` if the passed value is null or undefined.  This avoids errors from JSLint complaining about use of ==, which can be technically confusing.

```
Ember.isNone(); // true
Ember.isNone(null); // true
Ember.isNone(undefined); // true
Ember.isNone(''); // false
Ember.isNone([]); // false
Ember.isNone(function(){}); // false
```

#### isEmpty

 Verifies that a value is `null` or an `empty` `string | array | function`

```
Ember.isEmpty(); // true
Ember.isEmpty(null); // true
Ember.isEmpty(undefined); // true
Ember.isEmpty(''); // true
Ember.isEmpty([]); // true
Ember.isEmpty('pizza'); // false
Ember.isEmpty([0,1,2]); // false
```

#### isArray

Use this to check if a value is an array. This method also returns `true` for array-like objects, like the ones implementing `Ember.Array`.

```
Ember.isArray(); // false
Ember.isArray([]); // true
Ember.isArray( Ember.ArrayProxy.create({ content: [] }) ); // true
```


####  makeArray

Forces the passed object to be part of an array. If the object is already an array, it will return the object. Otherwise, it will add the object to an array. If obj is `null` or `undefined`, it will return an empty array.

 ```
 Ember.makeArray();            // []
 Ember.makeArray(null);        // []
 Ember.makeArray(undefined);   // []
 Ember.makeArray('lindsay');   // ['lindsay']
 Ember.makeArray([1, 2, 42]);  // [1, 2, 42]
 let controller = Ember.ArrayProxy.create({ content: [] });
 Ember.makeArray(controller) === controller;  // true
 ```
