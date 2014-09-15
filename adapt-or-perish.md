![](images/part-3/473981158_cec8cb4f18_o.jpg)

^ https://www.flickr.com/photos/liberato/473981158

^ "A Unified Theory of JavaScript Style, Part III"

---

### the art of the javascript metaobject protocol

# Adapt or Perish

![](images/part-3/473981158_cec8cb4f18_o.jpg)

^ https://www.flickr.com/photos/liberato/473981158

---

![](images/part-3/5744858794_6e1150c74a_o.jpg)

^ https://www.flickr.com/photos/davelawler/5744858794

^ time is in short supply, so we'll talk about one thing

---

![](images/part-3/15064250645_3e3122b453_k.jpg)

^ https://www.flickr.com/photos/14052937@N04/15064250645

^ "fruit vs duck typing"

---

# Fruit vs Duck Typing

![](images/part-3/15064250645_3e3122b453_k.jpg)

^ https://www.flickr.com/photos/14052937@N04/15064250645

---

![](images/part-3/6359758713_29ac30e758_o.jpg)

^ https://www.flickr.com/photos/22711505@N05/6359758713

^ "fruit typing"

---

# Fruit Typing:

![](images/part-3/6359758713_29ac30e758_o.jpg)

^ https://www.flickr.com/photos/22711505@N05/6359758713

---

![](images/part-3/3986974018_41c229f122_o.jpg)

^ https://www.flickr.com/photos/macieklew/3986974018

^ "fruit"

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

^ "fruit is deeper than its taste, smell or color"

---

### "fruit" is deeper than its

# Taste, Smell or Color

![](images/part-3/54070471_1f5ffba40a_o.jpg)

^ https://www.flickr.com/photos/calliope/54070471

^ In botany, a fruit is a part of a flowering plant that derives from specific tissues of the flower, one or more ovaries, and in some cases accessory tissues. Fruits are the means by which these plants disseminate seeds. "Fruit" is defined in a semantic way that is deeper than the things we can observe of the fruit, deeper than its "interface."

---

![](images/part-3/3543936498_03a4b8d8c7_o.jpg)

^ https://www.flickr.com/photos/katdaned/3543936498

^ "duck typing"

---

# Duck Typing:

![](images/part-3/3543936498_03a4b8d8c7_o.jpg)

^ https://www.flickr.com/photos/katdaned/3543936498

---

![](images/part-3/9808827924_046897b6df_k.jpg)

^ https://www.flickr.com/photos/98925031@N08/9808827924

^ "enumerable"

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

### duck typing is like

## "Enumerable," not like "Fruit"

![](images/part-3/5748155319_0544532721_o.jpg)

^ https://www.flickr.com/photos/jwright4701/5748155319

---

> Duck typing is a style of typing in which an object's methods and properties determine the valid semantics,

---

> ...rather than its inheritance from a particular class or implementation of an explicit interface.

---

![](images/part-3/9704526768_e43ab62981_k.jpg)

^ https://www.flickr.com/photos/ter-burg/9704526768

---

### a problem we usually

# Solve with Duck Typing:

![](images/part-3/9704526768_e43ab62981_k.jpg)

^ https://www.flickr.com/photos/ter-burg/9704526768

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
    this._canvasContext.fillStyle = this.cellColour(cell);
    this._canvasContext.fillRect(x, y, xPlus, yPlus);
    return self;
  };
```

---

```javascript
View.prototype.cellColour =
  function cellColour (cell) {
    return cell.alive()
           ? WHITE
           : BLACK;
  };
