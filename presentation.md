# A Unified Theory of JavaScript Style

Reginald Braithwaite

GitHub, Inc.

Oslo, Norway, June 5, 2014

![](https://farm4.staticflickr.com/3342/3226981951_cec5a7db02_o_d.jpg)

^ https://www.flickr.com/photos/wwworks/3226981951

---

# Part I: JavaScript Combinators

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

# Part II: The Art of the JavaScript Metaobject Protocol

![](https://farm7.staticflickr.com/6203/6139595092_03a302ac43_o_d.jpg)

^ https://www.flickr.com/photos/frans16611/6139595092

---

## Encapsulation

![](https://farm9.staticflickr.com/8187/8134526531_ca358cc78d_o_d.jpg)

^ https://www.flickr.com/photos/lukepeterson/8134526531

---

> OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme late-binding of all things.
--Dr. Alan Kay

---

# Software entities should be open for extension, but closed for modification

![](https://farm4.staticflickr.com/3683/9715489429_43048e0f9c_o_d.jpg)

^ https://www.flickr.com/photos/jorbasa/9715489429
^ [The Open/Closed Principle](https://en.wikipedia.org/wiki/Open/closed_principle)

---

# Mutable metaobjects violate the open/closed principle

---

# extend considered harmful

![](https://farm9.staticflickr.com/8199/8178035999_b74a17a2a7_o_d.jpg)

^ https://www.flickr.com/photos/marfis75/8178035999

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