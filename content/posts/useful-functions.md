+++
    title = "Useful Functions"
    date = 2017-09-30T21:42:22+05:30
    draft = false
    Description = "useful emberjs functions"
    Tags = ["ember"]
    Categories = ["ember"]
+++

The following functions are defined in packages/ember-runtime/lib/ and packages/ember-metal/lib/ respectively.

### Ember functions

#### isNone
Returns `true` if the passed value is null or undefined.  This avoids errors from JSLint complaining about use of ==, which can be technically confusing.

```javascript
Ember.isNone(); // true
Ember.isNone(null); // true
Ember.isNone(undefined); // true
Ember.isNone(''); // false
Ember.isNone([]); // false
Ember.isNone(function(){}); // false
```

#### isBlank 
Returns `true` if the passed value is empty or a whitespace string.

```javascript
  Ember.isBlank();                // true
  Ember.isBlank(null);            // true
  Ember.isBlank(undefined);       // true
  Ember.isBlank('');              // true
  Ember.isBlank([]);              // true
  Ember.isBlank('\n\t');          // true
  Ember.isBlank('  ');            // true
  Ember.isBlank({});              // false
  Ember.isBlank('\n\t Hello');    // false
  Ember.isBlank('Hello world');   // false
  Ember.isBlank([1,2,3]);         // false
  ```


#### isEmpty
 Verifies that a value is `null` or an `empty` `string | array | function`

```javascript
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

```javascript
Ember.isArray(); // false
Ember.isArray([]); // true
Ember.isArray( Ember.ArrayProxy.create({ content: [] }) ); // true
```


####  makeArray
Forces the passed object to be part of an array. If the object is already an array, it will return the object. Otherwise, it will add the object to an array. If obj is `null` or `undefined`, it will return an empty array.

 ```javascript
 Ember.makeArray();            // []
 Ember.makeArray(null);        // []
 Ember.makeArray(undefined);   // []
 Ember.makeArray('lindsay');   // ['lindsay']
 Ember.makeArray([1, 2, 42]);  // [1, 2, 42]
 let controller = Ember.ArrayProxy.create({ content: [] });
 Ember.makeArray(controller) === controller;  // true
 ```

#### typeOf
Returns a consistent type for the passed item.

```javascript
  Ember.typeOf();                       // 'undefined'
  Ember.typeOf(null);                   // 'null'
  Ember.typeOf(undefined);              // 'undefined'
  Ember.typeOf('michael');              // 'string'
  Ember.typeOf(new String('michael'));  // 'string'
  Ember.typeOf(101);                    // 'number'
  Ember.typeOf(new Number(101));        // 'number'
  Ember.typeOf(true);                   // 'boolean'
  Ember.typeOf(new Boolean(true));      // 'boolean'
  Ember.typeOf(Ember.A);                // 'function'
  Ember.typeOf([1, 2, 90]);             // 'array'
  Ember.typeOf(/abc/);                  // 'regexp'
  Ember.typeOf(new Date());             // 'date'
  Ember.typeOf(event.target.files);     // 'filelist'
  Ember.typeOf(Ember.Object.extend());  // 'class'
  Ember.typeOf(Ember.Object.create());  // 'instance'
  Ember.typeOf(new Error('teamocil'));  // 'error'
  // 'normal' JavaScript object
  Ember.typeOf({ a: 'b' });             // 'object'
  ```

#### compare
Compares two javascript values and returns:

- -1 if the first is smaller than the second,
- 0 if both are equal,
- 1 if the first is greater than the second.

```javascript
Ember.compare('hello', 'hello');  // 0
Ember.compare('abc', 'dfg');      // -1
Ember.compare(2, 1);              // 1
```

#### isEqual 
Compares two objects, returning true if they are equal.

 ```javascript
  Ember.isEqual('hello', 'hello');                   // true
  Ember.isEqual(1, 2);                               // false
