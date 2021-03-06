# A Deep Dive into JavaScript Objects And Prototypes

For anybody who has been working with JavaScript even on a beginner level, has come across the notion of object in one's code. Remember the first program in JavaScript we wrote, it must have looked like `console.log('Hello World!')`. Where we used the `log` method of the `console` object.

Broadly speaking, objects in JavaScript can be defined as an unordered collection of related data, of primitive or reference types. This data is represented in the 'key: value' form. The keys can be variables or functions, which in the context of objects are referred to as properties and methods.

Without further ado, lets create our first object by using the object literal.

```js
var beer = {
  name: 'Guinness',
  style: 'Stout'
};
```

As we can see we just created an object with a name of `beer` and two properties which are `name` and `style`, with values `'Guinness'` and `'Stout'` respectively. We can access these properties very easily by using the `dot` operator.

```js
> console.log(beer.name);
  Guinness
> console.log(beer.style);
  Stout
```

Once an object is created using an object literal we can easily add additional properties to it, let's try to add a `color` property to our `beer` object and assign a value of `black` to it.

```js
beer.color = 'Black';
```

```js
> console.log(beer.color);
  Black
```

Similar to adding properties, methods can be added to our `beer` object very easily. We'll add a `makePersonHappy()` method to our object.

```js
beer.makePersonHappy = function() {
  console.log('Be happy, Good things come to those who wait.');
}

```

Let's execute this method right away,

```js
> beer.makePersonHappy();
  Be happy, Good things come to those who wait.
```

Also, deleting properties (or methods) from your object is very simple with the use of `delete` keyword, let's have a look at it in the code

```js
var beer = {
  name: 'Guinness',
  style: 'Stout',
  color: 'Black',
  makePersonParty: function() {
    console.log('Partyyyy!');
  }
};

delete beer.color;
delete beer.makePersonParty;
```

``` js
> console.log(beer);
  {name: "Guinness", style: "Stout"}
```

So, we can see the property `color` and the method `makePersonParty` are successfully deleted from our object `beer`.

## Wait, where are the Classes ?

If you are coming from a background in statically typed languages (like I did), it's easy to get bamboozled here, what did I just do?
what is a beer? I haven't even defined a class beer.
The thing is, in a dynamically typed language we can skip the whole ceremony of creating the blueprints i.e. the classes or types ahead of time before we their instances aka. the objects.

Just create an object when you need one with the properties and methods you deem necessary. But another powerful feature of JavaScript objects is that you can change the entire shape of the object as and when you feel necessary. We created our `beer` object with two properties, `name` and `style`, later we felt that the `beer` needs to have a color, so we added a `color` property, similarly, we thought it would be good if our `beer` made a person happy, so that's what we did we added a method to our object `makePersonHappy`. This dynamic nature allows for more flexibility with less code and less constrainsts.

Now this may seem fun for small scripts, but, especially after JavaScript has become a mainstay in the server side development ecosystem as well, a burning question is, HOW THE HECK DO I WRITE COMPLEX SYSTEMS ?

We will explore features JavaScript provides to get some of the same benefits you can from  statically typed languages.

## Creating Objects

### Using Constructor Functions

```js
function Beer() {
  this.name = 'Guinness';
  this.style = 'Stout';  
}

var beer = new Beer();
```

```js
> console.log(beer);
  Beer {name: "Guinness", style: "Stout"}
```

JavaScript provides a `new` keyword which followed by a function (constructor function) helps us create objects with the desired properties (and methods), without losing the dynamic nature of JavaScript objects. The constructor function is like any other JavaScript function with the first letter of it's name capitalized as a convention.