```

---

![](images/part-3/6340506358_49e4687b0a_o.jpg)

^ https://www.flickr.com/photos/ourmaninjapan/6340506358

^ "Let's get to work!"

---

![](images/part-3/6636632951_c5d4f53f1c_o.jpg)

^ https://www.flickr.com/photos/nanagyei/6636632951

---

# Ch-ch-ch-changes!

![](images/part-3/6636632951_c5d4f53f1c_o.jpg)

^ https://www.flickr.com/photos/nanagyei/6636632951

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

![](images/part-3/14262980504_4b7f8ed0e4_k.jpg)

^ https://www.flickr.com/photos/120600995@N07/14262980504

---

### changes to "cell" ripple through
# Universe and View

![](images/part-3/14262980504_4b7f8ed0e4_k.jpg)

^ https://www.flickr.com/photos/120600995@N07/14262980504

---

![](images/part-3/183318800_100f6cc225_o.jpg)

^ https://www.flickr.com/photos/maxwarren/183318800

---

![](images/part-3/6985514625_423a4001ea_k.jpg)

^ https://www.flickr.com/photos/42244964@N03/6985514625

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

### there is

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
  function neighbours () {
    return this.it.neighbours();
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

![](images/part-3/3313053382_f128f329aa_o.jpg)

^ https://www.flickr.com/photos/andymangold/3313053382

---

![](images/part-3/2290758079_f5883b74a3_o.jpg)

^ https://www.flickr.com/photos/anaru/2290758079

---

### we can go in the

# Other Direction

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
AsColour.prototype.neighbours = 
  function neighbours () {
    return this.it.neighbours();
  };
  
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

![](images/part-3/3459777668_61ca9c2b82_o.jpg)

^ https://www.flickr.com/photos/theincidental/3459777668

---

### AsStandard and AsColour are

# Adaptors

![](images/part-3/3459777668_61ca9c2b82_o.jpg)

^ https://www.flickr.com/photos/theincidental/3459777668

---

> The adapter (sic) pattern is a software design pattern that allows the interface of an existing class to be used from another interface...

^ Wikipedia

---

> It is often used to make existing classes work with others without modifying their source code.

^ Wikipedia

---

![](images/part-3/3326060843_a5205e3183_o.jpg)

^ https://www.flickr.com/photos/mikaelmiettinen/3326060843

^ "homework: what are the implications of proxies and ownership?"

---

### homework: what are the implications of

# Proxies and Ownership?

![](images/part-3/3326060843_a5205e3183_o.jpg)

^ https://www.flickr.com/photos/mikaelmiettinen/3326060843

^ "Can we create proxy adaptors instead of wrapping adaptors?"

---

![](images/part-3/3268894584_95d4237010_o.jpg)

^ https://www.flickr.com/photos/fhke/3268894584

^ "good programmers borrow from smalltalk, great programmers steal from C++"

---

### good programmers borrow from smalltalk,

### great programmers

# Steal from C++

![](images/part-3/3268894584_95d4237010_o.jpg)

^ https://www.flickr.com/photos/fhke/3268894584

^ "copy constructors are value adaptors"

---

```javascript
function colourFromStandard (standard) {
  return new ColourCell()
         .setNeighbours(standard.neighbours())
         .setAge(standard.alive() ? 1 : 0);
}
```

---

```javascript
function standardFromColour (colour) {
  return new StandardCell()
         .setNeighbours(colour.neighbours())
         .setAlive(colour.age() > 0);
}
```

---

![](images/part-3/8733249464_07ba616ac4_o.jpg)

^ https://www.flickr.com/photos/jameshammond/8733249464

^ "if you use rails, you'd call these 'object migrations'"

---

### if you use rails, you'd call these

# "Object Migrations"

![](images/part-3/8733249464_07ba616ac4_o.jpg)

^ https://www.flickr.com/photos/jameshammond/8733249464

---

![](images/part-3/8271747835_b5e6f68251_k.jpg)

^ https://www.flickr.com/photos/esoastronomy/8271747835

^ "that's interesting!"

---

# Hmmm, that's interesting!

![](images/part-3/8271747835_b5e6f68251_k.jpg)

^ https://www.flickr.com/photos/esoastronomy/8271747835

---

> What if we could manage change with migrations between versions of classes?

---

![](images/part-3/4744866167_c631aa4e5f_o.jpg)

^ https://www.flickr.com/photos/34316967@N04/4744866167

^ Hmm some more!

---

![](images/part-3/6273266577_c37d3fec72_o.jpg)

^ https://www.flickr.com/photos/zeevveez/6273266577

^ "No time, mush rush on!"

---

![](images/part-3/11689309615_98aaa0606c_k.jpg)

^ https://www.flickr.com/photos/samsaunders/11689309615

^ "seprate the concern of how to interact with a model from the concern of what the model does"

---

### adaptors 

# Separate Concerns

![](images/part-3/11689309615_98aaa0606c_k.jpg)

^ https://www.flickr.com/photos/samsaunders/11689309615

^ "seprate the concern of how to interact with a model from the concern of what the model does"

---

![](images/part-3/14944977391_7ce4c95d19_k.jpg)

^ https://www.flickr.com/photos/xlibber/14944977391

---

### adaptors

# Isolate Change

![](images/part-3/14944977391_7ce4c95d19_k.jpg)

^ https://www.flickr.com/photos/xlibber/14944977391

---

![](images/part-3/9631393073_a965bb0929_o.jpg)

^ https://www.flickr.com/photos/94418464@N08/9631393073

---

### adaptors
# Decouple Modules

![](images/part-3/9631393073_a965bb0929_o.jpg)

^ https://www.flickr.com/photos/94418464@N08/9631393073

---

![](images/part-3/8503205792_d98abf5f13_o.jpg)

^ https://www.flickr.com/photos/dullhunk/8503205792

^ "adaptors *can* be used to solve compatibility problems"

---

### adaptors *can* be used to

# Solve Compatibility Problems

![](images/part-3/8503205792_d98abf5f13_o.jpg)

^ https://www.flickr.com/photos/dullhunk/8503205792

---

![](images/part-3/5020192587_220423f8fd_o.jpg)

^ https://www.flickr.com/photos/mathplourde/5020192587

^ "but they also gives us the freedom to make changes, safely"

---

### but they also gives us the freedom to

# Make Changes, Safely 

![](images/part-3/5020192587_220423f8fd_o.jpg)

^ https://www.flickr.com/photos/mathplourde/5020192587

---

![](images/part-3/15150575105_66b36a3f0e_k.jpg)

^ https://www.flickr.com/photos/kjcs/15150575105

^ "tack så mycket"

---

# Tack Så Mycket!

![](images/part-3/15150575105_66b36a3f0e_k.jpg)

^ https://www.flickr.com/photos/kjcs/15150575105

---

# Questions?

![](images/part-3/13262702873_4ae0e3429c_k.jpg)

^ https://www.flickr.com/photos/w00ter/13262702873

---

# Reginald Braithwaite

## GitHub, Inc.

## raganwald.com

## @raganwald

Nordic JS
Artipelag, Stockholm, September 19, 2014

![right, fit](images/spessore.png)

^ https://leanpub.com/javascript-spessore