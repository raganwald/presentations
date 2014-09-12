![](images/part-3/473981158_cec8cb4f18_o.jpg)

^ https://www.flickr.com/photos/liberato/473981158

^ "A Unified Theory of JavaScript Style, Part III"

---

### the art of the javascript metaobject protocol

# Part III

![](images/part-3/473981158_cec8cb4f18_o.jpg)

^ https://www.flickr.com/photos/liberato/473981158

---

![](images/part-3/3356900874_a3893986af_o.jpg)

^ https://www.flickr.com/photos/tupwanders/3356900874

^ "We'll talk about Proxies and Polymorphism"

---

# Duck Typing

![](images/part-3/3356900874_a3893986af_o.jpg)

^ https://www.flickr.com/photos/tupwanders/3356900874

---

![](images/part-3/3986974018_41c229f122_o.jpg)

^ https://www.flickr.com/photos/macieklew/3986974018

---

# Fruit

![](images/part-3/3986974018_41c229f122_o.jpg)

^ https://www.flickr.com/photos/macieklew/3986974018

---

![](images/part-3/4547999416_d05fa35f80_o.jpg)

^ https://www.flickr.com/photos/yimhafiz/4547999416

---

# Not Fruit

![](images/part-3/4547999416_d05fa35f80_o.jpg)

^ https://www.flickr.com/photos/yimhafiz/4547999416

^ something can look, smell, and taste like fruit but not *be* fruit

---

![](images/part-3/54070471_1f5ffba40a_o.jpg)

^ https://www.flickr.com/photos/calliope/54070471

---

### "fruit" has a very specific meaning

![](images/part-3/54070471_1f5ffba40a_o.jpg)

^ https://www.flickr.com/photos/calliope/54070471

^ In botany, a fruit is a part of a flowering plant that derives from specific tissues of the flower, one or more ovaries, and in some cases accessory tissues. Fruits are the means by which these plants disseminate seeds. "Fruit" is defined in a semantic way that is deeper than the things we can observe of the fruit, deeper than its "interface."

---

![](images/part-3/13507827355_e25e417d73_o.jpg)

^ https://www.flickr.com/photos/raneko/13507827355

---

### let's consider
# Plus:

![](images/part-3/13507827355_e25e417d73_o.jpg)

^ https://www.flickr.com/photos/raneko/13507827355

---

![](images/part-3/7674654032_35752edf39_k.jpg)

^ https://www.flickr.com/photos/zigazou76/7674654032

---

### correction:

# Addition

![](images/part-3/7674654032_35752edf39_k.jpg)

^ https://www.flickr.com/photos/zigazou76/7674654032

---

![](images/part-3/9808827924_046897b6df_k.jpg)

^ https://www.flickr.com/photos/98925031@N08/9808827924

---

# "Enumerable"

![](images/part-3/9808827924_046897b6df_k.jpg)

^ https://www.flickr.com/photos/98925031@N08/9808827924

---

![](images/part-3/12909092113_866cbf935b_o.jpg)

^ https://www.flickr.com/photos/lasvegasinside/12909092113

---

### people are

# "Enumerable"

![](images/part-3/12909092113_866cbf935b_o.jpg)

^ https://www.flickr.com/photos/lasvegasinside/12909092113

---

![](images/part-3/90632001_46c99752b8_o.jpg)

^ https://www.flickr.com/photos/martinlabar/90632001

---

### rocks are

# "Enumerable"

![](images/part-3/90632001_46c99752b8_o.jpg)

^ https://www.flickr.com/photos/martinlabar/90632001

---

![](images/part-3/5748155319_0544532721_o.jpg)

^ https://www.flickr.com/photos/jwright4701/5748155319

---

### duck typing is

## "Enumerable," not "Fruit"

![](images/part-3/5748155319_0544532721_o.jpg)

^ https://www.flickr.com/photos/jwright4701/5748155319

---

![](images/part-3/turingmachine.jpg)

^ https://www.youtube.com/watch?v=DD0B-4KNna8

---

## Conway's Game of Life

![](images/part-3/turingmachine.jpg)

^ https://www.youtube.com/watch?v=DD0B-4KNna8

---

```javascript
function StandardCell () {
  this._neighbours = [];
  this._alive      = false;
}
```

---