Let's just take a look at our new `Beer` object. There you can see that our lowercase `beer` variable is now a pointer to a `Beer` object, and that beer is named Guinness and is a Stout. So how exactly did that work? To really understand what is happening here, it's important that you understand what the keyword `this` is in JavaScript. The `this` keyword refers to an object. That object is whatever object is executing the current bit of code. By default, that is the `global` object. In a web browser, that is the `window` object. So when we executed this `Beer` function, what was `this` referring to? It was referring to a new empty object. That's what the `new` keyword does for us. It creates a new empty JavaScript object, sets the context of `this` to that new object, and then calls the `Beer` function. (If it doesn't make sense, please re-read this paragraph)

Let's now make out contructor function dynamic enough to create different beers.

```js
function Beer (name, style) {
  this.name = name;
  this.style = style;
}

var guinness = new Beer('Guinness', 'Stout');
var miller = new Beer('Miller', 'American Pilsner');
```

```js
> console.log(guinness);
  Beer {name: "Guinness", style: "Stout"}
> console.log(miller);
  Beer {name: "Miller", style: "American Pilsner"}
```

### Using ECMAScript 6 Classes

```js
class Beer {
  constructor (name, style) {
    this.name = name;
    this.style = style;
  }
}

var guinness = new Beer('Guinness', 'Stout');
var miller = new Beer('Miller', 'American Pilsner');
```

``` js
> console.log(guinness);
  Beer {name: "Guinness", style: "Stout"}
> console.log(miller);
  Beer {name: "Miller", style: "American Pilsner"}
```

ES6 classes offer a relatively cleaner and very similar syntax to create objects which may seem familiar to class declarations in statically typed languages.

### Using Object.create()

So far we've seen three ways to create JavaScript objects - the object literal, contructor functions and ES6 classes. But there is another way to create objects and is actually how objects are created under the hood even when we use the syntactic sugar available in the three ways we saw earlier.

```js
var guinness = Object.create(Object.prototype, {
  name: {
    value: 'Guinness',
    writable: true,
    iterable: true,
    configurable: true
  },
  style: {
    value: 'Stout',
    writable: true,
    iterable: true,
    configurable: true
  }
});
```

``` js
> console.log(guinness);
  Beer {name: "Guinness", style: "Stout"}
> console.log(miller);
  Beer {name: "Miller", style: "American Pilsner"}
```

Now all these properties while creating an object using `Object.create()` may seem very weird because most of the times we do not interact with them and they are oblivious to us, because the other ways of creating objects just abstracts us from that detail. But we'll have a look at them later.

## Object Properties

We have already seen creating objects with properties in the previous section, but there's a lot to object properties than it meets the eye. So far we have discussed accessing object properties with the `dot` notation, but there is an alternative and in some cases an essential construct to access object properties, the `bracket` notation.

```js
var beer = {
  name: 'Miller',
  style: 'American Pilsner'
}
```

```js
> console.log(beer.name) // accessing properties using dot notation
  Miller

> console.log(beer['name']) // accessing properties using bracket notation
  Miller
```

Just place the property name as a string (notice the single quotes) inside a bracket and we have an alternate syntax to aaccess object's properties.

What if we name our properties (or a data fetched as JSON from some source) which are not valid identifier names, in that case the dot notation will not work and we will have to use the bracket notation

```js
var beer = {
  'beer name': 'Kingfisher' // property name is invalid identifier
}
```

```js
> console.log(beer['beer name'])
  Kingfisher
```

Bracket notation is extremly useful when we want to access a property through a variable as a key.

```js
var beerStyleKey = 'style';

var beer = {
  name: 'Hoegarden',
  style: 'Belgian Wheat Beer'
}
```

```js
> console.log(beer[beerStyleKey]) // accessing the property
                                  // using variable as a key
  Belgian Wheat Beer
```

### Property Descriptors

Let us take a closer look at properties, they are more than a key-value pair, using `Object.getOwnPropertyDescriptor()` which returns a property descriptor for an own property. (we will look at the difference between an own property and a prototype property later). 

```js
var beer = {
  name: 'Guinness',
  style: 'Stout'
}
```

```js
> Object.getOwnPropertyDescriptor(beer, 'name');
  {value: "Guinness", writable: true, enumerable: true, configurable: true}
```

