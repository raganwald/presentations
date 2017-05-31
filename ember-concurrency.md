footer: Reginald @raganwald Braithwaite, PagerDuty Inc.

# `ember-concurrency`
### "an experience report"

---

## `ember-concurrency`

> ember-concurrency is an Ember Addon that makes it easy to write concise, robust, and beautiful asynchronous code.

--http://ember-concurrency.com/

---

# the problem

---

> In JavaScript, asynchronous tasks happen concurrently and unreliably.

---

^ Job Switching Video

![autoplay mute](https://www.youtube.com/watch?v=_FLpXmxTbbM&t=1m30s)

---

### caveat:

## At PagerDuty, we manage asynchronous client-server tasks, not locally asynchronous tasks.

^ implications: unreliability, ordering

---

# problems that ember-concurrency solves

^ Mashing the "submit" button on an update
^ displaying a loading spinner
^ chunking updates to our api

---

## Mashing the "submit" button on an update

![inline 200%](images/ember-concurrency/mash.png)

---

## displaying a loading spinner

![inline 200%](images/ember-concurrency/loading.png)

---

## chunking updates to our api

![inline 400%](images/ember-concurrency/resolving.png)


---

## what ember-concurrency cannot do

- Handling two external signals
- Handling one internal and one external signal