```javascript
StandardCell.prototype.neighbours =
  function neighbours (neighbours) {
    return this._neighbours;
  };

StandardCell.prototype.setNeighbours =
  function setNeighbours (neighbours) {
    this._neighbours = neighbours.slice(0);
    return this;
  };
```

---

```javascript
StandardCell.prototype.alive =
  function alive () {
    return this._alive;
  };

StandardCell.prototype.setAlive =
  function setAlive (alive) {
    this._alive = alive;
    return this;
  };
```

---

![](images/part-3/9524854754_2cedd492ac_k.jpg)

^ https://www.flickr.com/photos/gsfc/9524854754

---

#### moving through
# Time

![](images/part-3/9524854754_2cedd492ac_k.jpg)

^ https://www.flickr.com/photos/gsfc/9524854754

---

```javascript
StandardCell.prototype.nextAlive =
  function nextAlive () {
    var alives =
      this._neighbours.filter(function (n) {
        return n.alive();
      }).length;
    if (this.alive()) {
      return alives === 2 ||
             alives == 3;
    }
    else {
      return alives == 3;
    }
  };
```

---

```javascript
Universe.prototype.iterate =
  function iterate () {
    var aliveInNextGeneration = this.cells().map(
      function (c) {
        return [c, c.nextAlive()];
      }
    );
    
    aliveInNextGeneration.forEach(function (a) {
      var cell = a[0],
          next = a[1];
          
      cell.setAlive(next);
    });
  };
```

---

![](images/part-3/6838117968_edcb258a9b_k.jpg)

^ https://www.flickr.com/photos/oskay/6838117968

---

### drawing

# Life

![](images/part-3/6838117968_edcb258a9b_k.jpg)

^ https://www.flickr.com/photos/oskay/6838117968

---

```javascript
View.prototype.drawCell =
  function drawCell (cell, x, y) {
    var xPlus = x + this.cellSize(),
        yPlus = y + this.cellSize()
    this._canvasContext.clearRect(x, y, xPlus, yPlus);
    this._canvasContext.fillStyle = cell.alive() 
                                    ? COLOURS.ALIVE 
                                    : COLOURS.BACKGROUND);
    this._canvasContext.fillRect(x, y, xPlus, yPlus);
    return self;
  };

// ...
```

---

![](images/part-3/maxresdefault.jpg)

^ https://www.youtube.com/watch?v=etUZw_xm-98 by Marcel Rodrigues

---

### redesigning for 

# Colour

![](images/part-3/maxresdefault.jpg)

^ https://www.youtube.com/watch?v=etUZw_xm-98 by Marcel Rodrigues

---

```javascript
function ColourCell () {
  this._neighbours = [];
  this._age        = 0;
}

ColourCell.prototype.neighbours =
  StandardCell.prototype.neighbours;


ColourCell.prototype.setNeighbours =
  StandardCell.prototype.setNeighbours;
```

---

```javascript
ColourCell.prototype.age =
  function age () {
    return this._age;
  };

ColourCell.prototype.setAge =
  function setAge (age) {
    this._age = age;
    return this;
  };
```

---

![](images/part-3/2599093453_e6a6492f93_o.jpg)

^ https://www.flickr.com/photos/dexxus/2599093453

---

### moving through time

# in Colour

![](images/part-3/2599093453_e6a6492f93_o.jpg)

^ https://www.flickr.com/photos/dexxus/2599093453

---

```javascript
ColourCell.prototype.nextAge =
  function next () {
    var alives =
      this._neighbours.filter(function (n) {
        return n.age() > 0;
      }).length;
    if (this.age() > 0) {
      return (alives === 2 || alives == 3) 
             ? (this.age() + 1)
             : 0;
    }
    else {
      return (alives == 3) 
             ? (this.age() + 1)
             : 0;
    }
  };
```

---

```javascript
Universe.prototype.iterate =
  function iterate () {
    var ageInNextGeneration = this.cells().map(
      function (c) {
        return [c, c.nextAge()];
      }
    );
    
    ageInNextGeneration.forEach(function (a) {
      var cell = a[0],
          next = a[1];
          
      cell.setAge(next);
    });
  };
```

---

![](images/part-3/4360710819_f1af19e346_o.jpg)

^ https://www.flickr.com/photos/kara_allyson/4360710819

---

### drawing life

# in Colour

![](images/part-3/4360710819_f1af19e346_o.jpg)

