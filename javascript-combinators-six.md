![original](https://farm3.staticflickr.com/2706/4378437185_4bdff99419_o_d.jpg)

^ https://www.flickr.com/photos/andy_li/4378437185

^ "A Unified Theory of JavaScript Style, Part I"

---

# JavaScript Combinators (2016)

![](https://farm3.staticflickr.com/2706/4378437185_4bdff99419_o_d.jpg)

^ https://www.flickr.com/photos/andy_li/4378437185

^ The "backbone" of this talk is to explain the combinators make software that is easier to factor and refactor by creating an "algebra" where functions form a semigroup, rather than using code, which is more flexibible but creates a sparse group

---

![](https://farm7.staticflickr.com/6132/5991539415_300af283a6_o_d.jpg)

^ https://www.flickr.com/photos/popculturegeek/5991539415

^ "talk about Combinators and decorators"

---

## we'll talk about
# Combinators and Decorators

![](https://farm7.staticflickr.com/6132/5991539415_300af283a6_o_d.jpg)

^ https://www.flickr.com/photos/popculturegeek/5991539415

---

![](https://farm4.staticflickr.com/3306/3521504068_3664448df2_o_d.jpg)

^ https://www.flickr.com/photos/creative_stock/3521504068

^ "think about flexibility and decluttering"

---

## and we'll think about
# Decomposition and Factoring

![](https://farm4.staticflickr.com/3306/3521504068_3664448df2_o_d.jpg)

^ https://www.flickr.com/photos/creative_stock/3521504068

---

![original](images/engine-start.jpg)

^ https://www.flickr.com/photos/npobre/2601582256

---

![original](images/hanging-lamp.jpg)

^ https://www.flickr.com/photos/westher/7709055816

---

# Decomposition

![](images/hanging-lamp.jpg)

^ https://www.flickr.com/photos/westher/7709055816

---

> We *decompose* entities to make discreet responsibilities explicit

---

# Example: A monolith

```javascript
Parse.User.logIn("user", "pass", {
  success: function (user) {
    query.find({
      success: function (users) {
        users[0].save({ key: value }, {
          success: function (user) {
            currentUser = user;
          }
        });
      }
    });
  }
});
```

^ Adapted from http://blog.parse.com/learn/engineering/whats-so-great-about-javascript-promises/

---

# Decomposed by extracting functions

```javascript
let assignCurrentUser = (user) => {
    currentUser = user;
  };

let saveFirstUser = (users) =>
  users[0].save({ key: value }, {
    success: assignCurrentUser
  });

let logUserIn = (user) =>
  query.find({
    success: saveFirstUser
  });

Parse.User.logIn("user", "pass", {
  success: logUserIn
});
```

^ Adapted from http://blog.parse.com/learn/engineering/whats-so-great-about-javascript-promises/

---

![original](images/rietveld-schroederhuis.jpg)

^ https://www.flickr.com/photos/mariannebevis/8646014265

---

# Composition

![](images/rietveld-schroederhuis.jpg)

^ https://www.flickr.com/photos/mariannebevis/8646014265

---

> We *compose* entities to make the relationships between them explicit

---

# Example: Promises explicitly compose asynchronous functions

```javascript
let findUser = (user) => query.find();

let saveFirstUser = (user) => users[0].save({ key: value });

let assignCurrentUser = (user) => {
    currentUser = user;
  };

Parse.User.logIn("user", "pass")
  .then(findUser)
  .then(saveFirstUser)
  .then(assignCurrentUser);
```

^ Adapted from http://blog.parse.com/learn/engineering/whats-so-great-about-javascript-promises/

---

![original](images/red-blue-chair.jpg)

^ https://www.flickr.com/photos/jforth/4467184640

---

# Decomposition is about entities

![](images/red-blue-chair.jpg)

^ https://www.flickr.com/photos/jforth/4467184640

---

# Composition is about relationships

![](images/red-blue-chair.jpg)

^ https://www.flickr.com/photos/jforth/4467184640

---

> Back to Decomposition

---

## extracting named functions

# The most obvious form of decomposition

---

![original](images/watch-movement.jpg)

^ https://www.flickr.com/photos/guysie/3325635275

---

# Extracting functions works from the *inside-out*

![](images/watch-movement.jpg)

^ https://www.flickr.com/photos/guysie/3325635275

---

## extracting named functions

# Decomposition of *Implementation*

---

> Let's look at something else

---

![original](https://c2.staticflickr.com/8/7743/17185509447_1166acf54b_h.jpg)

^ https://www.flickr.com/photos/130973380@N08/17185509447

^ "pluck"

---

## pluck: "A convenient version of what is perhaps the most common use-case for `map`, extracting a list of property values."

![](https://c2.staticflickr.com/8/7743/17185509447_1166acf54b_h.jpg)

^ https://www.flickr.com/photos/130973380@N08/17185509447

^ underscorejs.org

---

```javascript
let pluck = (collection, property) =>
  collection.map( (obj) => obj[property] );

var deStijl = [
  {name: 'Theo van Doesburg', occupation: 'theorist'},
  {name: 'Piet Mondriaan', occupation: 'painter'},
  {name: 'Gerrit Rietveld', occupation: 'architect'}];

pluck(deStijl, 'name')
  //=> ["Theo van Doesburg", "Piet Mondriaan", "Gerrit Rietveld"]
```

---

## functions have interfaces

# `pluck`'s *interface* has two parts: The collection, and the property

---

# Manually decomposing `pluck`'s interface

```javascript
let pluckFrom = (collection) =>
  (property) => pluck(collection, property);

var deStijl = [
  {name: 'Theo van Doesburg', occupation: 'theorist'},
  {name: 'Piet Mondriaan', occupation: 'painter'},
  {name: 'Gerrit Rietveld', occupation: 'architect'}];

pluckFrom(deStijl)('name')
  //=> ["Theo van Doesburg", "Piet Mondriaan", "Gerrit Rietveld"]
```

---

# Manually decomposing `pluck`'s interface

```javascript
let pluckWith = (property) =>
  (collection) => pluck(collection, property);

var deStijl = [
  {name: 'Theo van Doesburg', occupation: 'theorist'},
  {name: 'Piet Mondriaan', occupation: 'painter'},
  {name: 'Gerrit Rietveld', occupation: 'architect'}];

pluckFrom(deStijl)('name')
  //=> ["Theo van Doesburg", "Piet Mondriaan", "Gerrit Rietveld"]
```

---

> `pluckFrom` and `pluckWith` partially apply `pluck`

---

![origina](images/two-blue-doors.jpg)

^ https://www.flickr.com/photos/16210667@N02/18067981830

---

# Partial application decomposes functions from the *outside-in*

![](images/two-blue-doors.jpg)

^ https://www.flickr.com/photos/16210667@N02/18067981830

---

## partial application

# Decomposition of *Interface*

---

![original](images/lenses.jpg)

^ https://www.flickr.com/photos/vauvau/23067022785

---

# Decorators

![](images/lenses.jpg)

^ https://www.flickr.com/photos/vauvau/23067022785

---

> A *decorator* is a higher-order function that takes a function, and returns another function that adds to or modifies its argument's behaviour.

---

## Partial Application (again)

# Partial application decomposes a function from the outside-in

```javascript
let pluck = (collection, property) =>
  collection.map( (obj) => obj[property] );

// decomposes into:

let pluckFrom = (collection) =>
  (property) => pluck(collection, property);
```

^ http://raganwald.com/2013/03/07/currying-and-partial-application.html

---

# Extract closed-over binding

```javascript
let pluckFrom = (collection) =>
  (property) => pluck(collection, property);

// becomes:

let leftApply = (fn, a) =>
  (b) => fn(a, b);

let pluckFrom = (collection) =>
  leftApply(pluck, collection);
```

---

## Extract closed-over binding

# Closed over bindings can be extracted, just like parameters

---

## Extract closed-over binding

# Decomposing `pluckFrom` into `leftApply` gives us a decorator

---

> `leftApply` is a decorator that decomposes a function's interface

---

![](https://farm9.staticflickr.com/8490/8170948596_372d1960a5_o_d.jpg)

^ https://www.flickr.com/photos/saramarlowe/8170948596

---

## Hmmmm

# What does `leftApply(leftApply, leftApply)` do?

![](https://farm9.staticflickr.com/8490/8170948596_372d1960a5_o_d.jpg)

^ https://www.flickr.com/photos/saramarlowe/8170948596

---

## back to partial application

```javascript
let rightApply = (fn, b) =>
  (a) => fn(a, b);

let pluckWith = (property) =>
  rightApply(pluck, property);
```

---

## more decomposition with partial application

```javascript
let get = (object, property) =>
  object[property];

get({name: 'Gerrit Rietveld'}, 'name')
  //=> Gerrit Rietveld

let getWith = (property) => rightApply(get, property);

let nameOf = getWith('name');

nameOf({name: 'Gerrit Rietveld'})
  //=> Gerrit Rietveld
```

---

## more decomposition with partial application

```javascript
let map = (collection, fn) =>
  collection.map(fn);

let mapWith = (fn) => rightApply(map, property);
```

---

> Back to composition

---

![](https://farm9.staticflickr.com/8376/8487304182_01e9c118c8_o_d.jpg)

^ https://www.flickr.com/photos/ctaweb/8487304182

^ "Simple Composition"

---

# Simple Composition

![](https://farm9.staticflickr.com/8376/8487304182_01e9c118c8_o_d.jpg)

^ https://www.flickr.com/photos/ctaweb/8487304182

---

> `let compose = (a, b) =>`
> `  (c) => a(b(c));`

---

## `compose` in action

```javascript
var deStijl = [
  {name: 'Theo van Doesburg', occupation: 'theorist'},
  {name: 'Piet Mondriaan', occupation: 'painter'},
  {name: 'Gerrit Rietveld', occupation: 'architect'}];

let pluckWith = compose(mapWith, getWith);

let namesOf = pluckWith('name');

namesOf(deStijl)
  //=> ["Theo van Doesburg","Piet Mondriaan","Gerrit Rietveld"]
```

---

>  `pluckWith = compose(mapWith, getWith);`

> `compose` wires the output of one function into the input of another

---

![original](images/ode-to-mondriaan.jpg)

^ https://www.flickr.com/photos/72283508@N00/2444938101

^ "We compose entities to make the relationships between them explicit"

---

## reminder:
# *We compose entities to make the relationships between them explicit*

![](images/ode-to-mondriaan.jpg)

^ https://www.flickr.com/photos/72283508@N00/2444938101

---

> More Composition

---

```javascript
let mix = (...ingredients) => console.log('mixing', ...ingredients);

let bake = () => console.log('baking');

let cool = () => console.log('cooling');

let makeBread = (...ingredients) => {
  mix(...ingredients);
  bake();
  cool();
}
```

---

## composition with `before`

```javascript
let before = (fn, decoration) =>
  (...args) => {
    decoration(...args);
    return fn(...args);
  };

let bakeBread = before(bake, mix);

let makeBread = (...ingredients) => {
  bakeBread();
  cool();
}
```

---

> `before` makes the time relationship between two functions explicit

---

## composition with `after`

```javascript
let after = (fn, decoration) =>
  (...args) => {
    let returnValue = fn(...args);
    decoration(...args);
    return returnValue;
  };

let bakeBread = before(bake, mix);

let makeBread = after(bakeBread, cool);
```

---

> `after` also makes the time relationship between two functions explicit

---

# decomposing `before` and `after`

```javascript
let beforeWith = (decoration) =>
  rightApply(before, decoration);

let mixBefore = beforeWith((...ingredients) =>
  console.log('mixing', ...ingredients));

let bakeBread = mixBefore(bake);
```

---

# decomposing `before` and `after`

```javascript
let afterWith = (decoration) =>
  rightApply(after, decoration);

let coolAfter = afterWith(() =>
  console.log('cooling'));

let makeBread = coolAfter(bakeBread);
```

---

> `beforeWith` and `afterWith` are combinators that turn functions into decorators

---

# generalized guards

```javascript
function provided (guard) {
  return around(function () {
    var fn = arguments[0],
        values = [].slice.call(arguments, 1);

    if (guard.apply(this, values)) {
      return fn.apply(this, va

---

![original](https://farm7.staticflickr.com/6211/6210847796_ab54ea2b0b_o_d.jpg)

^ https://www.flickr.com/photos/68112440@N07/6210847796

^ "lessons"

---

## lessons

![](https://farm7.staticflickr.com/6211/6210847796_ab54ea2b0b_o_d.jpg)

^ https://www.flickr.com/photos/68112440@N07/6210847796

---

![original](https://farm8.staticflickr.com/7144/6792412657_f8dbe78eb1_o_d.jpg)

^ https://www.flickr.com/photos/paulmccoubrie/6792412657

^ "Combinators increase code flexibility and require increased mental flexibility"

---

## lesson one
# *Combinators increase code flexibility and require increased mental flexibility*

![](https://farm8.staticflickr.com/7144/6792412657_f8dbe78eb1_o_d.jpg)

^ https://www.flickr.com/photos/paulmccoubrie/6792412657

---

![original](https://farm1.staticflickr.com/54/107810852_44e98d9298_o_d.jpg)

^ https://www.flickr.com/photos/terry_wha/107810852

^ "Decorators declutter secondary concerns"

---

## lesson two
# *Decorators declutter secondary concerns*

![](https://farm1.staticflickr.com/54/107810852_44e98d9298_o_d.jpg)

^ https://www.flickr.com/photos/terry_wha/107810852

---

![original](https://farm2.staticflickr.com/1406/723665503_48c3a82af8_o_d.jpg)

^ https://www.flickr.com/photos/suburbanbloke/723665503

^ "Do not follow in the footsteps of the sages. Seek what they sought"

---

## lesson three
# *Do not follow in the footsteps of the sages.*
# *Seek what they sought*

![](https://farm2.staticflickr.com/1406/723665503_48c3a82af8_o_d.jpg)

^ https://www.flickr.com/photos/suburbanbloke/723665503

---

# Reginald Braithwaite

## PagerDuty

## raganwald.com

## @raganwald

NDC Conference, London, England, January 13, 2016

![right, fit](images/allonge-six.jpg)

^ https://leanpub.com/javascriptallongesix

---

> ANNEX

---

![original](https://farm6.staticflickr.com/5125/5317820857_7d86383407_o_d.jpg)

^ https://www.flickr.com/photos/justinbaeder/5317820857

^ Not all entities "fit together"

---

## interfaces
# *Not all entities "fit together"*

![](https://farm6.staticflickr.com/5125/5317820857_7d86383407_o_d.jpg)

^ https://www.flickr.com/photos/justinbaeder/5317820857

^ worse, some are time-dependent

---

![original](https://farm4.staticflickr.com/3260/2744016741_d3ca684983_o_d.jpg)

^ https://www.flickr.com/photos/michale/2744016741

^ "Homogeneous interfaces create dense spaces"

---

# Homogeneous interfaces create dense spaces

![](https://farm4.staticflickr.com/3260/2744016741_d3ca684983_o_d.jpg)

^ https://www.flickr.com/photos/michale/2744016741

^ Example: Integers and addition, multiplication: very dense

---

![original](https://farm6.staticflickr.com/5302/5572576057_86fc9b29cc_o_d.jpg)

^ https://www.flickr.com/photos/stretta/5572576057

^ "Heterogeneous interfaces create sparse spaces"

---

# Heterogeneous interfaces create sparse spaces

![](https://farm6.staticflickr.com/5302/5572576057_86fc9b29cc_o_d.jpg)

^ https://www.flickr.com/photos/stretta/5572576057

^ counter-example: imagine an operation on integers wher emost operations were invalid. you'd have to engage in  a complex search to find the path from any one integer to another.

---

![original](https://farm4.staticflickr.com/3260/2744016741_d3ca684983_o_d.jpg)

^ https://www.flickr.com/photos/michale/2744016741

![original](https://farm6.staticflickr.com/5302/5572576057_86fc9b29cc_o_d.jpg)

^ https://www.flickr.com/photos/stretta/5572576057

^ "Dense is more flexible than sparse"

---

# Dense is more flexible than sparse

![](https://farm4.staticflickr.com/3260/2744016741_d3ca684983_o_d.jpg)

^ https://www.flickr.com/photos/michale/2744016741

![](https://farm6.staticflickr.com/5302/5572576057_86fc9b29cc_o_d.jpg)

^ https://www.flickr.com/photos/stretta/5572576057

---

![original](https://farm1.staticflickr.com/54/110857520_7c8178c863_o_d.jpg)

^ https://www.flickr.com/photos/dipfan/110857520

![original](https://farm4.staticflickr.com/3749/9604185186_7038cfaa83_o_d.jpg)

^ https://www.flickr.com/photos/nathanoliverphotography/9604185186

^ "Sparse can be quicker to grasp"

---

# Sparse can be quicker to grasp

![](https://farm1.staticflickr.com/54/110857520_7c8178c863_o_d.jpg)

^ https://www.flickr.com/photos/dipfan/110857520

![](https://farm4.staticflickr.com/3749/9604185186_7038cfaa83_o_d.jpg)

^ https://www.flickr.com/photos/nathanoliverphotography/9604185186

^ we'll give an example, showing how we have to build up.

---

![original](https://farm3.staticflickr.com/2693/4362414729_32f57b4f6d_o_d.jpg)

^ https://www.flickr.com/photos/joao_trindade/4362414729

^ "enough with the math!"

---

## enough with the math!

![](https://farm3.staticflickr.com/2693/4362414729_32f57b4f6d_o_d.jpg)

^ https://www.flickr.com/photos/joao_trindade/4362414729

___

# A unary combinator

```javascript
let flip = (fn) =>
  (a, b) => fn(b, a);

let arrow = (a, b) =>
    `${a} -> ${b}`;

flip(arrow)("x", "y")
  //=> 'y -> x'
```

^ http://jsfiddle.net/raganwald/wFRP8/

---

# Currying:

```javascript
let curry = (fn) =>
  (a) => (b) => fn(a, b);

let arrow = (a, b) =>
    `${a} -> ${b}`;

let curriedArrow = curry(arrow);
  //=> [function]

curriedArrow('finger')('moon')
  //=> 'finger -> moon'
```

^ http://raganwald.com/2013/03/07/currying-and-partial-application.html


![](https://farm1.staticflickr.com/4/4449316_3a315432b5_o_d.jpg)

^ https://www.flickr.com/photos/genista/4449316

^ "Partial application transforms binary operations into unary operations"

---

## nota bene
# *Partial application transforms binary operations into unary operations*

![](https://farm1.staticflickr.com/4/4449316_3a315432b5_o_d.jpg)

^ https://www.flickr.com/photos/genista/4449316

---

> Why do we care about
> "unary operations?"

---

# Currying and partial application *decompose* functions

That allows us to re-compose them in different ways. We'll look at composition in a moment.

---

> Partial Application in Action

^ do a picture here
