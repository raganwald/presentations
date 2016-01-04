![original](images/command/19987036192_b9f4db48ad_k.jpg)

^ https://www.flickr.com/photos/usgsbiml/19987036192

^ The Command Pattern

---

# The Command Pattern

![original](images/command/19987036192_b9f4db48ad_k.jpg)

^ https://www.flickr.com/photos/usgsbiml/19987036192

---

![original](images/command/9112267161_bb926bee3d_o.jpg)

^ https://www.flickr.com/photos/usdagov/9112267161

---

### the canonical example:

## A Mutable Data Structure

![](images/command/9112267161_bb926bee3d_o.jpg)

^ https://www.flickr.com/photos/usdagov/9112267161

---

![](images/command/16812936603_ac15e5c3e0_k.jpg)

---

## Operations

![](images/command/16812936603_ac15e5c3e0_k.jpg)

`insert(document, 42, 'foo')`

`delete(document, 40, 6)`

---

### Operations as Methods

`document.insert(42, 'foo')`

`document.delete(40, 6)`

---

### Operations as Functions: Thunks

`() => insert(document, 42, 'foo')`

---

## "Design Patterns"

### Implementing Undo and Redo with an Operation Stack

- never confuse an example given of a pattern with the underlying purpose of the pattern

- "higher-order invocation"

---

### Naked Objects

- validation responsibility

---

### RESTful Operations

---

### An Algebra of Operations

---

### Operational Transforms

---

### composing insertions and deletions

---

### reordering operations

- out-of-order undo
- replay vs transformation
- post-change transformations

---

### Eventual Consistency

- ? out-of-scope??

---

### Thunks

---

### Comparing Commands to Thunks

- functions are opaque
- operations that cannot be performed on opaque thinks

---

### Single Responsibility Principle

- using commands instead of methods for controllers

---

### Is trampolining a command pattern?