^ https://www.flickr.com/photos/kara_allyson/4360710819

---

```javascript

var COLOURS =
  [ BLACK, GREEN, BLUE, YELLOW, WHITE, RED ];

View.prototype.cellColour =
  function cellColour (cell) {
    return COLORS[
                   (cell.age() >= COLOURS.length)
                   ? (COLOURS.length - 1)
                   : cell.age()
                 ];
  };

// ...
```

---

![](images/part-3/8709649221_26caa39c4f_o.jpg)

^ https://www.flickr.com/photos/juliusprinz/8709649221

---

### something

# Doesn't Fit

![](images/part-3/8709649221_26caa39c4f_o.jpg)

^ https://www.flickr.com/photos/juliusprinz/8709649221

---

```javascript
Object.keys(StandardCell.prototype)
// =>
  [ 'neighbours',
    'setNeighbours',
    'alive',
    'setAlive',
    'nextAlive' ]
```

---

```javascript
Object.keys(ColourCell.prototype)
// =>
  [ 'neighbours',
    'setNeighbours',
    'age',
    'setAge',
    'nextAge' ]
```

---

![](https://farm1.staticflickr.com/6/5756772_26fbcbf5de_o_d.jpg)

^ https://www.flickr.com/photos/genista/5756772

^ "this is coupled"

---

### cell, universe, and view are
# coupled

![](https://farm1.staticflickr.com/6/5756772_26fbcbf5de_o_d.jpg)

^ https://www.flickr.com/photos/genista/5756772

---

![](images/part-3/6985514625_423a4001ea_k.jpg)

^ https://www.flickr.com/photos/42244964@N03/6985514625

^ "problems at scale"

---

### direct
# Duck Typing

![](images/part-3/6985514625_423a4001ea_k.jpg)

^ https://www.flickr.com/photos/42244964@N03/6985514625

---

```javascript
ColourCell.prototype.alive =
  function alive () {
    return this._age > 0;
  };
```

---

```javascript
ColourCell.prototype.setAlive =
  function setAlive (alive) {
    if (alive) {
      this.setAge(this.age() + 1);
    }
    else this.setAge(0);
    return this;
  };
```

---

```javascript
ColourCell.prototype.nextAlive =
  StandardCell.prototype.nextAlive;
```

---

```javascript
Object.keys(ColourCell.prototype)
// =>
  [ 'neighbours',
    'setNeighbours',
    'age',
    'setAge',
    'nextAge',
    'alive',
    'setAlive',
    'nextAlive' ]
```

---

![](images/part-3/4423502365_f4dbde780b_o.jpg)

^ https://www.flickr.com/photos/angeloangelo/4423502365

---

# Another Way

![](images/part-3/4423502365_f4dbde780b_o.jpg)

^ https://www.flickr.com/photos/angeloangelo/4423502365

---

```javascript
function AsStandard (colour) {
  this.it = colour;
}

var quacksLikeAStandardDuck =
  new AsStandard(aColourCell);
```

---

```javascript
AsStandard.prototype.neighbours = 
  function neighbours (neighbours) {
    return this.it.setNeighbours(neighbours);
    return this;
  };
  
AsStandard.prototype.setNeighbours = 
  function setNeighbours (neighbours) {
    this.it.setNeighbours(neighbours);
    return this;
  };
```

---

```javascript
AsStandard.prototype.alive =
  function alive () {
    return this.it.age() > 0;
  };
```

---

```javascript
AsStandard.prototype.setAlive =
  function setAlive (alive) {
    if (alive) {
      this.it.setAge(this.it.age() + 1);
    }
    else this.it.setAge(0);
    return this;
  };
```

---

```javascript
AsStandard.prototype.nextAlive =
  function nextAlive () {
    return this.it.nextAge() > 0;
  }
```

---

```javascript
Object.keys(AsStandard.prototype)
// =>
  [ 'setNeighbours',
    'alive',
    'setAlive',
    'nextAlive' ]
```

---

![](images/part-3/3459777668_61ca9c2b82_o.jpg)

^ https://www.flickr.com/photos/theincidental/3459777668

---

# AsStandard is an Adapter

![](images/part-3/3459777668_61ca9c2b82_o.jpg)

^ https://www.flickr.com/photos/theincidental/3459777668

---

> The adapter pattern is a software design pattern that allows the interface of an existing class to be used from another interface...

^ Wikipedia

---

> ... It is often used to make existing classes work with others without modifying their source code.

^ Wikipedia

---

![](images/part-3/2290758079_f5883b74a3_o.jpg)

^ https://www.flickr.com/photos/anaru/2290758079

---

### adapters can go both ways

# narrowing & widening 

![](images/part-3/2290758079_f5883b74a3_o.jpg)

^ https://www.flickr.com/photos/anaru/2290758079

------

```javascript
function AsColour (standard) {
  this.it = standard;
}

var quacksLikeAColouredDuck =
  new AsColour(aStandardCell);
```

---

```javascript
AsColour.prototype.setNeighbours = 
  function setNeighbours (neighbours) {
    this.it.setNeighbours(neighbours);
    return this;
  };
```

---

```javascript
AsColour.prototype.age =
  function age () {
    return this.it.alive()
           ? 1
           : 0;
  };
```

---

```javascript
AsColour.prototype.setAge =
  function setAge (age) {
    this.it.setAlive(age > 0);
    return this;
  };
```

---

```javascript
AsColour.prototype.nextAge =
  function nextAge () {
    return this.it.nextAlive()
           ? 1
           : 0;
  }
```

---

```javascript
Object.keys(AsColour.prototype)
// =>
  [ 'neighbours',
    'setNeighbours',
    'age',
    'setAge',
    'nextAge' ]
```

---

![](images/part-3/3268894584_95d4237010_o.jpg)

^ https://www.flickr.com/photos/fhke/3268894584

---

### good programmers borrow from smalltalk,

### great programmers

# Steal from C++

![](images/part-3/3268894584_95d4237010_o.jpg)

^ https://www.flickr.com/photos/fhke/3268894584

---

![](images/part-3/322057575_9404372473_o.jpg)

^ https://www.flickr.com/photos/jreed/322057575

---

### copy constructors are

# Value Adapters

![](images/part-3/322057575_9404372473_o.jpg)

^ https://www.flickr.com/photos/jreed/322057575

---

```javascript
function colourFromStandard (standard) {
  var colour = new ColourCell()
               .setNeighbours(standard.neighbours())
               .setAge(standard.alive() ? 1 : 0);
}
```

---

```javascript
function standardFromColour (colour) {
  var colour = new StandardCell()
               .setNeighbours(colour.neighbours())
               .setAlive(colour.age() > 0);
}
```

---

![](images/part-3/11689309615_98aaa0606c_k.jpg)

^ https://www.flickr.com/photos/samsaunders/11689309615

---

### adapters 

# separate concerns

![](images/part-3/11689309615_98aaa0606c_k.jpg)

^ https://www.flickr.com/photos/samsaunders/11689309615

---

![](images/part-3/9631393073_a965bb0929_o.jpg)

^ https://www.flickr.com/photos/94418464@N08/9631393073

---

### adapters
# Decouple Modules

![](images/part-3/9631393073_a965bb0929_o.jpg)

^ https://www.flickr.com/photos/94418464@N08/9631393073

---

![](images/part-3/3874139269_c9c16c41c8_o.jpg)

^ https://www.flickr.com/photos/russelljsmith/3874139269

---

### sometimes, you only want to

# Pretend to be a Duck

![](images/part-3/3874139269_c9c16c41c8_o.jpg)

^ https://www.flickr.com/photos/russelljsmith/3874139269

---

![](images/part-3/8598175927_1c63ac29ea_k.jpg)

^ https://www.flickr.com/photos/peterm7/8598175927

---

### instead of

# Conflating Interfaces

![](images/part-3/4380394959_28c761b7c6_o.jpg)

^ https://www.flickr.com/photos/automotocycle/4380394959

---

![](images/part-3/4380394959_28c761b7c6_o.jpg)

^ https://www.flickr.com/photos/automotocycle/4380394959

---

### consider writing an

# Adapter

![](images/part-3/8598175927_1c63ac29ea_k.jpg)

^ https://www.flickr.com/photos/peterm7/8598175927

---

# Reginald Braithwaite

## GitHub, Inc.

## raganwald.com

## @raganwald

Nordic JS
Artipelag, Stockholm, September 19, 2014

![right, fit](images/spessore.png)

^ https://leanpub.com/javascript-spessore