```

#### copy
Cloning an object can be done via Ember.copy. If the object has the Ember.Copyable mixin its copy function is used. You can create a deep copy of the object by passing true as second argument.

Ember.copy('hello'); // 'hello'

#### isPresent
Returns `true` if the passed value is not `isBlank`.

```javascript
  Ember.isPresent();                // false
  Ember.isPresent(null);            // false
  Ember.isPresent(undefined);       // false
  Ember.isPresent('');              // false
  Ember.isPresent('  ');            // false
  Ember.isPresent('\n\t');          // false
  Ember.isPresent([]);              // false
  Ember.isPresent({ length: 0 })    // false
  Ember.isPresent(false);           // true
  Ember.isPresent(true);            // true
  Ember.isPresent('string');        // true
  Ember.isPresent(0);               // true
  Ember.isPresent(function() {})    // true
  Ember.isPresent({});              // true
  Ember.isPresent(false);           // true
  Ember.isPresent('\n\t Hello');    // true
  Ember.isPresent([1,2,3]);         // true
  ```

## String functions

#### w()
Splits a string into separate units separated by spaces, eliminating any empty strings in the process. This is a convenience method for split that is mostly useful when applied to the `String.prototype`

```javascript
'a b c'.w(); // ['a', 'b', 'c']
''.w(); // ['']
'thanks  brother!'.w(); // ['thanks', 'brother!']
```

#### camelize 
Returns the lowerCamelCase form of a string.

```javascript
'innerHTML'.camelize();          // 'innerHTML'
'action_name'.camelize();        // 'actionName'
'css-class-name'.camelize();     // 'cssClassName'
'my favorite items'.camelize();  // 'myFavoriteItems'
'My Favorite Items'.camelize();  // 'myFavoriteItems'
'private-docs/owner-invoice'.camelize(); // 'privateDocs/ownerInvoice'
```


#### capitalize
Returns the Capitalized form of a string

```javascript
'innerHTML'.capitalize()         // 'InnerHTML'
'action_name'.capitalize()       // 'Action_name'
'css-class-name'.capitalize()    // 'Css-class-name'
'my favorite items'.capitalize() // 'My favorite items'
'privateDocs/ownerInvoice'.capitalize(); // 'PrivateDocs/ownerInvoice'
```

#### classify
Returns the UpperCamelCase form of a string.

```javascript
'innerHTML'.classify();          // 'InnerHTML'
'action_name'.classify();        // 'ActionName'
'css-class-name'.classify();     // 'CssClassName'
'my favorite items'.classify();  // 'MyFavoriteItems'
'private-docs/owner-invoice'.classify(); // 'PrivateDocs/OwnerInvoice'
```

#### dasherize
Replaces underscores, spaces, or camelCase with dashes.

```javascript
'innerHTML'.dasherize();          // 'inner-html'
'action_name'.dasherize();        // 'action-name'
'css-class-name'.dasherize();     // 'css-class-name'
'my favorite items'.dasherize();  // 'my-favorite-items'
'privateDocs/ownerInvoice'.dasherize(); // 'private-docs/owner-invoice'
```

#### decamelize
Converts a camelized string into all lower case separated by underscores.

```javascript
'innerHTML'.decamelize();           // 'inner_html'
'action_name'.decamelize();        // 'action_name'
'css-class-name'.decamelize();     // 'css-class-name'
'my favorite items'.decamelize();  // 'my favorite items'
```

#### htmlSafe
returns `Handlebars.SafeString` A string that will not be HTML escaped by Handlebars.
Mark a string as safe for unescaped output with Ember templates. If you return HTML from a helper, use this function to ensure Ember's rendering layer does not escape the HTML.

```javascript
Ember.String.htmlSafe('<div>someString</div>')
```

#### isHTMLSafe
returns Boolean `true` if the string was decorated with `htmlSafe`, `false` otherwise.

```javascript
var plainString = 'plain string',
    safeString = Ember.String.htmlSafe('<div>someValue</div>');

Ember.String.isHTMLSafe(plainString); // false
Ember.String.isHTMLSafe(safeString);  // true
```

#### loc
This method formats the passed string but first checks if its a key in the `Ember.STRINGS` hash. By doing so it is possible to have a central point for defining the strings used in an application. This also allows internationalization. As stated in the source code its recommended - though not required - to prefix such a string with an underscore so it can be easily distinguished from `normal` strings.

```javascript
Ember.STRINGS = {
  '_Hello World': 'Bonjour le monde',
  '_Hello %@ %@': 'Bonjour %@ %@'
};

Ember.String.loc("_Hello World");  // 'Bonjour le monde';
Ember.String.loc("_Hello %@ %@", ["John", "Smith"]);  // "Bonjour John Smith";
```

#### underscore
More general than decamelize. Returns the lower_case_and_underscored form of a string.

```javascript
'innerHTML'.underscore();          // 'inner_html'
'action_name'.underscore();        // 'action_name'
'css-class-name'.underscore();     // 'css_class_name'
'my favorite items'.underscore();  // 'my_favorite_items'
'privateDocs/ownerInvoice'.underscore(); // 'private_docs/owner_invoice'
```