Now, in the output, we can see in addition to the property haaving a value, it also has writable, enumerable and configurable attributes.

### Writable Attribute

The writable attribute controls wether we can change the value of the property from the initial value.

For demonstrating this behaviour we are going to use the JavaScript [strict mode](https://dev.to/ashubhadoria7/hey-alice-what-s-the-big-deal-about-the-javascript-s-strict-mode-24bb), and we are going to use `Object.defineProperty()` which defines a new property directly on an object, or modifies an existing property on an object, and returns the object.

Consider our object `beer`

```js
'use strict';

var beer  = {
  name: 'Guinness',
  style: 'Stout'
};

// set the writable attribute for property style to false.
Object.defineProperty(beer, 'style', {writable: false});

```

```js
// try to change the style value for beer
> beer.style = 'Belgian Blond Beer';
  Uncaught TypeError: Cannot assign to read only property 'style' of object '#<Object>'
```

As expected trying to reassign a new value to `style` property results in a `TypeError` being thrown.

A word of caution the key concept here is we will not be able to REDECLARE a property. So if in case, the property is an object, we can still modify that object, but we cannot set it to other object.

```js
'use strict';

var beer = {
  name: 'Simba',
  placeOfOrigin: {
    city: 'Bangalore',
    country: 'India'
  }
}

Object.defineProperty(beer, 'placeOfOrigin', {writable: false});

beer.placeOfOrigin.city = 'Mumbai'; // works fine
beer.placeOfOrigin = {city: 'Moscow', country: 'Russia'}; // throws TypeError
```

### Enumerable Attribute

Whenever we want to list or print all the properties of an object we just throw in a good ol' `for...in` loop. By default, properties on an object are enumerable, meaning we can loop over them using a `for…in` loop. But we can change that. Let's set `enumerable` to `false` for the style property.

```js
'use strict';

var beer  = {
  name: 'Guinness',
  style: 'Stout'
};

Object.defineProperty(beer, 'style', {enumerable: false});

for (var key in beer) {
  console.log(`${key} -> ${beer[key]}`);
}
```
```js
// output
name -> Guinness
```

Well looks like our `style` property wasn't _enumerated_ (no pun intended).

Setting the `enumerable` attribute to false also has another important implication, the JSON serialization of the object. Let's have a look what happens to our `beer` object which has `enumerable` attribute for `style` set to false.

```js
> JSON.stringify(beer);
  "{"name":"Guinness"}"
```

We didn't get the `style` property in our _stringified_ object.

A convenient way to get all the keys (or attributes) of an object is to use the `Object.keys()` method, let's see what if we set `enumerable` attribute to false for a particular key.

```js
> Object.keys(beer);
  ["name"]
```

Again the only key showing up is the `name` key and not the `style` key.

Although we cannot _enumerate_ the `style` key in the `for...in` loop, or JSON _stringification_, or in `Object.keys()`, we still have it present on the object. Let's print out it's value.

```js
> console.log(beer.style);
  Stout
```

### Configurable attribute

The configurable attribute helps you lock down some property from being changed. It prevents the property from being deleted.

Lets see this in the code

```js
'use strict';

var beer = {
  name: 'Guinness',
  style: 'Stout'
}

Object.defineProperty(beer, 'style', {configurable: false});
```

```js
// try deleting the style property.
> delete beer.style;
  Uncaught TypeError: Cannot delete property 'style' of #<Object>
```

Also, after setting `configurable` attribute to `false` we cannot change the `enumerable` attribute of the object.

```js
> Object.defineProperty(beer, 'style', {enumerable: false});
  Uncaught TypeError: Cannot redefine property: style
```

Interestingly, once we set `configurable` atribute to `false`, we cannot flip it back to `true`.

```js
> Object.defineProperty(beer, 'style', {configurable: true});
  Uncaught TypeError: Cannot redefine property: style
```

However, note that we can still change the `writable` attribute on the `style` property.

## Getters and Setters in JavaScript

Getters and Setters are properties on an object that allow you to set the value of a property or return the value of property using a function. Thus, allowing for a more secure and robust way of assigning or retrieving values of object properties.

```js
var beer = {
  brand: 'Miler',
  type: 'Lite'
}
```

Now suppose we wanted to retrieve the full name of our `beer` as `'Miller Lite'` we could define a getter as follows,

```js
var beer = {
  brand: 'Miller',
  type: 'Lite'
}

Object.defineProperty(beer, 'fullBeerName', {
  get: function() {
    return `${this.brand} ${this.type}`
  }
});
```

Now let's see if our code works

```js
> console.log(beer.fullBeerName);
  Miller Lite
```
Well it does :smile:

What if we wanted to do the reverse of what we've done, that we could supply a value such as `'Miller Lite'` and it will set the `brand` property to `'Miller'` and `type` property to `'Lite'`. For this we need to define a setter.

```js
var beer = {
  brand: 'Miller',
  type: 'Lite'
}

Object.defineProperty(beer, 'fullBeerName', {
  get: function() {
    return `${this.brand} ${this.type}`
  },
  set: function(str) {
    var parts = str.split(' ');
    this.brand = parts[0];
    this.type = parts[1];
  }
});
```

Let's test this out,

```js
> beer.fullBeerName = 'Kingfisher Strong';
> console.log(beer);
  {brand: "Kingfisher", type: "Strong"}
```

It seems to work! We just set the `brand` and `type` property using a single assignment to `fullBeerName`.

## Prototypes

Before we define and discuss prototypes let's consider an example, suppose we want to have a property which could give us last element of the array we defined. But as JavaScript is a dynamic language we can add a new property to achieve this.

```js
var beers = ['Heineken', 'Miller', 'Tuborg'];

Object.defineProperty(beers, 'last', {
  get: function() {
    return this[this.length - 1];
  }
});
```

```js
> console.log(beers.last);
  Tuborg
```

> NOTE: Declaring and initializing arrays like we have done above by using the square bracket notation is just a simplified syntax for creating an Array using the Array constructor, `var beers = ['Heineken', 'Miller', 'Tuborg'];` is equivalent to `var beers = new Array('Heineken', 'Miller', 'Tuborg');`

However, the problem in this approach is, if we decide to define a new array we will need to define the `last` attribute again for that particular array. This approach is not extensible for all arrays.

If we define our `last` method on Array's prototype instead of the `beers` array we declared we will be able to achieve the expected behaviour.

```js
Object.defineProperty(Array.prototype, 'last', {
  get: function () {
    return this[this.length - 1];
  }
});
```

```js
> var beers = ['Heineken', 'Miller', 'Tuborg'];
> console.log(beers.last);
  Tuborg
> var gins = ['Bombay Sapphire', 'Gordon', 'Beefeater'];
> console.log(gins.last);
  Beefeater
```

Awesome.

### So What is a Prototype?

A prototype is an object that exists on every function in JavaScript. Caution, some convoluted definitions are coming up. A function's prototype is the object instance that will become the prototype for all objects created using this function as a constructor. An object's prototype is the object instance from which the object is inherited.

Let's have a look at these concepts through code.

```js
function Beer (name, style) {
  this.name = name;
  this.style = style;
}

var corona = new Beer ('Corona', 'Pale Lager');
```

```js
> Beer.prototype;
  Beer {}

> corona.__proto__;
  Beer {}

> Beer.prototype === corona.__proto__;
  true
```

In the above example, when we define the constructor function `Beer` a protype object is created. Then we create a `corona` object using the `Beer` constructor function we can see that the same prototype object instance is available in the `corona` object (the name of the prototype object instance is `__proto__` in case of the objects created from the constructor).

Let's tinker around with this prototype object.

```js
Beer.prototype.color = "Golden";
```

```js
> Beer.prototype;
  Beer { color: 'golden' }

> corona.__proto__;
  Beer { color: 'golden' }

> console.log(corona.color);
  "Golden"

> var guinness = new Beer('Guinness', 'Stout');
> guiness.color;
  "Golden"
```

We added a new property `color` to `Beer`'s prototype and because the objects created from the `Beer` constructor have the exact same prototype object instance, the changes in function's `prototype` object are reflected in `corona` object's `__proto__` object. Also, we can see another more practical effect of adding a property to the prototype object, we are able to access `color` property from all the objects created through `Beer` constructor using the simple `dot` notation. Let's discuss this in the next section.

### Instance and Prototype properties

Let us code up our previous example real quick

```js
function Beer (name, style) {
  this.name = name;
  this.style = style;
}

Beer.prototype.color = 'Black';

var guinness = new Beer('Guinness', 'Stout');
```

Now we'll head to our JavaScript console to draw some insights from the above example

```js
> (console.log(guinness.name);
  "Guinness"

> console.log(guinness.style);
  "Stout"

> console.log(guinness.color);
  "Black"
```

So far so good, we are getting expected values for all the three properties.

Just to be sure, let us list the properties of the `guinness` object.

```js
> Object.keys(guinness);
   ["name", "style"]
```

Wait what? Where is the `color` property we just accessed it's value. Let's double-check this.

```js
> guinness.hasOwnProperty('name');  // expected
  true

> guinness.hasOwnProperty('style'); // expected
  true

> guinness.hasOwnProperty('color') // Oh! Weird
  false
```

```js
> guinness.__proto__.hasOwnProperty('color'); // Hmmmm
  true
```

To explain this, `name` and `style` are the properties of the `guinness` object and are referred to as _Instance properties_, while `color` is a _Prototype property_.

While trying to access a property of an object (using the `dot` or the `square bracket` notation) the engine first checks if the property we are trying to access is an Instance property, if yes the value of the Instance property is returned. However, when the property is not found in the Instance properties of the object, a look up of Prototype properties is performed, if a corresponding matching property is found, it's value is returned.

Let's see one last example to drive this concept home.

```js
function Beer (name) {
  this.name = name;
}

Beer.prototype.name = 'Kingfisher';

var corona = new Beer('Corona');
```

```js
> console.log(corona.name);
  "Corona"
```

Even though the `name` property is available on the `prototype` it's value is not returned because first a look up of Instance properties is performed, where the property `name` was found and it's value of `"Corona"` is returned.


## Multiple Levels of Inheritance

```js
function Beer (name) {
  this.name = name;
}

var corona = new Beer('Corona');
```

We know now, that `corona` has a prototype and that it was created from the `Beer` function, as can be seen here.

```js
> corona.__proto__;
  Beer {}
```

But on close inspection we will see that the the `Beer` prototype also has a prototype.

```js
> corona.__proto__.__proto__;
  Object {}    // maybe represented as `{}` in some environments
```

This indicated that `Beer` objects inherit from `Object`. Let us try going up the prototype chain.

```js
> corona.__proto__.__proto__.__proto__;
  null
```

Looks like we've hit the roof. So to conclude this discussion, by default, all objects in JavaScript inherit from `Object`. And `Object` has no prototype. So almost all objects that we work with have some type of prototypal inheritance chain like this.

### Creating Prototypal Inheritance Chains

To create complex systems it is often essential we think of in terms of creating ample abstractions to make the system design cleaner, robust and reusable.

Let us try to create an abstraction for our `Beer` class, let us say `Beer` is a type of `Beverage`, and the `Beverage` happens to make people happy. So, we add a method to `Beverage`'s prototype `makePersonHappy()`. Now `Beer` being a `Beverage` should also be able to make people happy, right? Let us see how we can achieve this

```js
function Beverage() {
}

Beverage.prototype.makePersonHappy = function () {
  console.log('You are feeling so good!');
}

function Beer (name, style) {
  this.name = name;
  this.style = style;
}

Beer.prototype = Object.create(Beverage.prototype);

var guinness = new Beer('Guinness', 'Stout');
```

Let's see if `guinness` can make a person happy.

```js
> guinness.makePersonHappy();
  "You are feeling so good!"
```

So what happened was, when we defined the method `makePersonHappy()` on `Beverage`'s prototype, every object created from the `Beverage` function would have this method. If you look closely at the line of code 

```js
Beer.prototype = Object.create(Beverage.prototype);
```

This sets up a prototype chain from `Beer` to it's parent `Beverage` and therefore we are able to access the method `makePersonHappy()`. Let's verify this claim

```js
> console.log(guinness.__proto__.__proto__);
  Beverage { makePersonHappy: [Function] }
```

There is, however, one discrepancy here, let's print the `guinness` object.

```js
> console.log(guinness);
  Beverage { name: 'Guinness', style: 'Stout' }
```

Here the object `guinness` has `Beverage` as it's constructor, but we created this object using `Beer` function. Turns out we had overwritten the `constructor` property of the `Beer`'s prototype when we established the prototype chain. This can be easily amended by explicitly setting the `constructor` property of the prototype.

```js
Beer.prototype = Object.create(Beverage.prototype);
// explicitly setting the constructor
Beer.prototype.constructor = Beer;
```

Now, let's go to the console to verify this

```js
> console.log(guinness);
  Beer { name: 'Guinness', style: 'Stout' }
```

A lot of times we may decide to change some default behaviour provided by the parent to better suit the design of the system. Here we will try to override the message shown in `makePersonHappy()` method provided by the `Beverage`. Let us use every thing we have covered in this sub-section.

```js
function Beverage (message) {
  this.message = message || 'You are feeling so good!';
}

Beverage.prototype.makePersonHappy = function () {
  console.log(this.message);
}

function Beer (name, style) {
  // Call Beverage constructor
  Beverage.call(this, 'You have never felt better before!');
  this.name = name;
  this.style = style;
}

// Set prototype chain
Beer.prototype = Object.create(Beverage.prototype);
// Explicitly set constructor
Beer.prototype.constructor = Beer;

var guinness = new Beer('Guinness', 'Stout');
```

In order to call the `Beverage` constructor we use the JavaScript's `call` method which calls a function with a given `this` value and arguments provided individually. This is done to take care of any initializations that we intended to do  in the parent class, in this case we want to display a custom message from the `makePersonHappy()` method.

Let's verify if everything works fine.

```js
> guinness.makePersonHappy();
  "You have never felt better before!"

> guinness;
  Beer {
    message: 'You have never felt better before!',
    name: 'Guinness',
    style: 'Stout'
  }
```

### Using Class Syntax to Create Prototype Chains

The way to achieve prototypal inheritance using the moden ES6 class syntax is very similar and perhaps more cleaner than what we have seen. Recall how in an earlier section we created objects from classes, let's apply those concepts here.

```js
class Beverage {
  constructor (message) {
    this.message = message || 'You are feeling so good!';
  }

  makePersonHappy () {
    console.log(this.message);
  }
}

// Set up inheritance chain
class Beer extends Beverage {
  constructor (name, style) {
    // Call constructor of parent class
    super('You have never felt better before!');
    this.name = name;
    this.style = style;
  }
}

var guinness = new Beer('Guinness', 'Stout');
```

Here we use the `extends` keyword to set up the inheritance chain, and used the `super` keyword to call parent class' constructor.
Let's test this out.

```js
> guinness.makePersonHappy();
  "You have never felt better before!"

> console.log(guinness);
  Beer {
    message: 'You have never felt better before!',
    name: 'Guinness',
    style: 'Stout'
  }
```

Note that here we did not have to explicitly set the constructor of the `Beer`'s prototype.

## Summary

With this deeper understanding, we'll be able to create powerful and well-structured applications that take advantage of the dynamic power of JavaScript to create real-world apps tackling complexity and standing the test of the harsh production-environments.

Happy coding :sunglasses: