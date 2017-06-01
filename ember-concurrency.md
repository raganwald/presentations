footer: Reginald @raganwald Braithwaite, PagerDuty Inc.

# `ember-concurrency`
### "an experience report"

---

## `ember-concurrency`

> ember-concurrency is an Ember Addon that makes it easy to write concise, robust, and beautiful asynchronous code.

--http://ember-concurrency.com/

---

# tasks and instances

```
aTask = task(function * () {
  const { geolocation, store } = this.get(...);

  const coords = yield geolocation.getCoords();
  const result = yield store.getNearbyStores(coords);
  this.set('result', result);
});

anInstance = someTask.perform();
```

^ example from the docs
^ looks a lot like async/await!

---

# problems
# ember-concurrency
# solves easily

---

## Mashing the "submit" button on an update

![inline 200%](images/ember-concurrency/mash.png)

^ sends multiple overlapping requests

---

# 💣

![inline](images/ember-concurrency/just-concurrent.png)

^ This is not what we want.

---

# concurrency protocols

```
task(function * () {
  // ...
}).drop()
```

#### (ember-concurrency calls these "modifiers")

---

# 🎉

![inline](images/ember-concurrency/just-drop.png)

---

## displaying a loading spinner

![inline 200%](images/ember-concurrency/loading.png)

---

# 💣

```
this.set('isLoading', true);
this.xhr = fetch(id).then(
  success => {
    this.set('isLoading', false);
    // ...
  },
  failure => {
    this.set('isLoading', false);
    // ...
  });
```

^ What if the component is destroyed?

---

# 🎉

```

isLoading: reads('fetchTask.isRunning')
```

^ tasks can be inspected

---

# using
# ember-concurrency
# to solve other problems

---

## chunking updates to our api

![inline 400%](images/ember-concurrency/resolving.png)

---

# progress

```
const chunks = _.chunk(incidents, CHUNK_SIZE);
let done = 0;

this.set('done', done);
for (const theseIncidents of chunks) {
  yield this.resolve(theseIncidents);
  done = done + theseIncidents.length);
  this.set('done', done);
}
```

---

![inline](images/ember-concurrency/stop.png)

---

# cancellation

```
aTask.cancelAll();

anInstance.cancel();
```

---

# a bigger challenge

---

> In JavaScript, asynchronous tasks happen concurrently and unreliably.

---

^ Job Switching Video

![autoplay mute](https://www.youtube.com/watch?v=_FLpXmxTbbM&t=1m30s)

---

# 💣

```
websocket ping: [-----------------]
submit update:     [-----------]
```

^ We are notified that an update is available for our model and execute a fetch. While this is happening in one component,
our user submits an update in the foreground.

---

# ❓

![inline](images/ember-concurrency/enqueued.png)

---

# sharing one task

```
task(function * (promissoryThunk) {
  let result;

  yield promissoryThunk().then(value => {
    result = value;
  });

  return result;
}).enqueue()
```

---

# serialized results

```
websocket ping: [--------]
submit update:            [-------]
```

---

# ⁉️

^ what happens to our `isLoading` indicator?
^ sharing queues amongst multiple component instances

---

# task groups

```
tasks: taskGroup().enqueue(),

handlePing: task(function * () {
              // ...
            }).group('tasks'),

submitUpdate: task(function * () {
                // ...
              }).group('tasks')
```

^ sharing task groups amongst multiple component instances

---

## ember-concurrency
# conclusion

---

## Simple things are easy,  complex things are *possible*.


