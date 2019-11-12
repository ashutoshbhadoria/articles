# A Deeper Dive into JavaScript Objects And Prototypes

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

## Wait, where are the Classes ?

If you are coming from a background in statically typed languages (like I did), it's easy to get bamboozled here, what did I just do?
what is a beer? I haven't even defined a class beer.
The thing is, in a dynamically typed language we can skip the whole ceremony of creating the blueprints i.e. the classes or types ahead of time before we their instances aka. the objects.

Just create an object when you need one with the properties and methods you deem necessary. But another powerful feature of JavaScript objects is that you can change the entire shape of the object as and when you feel necessary. We created our `beer` object with two properties, `name` and `style`, later we felt that the `beer` needs to have a color, so we added a `color` property, similarly, we thought it would be good if our `beer` made a person happy, so that's what we did we added a meethod to our object `makePersonHappy`. This dynamic nature allows for more flexibility with less code and constrainsts.

Now this may seem fun for small scripts, but, especially after JavaScript has become a mainstay in the server side development ecosystem as well, a burning question is, HOW THE HECK DO I WRITE COMPLEX SYSTEMS ?

We will explore features JavaScrirpt provides to get some of the same benefits you can from  statically typed languages.

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

JavaScript provides a `new` keyword which followed by a function (constructor function) helps we create objects with the desired properties (and methods), without losing the dynamic nature of JavaScript objects. The constructor function is like any other JavaScript function with the first letter of it's name capitalized as a convention.

Let's just take a look at our new `Beer` object. There you can see that our lowercase `beer` variable is now a pointer to a `Beer` object, and that beer is named Guinness and is a Stout. So how exactly did that work? To really understand what is happening here, it's important that you understand what the keyword `this` is in JavaScript. The `this` keyword refers to an object. That object is whatever object is executing the current bit of code. By default, that is the `global` object. In a web browser, that is the `window` object. So when we executed this `Beer` function, what was `this` referring to? It was referring to a new empty object. That's what the `new` keyword does for us. It creates a new empty JavaScript object, sets the context of `this` to that new object, and then calls the Cat function. (If it doesn't make sense, please re-read this paragraph)

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

ES6 classes offer a relatively cleaner and very similar syntax to create objects which may seem familiar to class declarations in statically types languages.

### Using Object.create()

So far we've seen three ways to create JavaScript objects - the object literal, contructor functions and ES6 classes. But there is another way to create objeects and is actually how objects are created under thee hood even when we use the syntactic sugar available in the three ways we saw earlier.

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
