### A Unified Theory of JavaScript Style, Part I

# JavaScript Combinators

![](https://farm3.staticflickr.com/2706/4378437185_4bdff99419_o_d.jpg)

^ https://www.flickr.com/photos/andy_li/4378437185

---

## It's Just Math

![](https://farm3.staticflickr.com/2693/4362414729_32f57b4f6d_o_d.jpg)

^ https://www.flickr.com/photos/joao_trindade/4362414729

---

> In mathematics, a *unary operation* is an operation with only one operand, i.e. a single input.

---

> In mathematics, a *binary operation* on a set is a calculation that combines two elements of the set to produce another element of the set.

^ In mathematics, a binary operation on a set is a calculation that combines two elements of the set (called operands) to produce another element of the set (more formally, an operation whose arity is two, and whose two domains and one codomain are (subsets of) the same set).

---

#Proposition:

## Dense is better than sparse

![](https://farm4.staticflickr.com/3749/9604185186_7038cfaa83_o_d.jpg)

^ https://www.flickr.com/photos/nathanoliverphotography/9604185186

![](https://farm1.staticflickr.com/54/110857520_7c8178c863_o_d.jpg)

^ https://www.flickr.com/photos/dipfan/110857520

---

# A simple combinator

```javascript
function flip (fn) {
	return function flipped (a, b) {
		return fn.call(this, b, a);
	}
}

function arrow (a, b) {
    return "" + a + " -> " + b;
}

flip(arrow)("x", "y")
	//=> 'y -> x'
```

^ http://jsfiddle.net/raganwald/wFRP8/

---

# A dash of curry

![](https://farm9.staticflickr.com/8490/8170948596_372d1960a5_o_d.jpg)

^ https://www.flickr.com/photos/saramarlowe/8170948596

---

```javascript
function curry (fn) {
	return function curried (a, optionalB) {
		if (arguments.length > 1) {
			return fn.call(this, a, optionalB);
		}
		else return function partiallyApplied (b) {
			return fn.call(this, a, b);
		}
	}
}
```

^ binary only

---
# Currying:

```javascript
var curriedArrow = curry(arrow);
	//=> [function]
	
curriedArrow('finger')('moon')
	//=> 'finger -> moon'
```

^ http://raganwald.com/2013/03/07/currying-and-partial-application.html

---

# Partial Application:

```javascript
var taoism = curry(arrow)('finger');
	//=> [function]
	
taoism('moon')
	//=> 'finger -> moon'
```

^ http://raganwald.com/2013/03/07/currying-and-partial-application.html

---

# Proposition:

## Partial application transforms binary operations into unary operations

![](https://farm1.staticflickr.com/4/4449316_3a315432b5_o_d.jpg)

^ https://www.flickr.com/photos/genista/4449316

---

```javascript
function property (object, property) {
	return object[property];
}

property({foo: 1}, 'foo')
	//=> 1
```

---

```javascript
var propertyWith = curry(flip(property));

propertyWith('foo')({foo: 1})
	//=> 1
```

---

```javascript
function map (mappable, fn) {
	return mappable.map(fn, this);
}

function double (n) {
    return n * 2;
}

map([1, 2, 3], double)
	//=> [2, 4, 6]
```

---

```javascript
var mapWith = curry(flip(map));

mapWith(double, [1, 2, 3]);
	//=> [2, 4, 6]

var doubleAll = mapWith(double);

doubleAll([1, 2, 3])
	//=> [2, 4, 6]
```

---

# Underscore

![](https://farm4.staticflickr.com/3813/12756787045_f9f0266bcc_o_d.jpg)

^ https://www.flickr.com/photos/mediterraneaaan/12756787045

---

`_.pluck` is "a convenient version of what is perhaps the most common use-case for `map`: extracting a list of property values."

```javascript
var stooges = [
	{name: 'moe', age: 40},
	{name: 'larry', age: 50},
	{name: 'curly', age: 60}];
	
_.pluck = function(obj, key) {
  return _.map(obj, _.property(key));
};

_.pluck(stooges, 'name');
	//=> ["moe", "larry", "curly"]
```

---

```javascript
function pluckWith (attr) {
  return mapWith(getWith(attr))
}

pluckWith('name')(stooges);
	//=> ["moe", "larry", "curly"]
```

---

# Compose

![](https://farm9.staticflickr.com/8376/8487304182_01e9c118c8_o_d.jpg)

^ https://www.flickr.com/photos/ctaweb/8487304182

---

```javascript
function compose (a, b) {
	return function composed (c) {
		return a(b(c));
	}
}

var pluckWith = compose(mapWith, getWith);

pluckWith('name')(stooges);
	//=> ["moe", "larry", "curly"]
```

---

### A Unified Theory of JavaScript Style, Part II

# The Art of the JavaScript Metaobject Protocol

![](https://farm7.staticflickr.com/6203/6139595092_03a302ac43_o_d.jpg)

^ https://www.flickr.com/photos/frans16611/6139595092

---

# Basics

![](https://farm1.staticflickr.com/33/49012397_1fbe7855e3_o_d.jpg)

^ https://www.flickr.com/photos/zscheyge/49012397

---

```javascript
var sam = {
  firstName: 'Sam',
  lastName: 'Lowry',
  fullName: function () {
    return this.firstName + " " + this.lastName;
  },
  rename: function (first, last) {
    this.firstName = first;
    this.lastName = last;
    return this;
  }
}
```

---

```javascript
var sam = {
  firstName: 'Sam',
  lastName: 'Lowry'
};

var Person = {
  fullName: function () {
    return this.firstName + " " + this.lastName;
  },
  rename: function (first, last) {
    this.firstName = first;
    this.lastName = last;
    return this;
  }
};
```

---

```javascript
function extend () {
  var consumer = arguments[0],
      providers = [].slice.call(arguments, 1),
      key,
      i,
      provider;

  for (i = 0; i < providers.length; ++i) {
    provider = providers[i];
    for (key in provider) {
      if (provider.hasOwnProperty(key)) {
        consumer[key] = provider[key];
      };
    };
  };
  return consumer;
}
```

---

# Mixins are many to _

```javascript
extend(sam, Person);

var peck = {
  firstName: 'Sam',
  lastName: 'Peckinpah'
};

extend(peck, Person);
```

---

# Mixins are _ to many

```javascript
var HasCareer = {
  career: function () {
    return this.chosenCareer;
  },
  setCareer: function (career) {
    this.chosenCareer = career;
    return this;
  }
};

extend(peck, HasCareer);

peck.setCareer('Director');
```

---

# Software entities should be open for extension, but closed for modification

![](https://farm4.staticflickr.com/3683/9715489429_43048e0f9c_o_d.jpg)

^ https://www.flickr.com/photos/jorbasa/9715489429
^ [The Open/Closed Principle](https://en.wikipedia.org/wiki/Open/closed_principle)

---

# `extend` considered harmful

## Mutable metaobjects violate the open/closed principle

![](https://farm9.staticflickr.com/8199/8178035999_b74a17a2a7_o_d.jpg)

^ https://www.flickr.com/photos/marfis75/8178035999

---

```javascript
function composeObjects () {
	return extend.apply(
		null,
		[{}].concat([].slice.call(arguments, 0))
	);
}

var Both = composeObjects(Person, Director);
```

---

## Encapsulation

![](https://farm9.staticflickr.com/8187/8134526531_ca358cc78d_o_d.jpg)

^ https://www.flickr.com/photos/lukepeterson/8134526531

---

> OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme late-binding of all things.
--Dr. Alan Kay

---

```javascript
function proxy (baseObject, optionalPrototype) {
  var proxyObject = Object.create(optionalPrototype || null),
      methodName;
  for (methodName in baseObject) {
    if (typeof(baseObject[methodName]) ===  'function') {
      (function (methodName) {
        proxyObject[methodName] = function () {
          var result = baseObject[methodName].apply(
						baseObject,
						arguments
					);
          return (result === baseObject)
                 ? proxyObject
                 : result;
        }
      })(methodName);
    }
  }
  return proxyObject;
}
```
---

```javascript
var stack = {
  array: [],
  index: -1,
  push: function (value) {
    return this.array[this.index += 1] = value;
  },
  pop: function () {
    var value = this.array[this.index];
    this.array[this.index] = void 0;
    if (this.index >= 0) {
      this.index -= 1;
    }
    return value;
  },
  isEmpty: function () {
    return this.index < 0;
  }
};
```

---

```javascript
var stackProxy = proxy(stack);

stackProxy.push('first');

stackProxy
  //=>
    { push: [Function],
      pop: [Function],
      isEmpty: [Function] }

stackProxy.pop();
  //=> first
```

---

# Open recursion considered harmful

![](https://farm1.staticflickr.com/224/497450458_860c31f0e9_o_d.jpg)

^ https://www.flickr.com/photos/furryscalyman/497450458

---

The **fragile base class problem** is a fundamental architectural problem of object-oriented programming systems where base classes (superclasses) are considered "fragile" because seemingly safe modifications to a base class, when inherited by the derived classes, may cause the derived classes to malfunction.

![original, right](https://farm4.staticflickr.com/3598/4081192022_2e79a7d42f_o_d.jpg)

^ https://www.flickr.com/photos/stevenduong/4081192022

---

![](https://farm4.staticflickr.com/3617/3572489968_0eecae201c_o_d.png)

^ https://www.flickr.com/photos/yugui/3572489968

---

# Encapsulation for Metaobjects

![](https://farm5.staticflickr.com/4121/4895250473_84ed3332e1_o_d.jpg)

^ https://www.flickr.com/photos/realsmiley/4895250473

---

![fit](images/7/proxy.png)

---

![fit](images/7/inner_proxy.png)

---

```javascript
var number = 0;

function encapsulate (behaviour) {
  var safekeepingName = "__" + ++number + "__",
      encapsulatedObject = {};

  Object.keys(behaviour).forEach(function (methodName) {
    var methodBody = behaviour[methodName];

    encapsulatedObject[methodName] = function () {
      var context = this[safekeepingName],
          result;
      if (context == null) {
        context = proxy(this);
        Object.defineProperty(this, safekeepingName, {
          enumerable: false,
          writable: false,
          value: context
        });
      }
      result = methodBody.apply(context, arguments);
      return (result === context) ? this : result;
    };
  });
  return encapsulatedObject;
}
```

^ includes safekeeping pattern

---

# Reginald Braithwaite

## GitHub, Inc.

## http://raganwald.com

NDC Conference, Oslo, Norway, June 5, 2014

![](https://farm4.staticflickr.com/3342/3226981951_cec5a7db02_o_d.jpg)

^ https://www.flickr.com/photos/wwworks/3226981951

---