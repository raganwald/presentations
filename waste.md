build-lists: true
footer: © 2017 Reginald Braithwaite and PagerDuty, Inc.
slidenumbers: true

![](/Users/reg/work/presentations/images/waste/gov-waste.jpg)

^ https://www.flickr.com/photos/55229469@N07/10187983806

---

# Waste in Software Development

![](/Users/reg/work/presentations/images/waste/gov-waste.jpg)

^ https://www.flickr.com/photos/55229469@N07/10187983806

---

# What is **Waste**?

**Waste** is any material or activity that does not create _*value*_ for the customer.

^ Waste includes intellectual property as well as physical property. It includes processes as well as code.

^ "Customer" can include our own organization. For example, the customer for billing code is the finance department.

---

# The Lean Model of Software Development

![](/Users/reg/work/presentations/images/waste/lean.png)

---

![](/Users/reg/work/presentations/images/waste/lean.png)

^ Three activities and three assets.

^ "Code," "Product," and "Service" are interchangeable terms.

---

![](/Users/reg/work/presentations/images/waste/loop.png)

^ The output of each activity is an asset, which is the input of the next activity.

---

## Activities

- Build
- Learn
- Measure

^ The Lean methodology concerns itself deeply with wasteful activities. We aren't going to do that today.

---

## Assets

- Code
- Ideas
- Data

---

### **Code** Waste

- Code that is deleted before it creates value for the customer
- Code for features the customer does not value
- Code for features the customer cannot use

---

### **Ideas** Waste

- Ideas that are discarded before being turned into potential features
- Potential features we do not turn into code
- Ideas we cannot turn into code

---

### **Data** Waste

- Data that is insufficient for creating ideas
- Data we do not turn into ideas
- Data we cannot turn into ideas

---

## Our simplified model of waste

That which is:

- **Incomplete**, or;
- **Unvalued**, or;
- **Unused**.

---

![](images/waste/code.jpg)

^ https://www.flickr.com/photos/ruiwen/3260095534

---

# Code Waste

![](images/waste/code.jpg)

^ https://www.flickr.com/photos/ruiwen/3260095534

---

Code that is *Incomplete*, *Unvalued*, or *Unused* by the customer.

![](images/waste/code.jpg)

^ https://www.flickr.com/photos/ruiwen/3260095534

---

### **Incomplete** Code

- Dead-ends
- Epics
- Work-in-Progress

---

### **Unvalued** Code

- Architecture
- Tests

---

### **Unused** Code

- Misfeatures
- Over-optimizations

---

![](images/waste/firefox.jpg)

^ A story about this phone: I have one., but it sat in my drawer for three years. In theory, I could use it today, and yes, it would still create value. But for the three years during which it sat in my drawer, it was waste. Unfulfilled potential is waste.

---

# Unfulfilled Potential

![](images/waste/firefox.jpg)

^ A key issue with waste is unfulfilled potential. Code, Ideas, and Data can all become obsolete.

---

# Unfulfilled Code

Unfulfilled code is code that _would have_ created customer value, had it been in the customer's hands earlier.

**But it wasn't.**

^ And there is partial unfulfillment: It eventually got into the customer's hands, but there was a period of time during which it was complete, but not in the custoemrs hands, and not creating value: That time is waste.

---

# Unfulfilled Ideas

Unfulfilled ideas are ideas that _would have_ become features creating customer value, had we employed it to create features sooner.

**But we didn't.**

^ And there is partial unfulfillment for ideas: They eventually did become features, but there was a period of time during which they were complete, but not being used to create features: That time is waste.

---

# Unfulfilled Data


Unfulfilled data is data that _would have_ pointed us towards valuable ideas for creating customer value, had we considered it earlier than we did.

**But we didn't.**

^ And there is partial unfulfillment for data: It eventually did point us towards valuable ideas, but there was a period of time during which it was complete, but not being used to create ideas: That time is waste.

---

![](images/waste/containers.jpg)

^ https://www.flickr.com/photos/sarah_c_murray/19968259200/

---

## Unfulfillment is Waste

![](images/waste/containers.jpg)

^ https://www.flickr.com/photos/sarah_c_murray/19968259200/

---

### Almost There…

![](/Users/reg/work/presentations/images/waste/lean.png)

---

![](/Users/reg/work/presentations/images/waste/lean.png)

^ We have these three activities and three assets. Looking at "waste," we see that the purpose of data is to avoid wasteful ideas, and the purpose of ideas is to avoid wasteful code.

^ But we can also understand that the longer it takes for our activities, and the less efficient our activities are, the more unfulfillment appears in our data, ideas, and code. And that is wasteful.

^ So now we understand why lean practises advocate tightening this loop: The faster we go from building something to getting data from it, to ideas, to building the next thing, the less we waste on unfulfillment.

---

## Waste (again)

Waste is:

- **Incomplete**, or;
- **Unvalued**, or;
- **Unused**.

---

## Code Waste (again)

**Incomplete** code, such as dead-ends, epics, and work-in-progress.

**Unvalued** code, such as architecture and tests.

**Unused code**, such as misfeatures and over-optimizations.

**Unfulfilled** code, such as features that are ready, but not released.

---

# Five Easy Pieces

![](images/waste/exam.jpg)

^ https://www.flickr.com/photos/patrikaxelsson/2068480462

---

### Question One

# Is **technical debt** waste?

---

### Question Two

# Do **big releases** create waste?

---

### Question Three

# Is **tooling** actually waste?

---

### Question Four

## What does this model tell us about **continuous integration and deployment**?

---

### Question Five

# What does this model tell us about **big design up-front**?

---

### Bonus Question

# Are **technical talks** waste?

