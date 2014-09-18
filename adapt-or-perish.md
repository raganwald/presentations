![](images/part-3/9173790839_17092b33ba_k.jpg)

^ https://www.flickr.com/photos/avidday/9173790839

^ "Duck Typing, Compatibility and the Adaptor Pattern"

---

### the art of the javascript metaobject protocol

# Duck Typing, Compatibility, and
# The Adaptor Pattern

![](images/part-3/9173790839_17092b33ba_k.jpg)

^ https://www.flickr.com/photos/avidday/9173790839

^ "Duck Typing, Compatibility and the Adaptor Pattern"

---

![](images/part-3/5744858794_6e1150c74a_o.jpg)

^ https://www.flickr.com/photos/davelawler/5744858794

^ time is in short supply, so we'll jump right into talking about duck typing

---

> Duck typing is a style of typing in which an object's methods and properties determine the valid semantics,

---

> ...rather than its inheritance from a particular class or implementation of an explicit interface.

---

![](images/part-3/3986974018_41c229f122_o.jpg)

^ https://www.flickr.com/photos/macieklew/3986974018

^ "tastes like fruit"

---

# Tastes Like Fruit

![](images/part-3/3986974018_41c229f122_o.jpg)

^ https://www.flickr.com/photos/macieklew/3986974018

---

![](images/part-3/4547999416_d05fa35f80_o.jpg)

^ https://www.flickr.com/photos/yimhafiz/4547999416

^ "also tastes like fruit"

---

# Also Tastes Like Fruit

![](images/part-3/4547999416_d05fa35f80_o.jpg)

^ https://www.flickr.com/photos/yimhafiz/4547999416

---

> "Write code that depends upon tastes-like(fruit) rather than depending upon is-a(fruit)."

---

![](images/part-3/2692040215_614a8bd83c_o.jpg)

^ https://www.flickr.com/photos/hoeken/2692040215

^ "duck typing is a compatibility technique"

---

### duck typing is a

# Compatibility Technique

![](images/part-3/2692040215_614a8bd83c_o.jpg)

^ https://www.flickr.com/photos/hoeken/2692040215

---

![](images/part-3/3231308527_1d31e39093_o.jpg)

^ https://www.flickr.com/photos/davies/3231308527

---

# A Problem:

![](images/part-3/3231308527_1d31e39093_o.jpg)

^ https://www.flickr.com/photos/davies/3231308527

---

![](images/part-3/turingmachine.jpg)

^ https://www.youtube.com/watch?v=DD0B-4KNna8

---

## Conway's Game of Life

![](images/part-3/turingmachine.jpg)

^ https://www.youtube.com/watch?v=DD0B-4KNna8

---

> "Conway's Game of Life is a two-state cellular automata."

---

# An Implementation

1. Cells,

---

# An Implementation

1. Cells,
1. Universe,

---

# An Implementation

1. Cells,
1. Universe, and
1. View.

---

# Cell

1. Neighbours,

---

# Cell

1. Neighbours, and
1. State.

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

# Cell

1. Neighbours,
1. State, and
1. Transformation of State.

---

# Universe

1. Neighbourhoods (not shown),

---

# Universe

1. Neighbourhoods (not shown), and
1. Transformation of States.

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

# View

1. Drawing a Cell:

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

^ "5 minutes"

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

> The state of a cell is now "age" instead of "aliveness."

---

![](images/part-3/14262980504_4b7f8ed0e4_k.jpg)

^ https://www.flickr.com/photos/120600995@N07/14262980504

---

### These changes will
# Ripple Through Universe and View

![](images/part-3/14262980504_4b7f8ed0e4_k.jpg)

^ https://www.flickr.com/photos/120600995@N07/14262980504

---

### our problem is that

# Universe is coupled to Cell's Interface

---

![](images/part-3/183318800_100f6cc225_o.jpg)

^ https://www.flickr.com/photos/maxwarren/183318800

^ "ten minutes"

---

![](images/part-3/9704526768_e43ab62981_k.jpg)

^ https://www.flickr.com/photos/ter-burg/9704526768

---

### how smart people

# Solve the Problem

![](images/part-3/9704526768_e43ab62981_k.jpg)

^ https://www.flickr.com/photos/ter-burg/9704526768

---

> "Make a coloured cell behave like a standard cell."

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

# Presto! Backwards Compatibility.

### (it "tastes like a standard cell")

---

![](images/part-3/253281960_91f60e218b_o.jpg)

^ https://www.flickr.com/photos/91553413@N00/253281960

---

### universe and cell are

# No Longer Coupled

![](images/part-3/253281960_91f60e218b_o.jpg)

^ https://www.flickr.com/photos/91553413@N00/253281960

---

![](images/part-3/7569786950_8379220279_k.jpg)

^ https://www.flickr.com/photos/66971402@N04/7569786950

---

# At What Cost?

![](images/part-3/7569786950_8379220279_k.jpg)

^ https://www.flickr.com/photos/66971402@N04/7569786950

---

![](images/part-3/7670055210_ed05b8c732_k.jpg)

^ https://www.flickr.com/photos/bitterjug/7670055210

---

# Increasing Flexibility
# Increases Complexity

![](images/part-3/7670055210_ed05b8c732_k.jpg)

^ https://www.flickr.com/photos/bitterjug/7670055210

---

![](images/part-3/4423502365_f4dbde780b_o.jpg)

^ https://www.flickr.com/photos/angeloangelo/4423502365

---

### there is

# Another Way

![](images/part-3/4423502365_f4dbde780b_o.jpg)

^ https://www.flickr.com/photos/angeloangelo/4423502365

---

> "Make a cell that behaves like a Standard Cell, but is implemented in terms of a Coloured Cell."

---

```javascript
function AsStandard (colouredCell) {
  this._colouredCell = colouredCell;
}

var tastesLikeAStandardCell =
  new AsStandard(aColourCell);
```

---

```javascript
AsStandard.prototype.neighbours = 
  function neighbours () {
    return this._colouredCell.neighbours();
  };
  
AsStandard.prototype.setNeighbours = 
  function setNeighbours (neighbours) {
    this._colouredCell.setNeighbours(neighbours);
    return this;
  };
```

---

```javascript
AsStandard.prototype.alive =
  function alive () {
    return this._colouredCell.age() > 0;
  };
```

---

```javascript
AsStandard.prototype.setAlive =
  function setAlive (alive) {
    if (alive) {
      this._colouredCell.setAge(this._colouredCell.age() + 1);
    }
    else this._colouredCell.setAge(0);
    return this;
  };
```

---

```javascript
AsStandard.prototype.nextAlive =
  function nextAlive () {
    return this._colouredCell.nextAge() > 0;
  }
```

---

```javascript
Object.keys(AsStandard.prototype)
// =>
  [ 'neighbours',
    'setNeighbours',
    'alive',
    'setAlive',
    'nextAlive' ]
```

---

# When we Upgrade to ColourCell

1. View can depend upon ColourCell,

---

# When we Upgrade to ColourCell

1. View can depend upon ColourCell, and
1. Universe can depend upon AsStandard.

---

# Sweet!

1. Universe is decoupled from changes to Cell,

![](images/part-3/3705717858_9056efa2d0_o.jpg)

^ https://www.flickr.com/photos/56832361@N00/3705717858

---

# Sweet!

1. Universe is decoupled from changes to Cell, and
2. ColourCell isn't burdened with backwards compatibility.

![](images/part-3/3705717858_9056efa2d0_o.jpg)

^ https://www.flickr.com/photos/56832361@N00/3705717858

---

![](images/part-3/3459777668_61ca9c2b82_o.jpg)

^ https://www.flickr.com/photos/theincidental/3459777668

---

# AsStandard is an *Adaptor*

![](images/part-3/3459777668_61ca9c2b82_o.jpg)

^ https://www.flickr.com/photos/theincidental/3459777668

---

> The adapter (sic) pattern is a software design pattern that allows the interface of an existing class to be used from another interface...

^ Wikipedia

---

> It is often used to make existing classes work with others without modifying their source code.

^ Wikipedia

---

# TIMTOWTDI

---

![](images/part-3/3268894584_95d4237010_o.jpg)

^ https://www.flickr.com/photos/fhke/3268894584

^ "good programmers borrow from smalltalk, great programmers steal from C++"

---

### good programmers borrow from smalltalk,

# Great Programmers Steal from C++

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

![](images/part-3/6885969828_513d3964d1_k.jpg)

^ https://www.flickr.com/photos/e3000/6885969828

^ "Can we create co-proxies instead of wrappers?"

---

# Coproxies

![](images/part-3/6885969828_513d3964d1_k.jpg)

^ https://www.flickr.com/photos/e3000/6885969828

---

![](images/part-3/6273266577_c37d3fec72_o.jpg)

^ https://www.flickr.com/photos/zeevveez/6273266577

^ "fifteen minutes"

---

![](images/part-3/6081198171_99a3ddabe8_o.jpg)

^ https://www.flickr.com/photos/warriorwoman531/6081198171

^ "The conclusion is near"

---

# Approaching the End

![](images/part-3/6081198171_99a3ddabe8_o.jpg)

^ https://www.flickr.com/photos/warriorwoman531/6081198171

---

![](images/part-3/11689309615_98aaa0606c_k.jpg)

^ https://www.flickr.com/photos/samsaunders/11689309615

^ "separate the concern of how to interact with a model from the concern of what the model does"

---

### adaptors 

# Separate Concerns

![](images/part-3/11689309615_98aaa0606c_k.jpg)

^ https://www.flickr.com/photos/samsaunders/11689309615

^ "separate the concern of how to interact with a model from the concern of what the model does"

---

![](images/part-3/14944977391_7ce4c95d19_k.jpg)

^ https://www.flickr.com/photos/xlibber/14944977391

---

### and thus, adaptors

# Isolate Change

![](images/part-3/14944977391_7ce4c95d19_k.jpg)

^ https://www.flickr.com/photos/xlibber/14944977391

---

![](images/part-3/9631393073_a965bb0929_o.jpg)

^ https://www.flickr.com/photos/94418464@N08/9631393073

---

### and they
# Decouple Modules

![](images/part-3/9631393073_a965bb0929_o.jpg)

^ https://www.flickr.com/photos/94418464@N08/9631393073

---

![](images/part-3/14188094421_d72a63c2dd_k.jpg)

^ https://www.flickr.com/photos/janitors/14188094421

^ "adaptors solve compatibility problems"

---

### adaptors

# Solve Compatibility Problems

![](images/part-3/14188094421_d72a63c2dd_k.jpg)

^ https://www.flickr.com/photos/janitors/14188094421

---

![](images/part-3/14874539886_f8ad4b6880_k.jpg)

^ https://www.flickr.com/photos/highwaysagency/14874539886

^ "but they also gives us the freedom to make changes, safely"

---

### but they also gives us the freedom to

# Make Changes, Safely 

![](images/part-3/14874539886_f8ad4b6880_k.jpg)

^ https://www.flickr.com/photos/highwaysagency/14874539886

---

![](images/part-3/6970784086_b96b7163a4_k.jpg)

^ https://www.flickr.com/photos/jacme31/6970784086

---

### when we're using teams to build software at scale

# Adaptors are an Important Tool

![](images/part-3/6970784086_b96b7163a4_k.jpg)

^ https://www.flickr.com/photos/jacme31/6970784086

---

![](images/part-3/11671457605_3117e67919_k.jpg)

^ https://www.flickr.com/photos/peddhapati/11671457605

^ THE END

---

![](images/part-3/3391592144_3efb85a652_o.jpg)

^ https://www.flickr.com/photos/swimparallel/3391592144/

^ "eighteen minutes"

---

one more thing:

---

![](images/part-3/4772680734_489299f43f_o.jpg)

^ https://www.flickr.com/photos/49889874@N05/4772680734

^ "packages"

---

![](images/part-3/7994149144_2d3052dbd3_o.jpg)

^ https://www.flickr.com/photos/seeminglee/7994149144

---

### when you're holding a

# Version-Shaped Hammer

![](images/part-3/7994149144_2d3052dbd3_o.jpg)

^ https://www.flickr.com/photos/seeminglee/7994149144

---

![](images/part-3/8733249464_07ba616ac4_o.jpg)

^ https://www.flickr.com/photos/jameshammond/8733249464

---

### all changes look like

# Migration-Shaped Nails

![](images/part-3/8733249464_07ba616ac4_o.jpg)

^ https://www.flickr.com/photos/jameshammond/8733249464

---

![](images/part-3/7213010794_fa1e4d89dd_k.jpg)

^ https://www.flickr.com/photos/poorfish/7213010794

---

# Packages Have Interlocking Dependencies

![](images/part-3/7213010794_fa1e4d89dd_k.jpg)

^ https://www.flickr.com/photos/poorfish/7213010794

---

![](images/part-3/4918018946_694b19ef5b_o.jpg)

^ https://www.flickr.com/photos/imazerart/4918018946

---

# Migrations Can Cascade

### you never know how much work a single upgrade can cause

![](images/part-3/4918018946_694b19ef5b_o.jpg)

^ https://www.flickr.com/photos/imazerart/4918018946

---

![](images/part-3/3466699346_e09bc9dc93_o.jpg)

^ https://www.flickr.com/photos/sidelong/3466699346

^ "semver: does a new version have to be duck-compatible? Or can it adapt?"

---

### migration is expensive, so semver values

# Backwards Compatibility

![](images/part-3/3466699346_e09bc9dc93_o.jpg)

^ https://www.flickr.com/photos/sidelong/3466699346

---

![](images/part-3/13026781635_a871791f11_o.jpg)

^ https://www.flickr.com/photos/aigle_dore/13026781635

---

### "duck typed" backwards compatibility

# Holds Progress Back

![](images/part-3/13026781635_a871791f11_o.jpg)

^ https://www.flickr.com/photos/aigle_dore/13026781635

---

![](images/part-3/14040288979_6590cceb2c_k.jpg)

^ https://www.flickr.com/photos/drphotomoto/14040288979

---

### breaking compatibilty

# Exposes Clients to Bugs

![](images/part-3/14040288979_6590cceb2c_k.jpg)

^ https://www.flickr.com/photos/drphotomoto/14040288979

---

![](images/part-3/15083719955_e83cfe0922_k.jpg)

^ https://www.flickr.com/photos/streetmatt/15083719955

---

### adaptors decouple

# Compatibility from Progress

![](images/part-3/15083719955_e83cfe0922_k.jpg)

^ https://www.flickr.com/photos/streetmatt/15083719955

---

![](images/part-3/2216528316_b3a0380048_o.jpg)

^ https://www.flickr.com/photos/sputnik_mania/2216528316

^ "adaptors can provide safe backwards-compatibility without debt"

---

### adaptors provide

# Safe Compatibility

![](images/part-3/2216528316_b3a0380048_o.jpg)

^ https://www.flickr.com/photos/sputnik_mania/2216528316

---

![](images/part-3/2353845688_d2080e2d24_o.jpg)

^ https://www.flickr.com/photos/halfbisqued/2353845688

^ What would an adaptor-aware package manager look like?

---

# What would an adaptor-aware package manager look like?

![](images/part-3/2353845688_d2080e2d24_o.jpg)

^ https://www.flickr.com/photos/halfbisqued/2353845688

---

# An Adaptor-Aware Package Manager

1. Interfaces and Implementations would be independantly versioned,

---

# An Adaptor-Aware Package Manager

1. Interfaces and Implementations would be independantly versioned,
1. Interfaces would have a many-to-many relationship with Implementations, mediated by Adaptors,

---

# An Adaptor-Aware Package Manager

1. Interfaces and Implementations would be independantly versioned,
1. Interfaces would have a many-to-many relationship with implementations, mediated by adaptors,
1. Clients would specifiy the required interface and implementation,

---

# An Adaptor-Aware Package Manager

1. Interfaces and Implementations would be independantly versioned,
1. Interfaces would have a many-to-many relationship with implementations, mediated by adaptors,
1. Clients would specifiy the required interface and implementation, and
1. Old clients can benefit from bug fixes without rewrites.

---

![](images/part-3/6171444454_573f37de59_o.jpg)

^ https://www.flickr.com/photos/wackybadger/6171444454

---

### an adaptor-aware package manager would

# Rewire the Way we Grow Software

![](images/part-3/6171444454_573f37de59_o.jpg)

^ https://www.flickr.com/photos/wackybadger/6171444454

---

![](images/part-3/8271747835_b5e6f68251_k.jpg)

^ https://www.flickr.com/photos/esoastronomy/8271747835

^ "and that provides food for thought for a very long time indeed"

---

### and that provides

# Food for Thought

### for a very long time indeed

![](images/part-3/8271747835_b5e6f68251_k.jpg)

^ https://www.flickr.com/photos/esoastronomy/8271747835

---

![](images/part-3/14175437597_2da2d44778_k.jpg)

^ https://www.flickr.com/photos/lionsthlm/14175437597

^ "tack så mycket"

---

# Tack Så Mycket!

![](images/part-3/14175437597_2da2d44778_k.jpg)

^ https://www.flickr.com/photos/lionsthlm/14175437597

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