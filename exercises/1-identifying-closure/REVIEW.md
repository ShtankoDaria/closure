# event-loop 

## /1-identifying-closure

> no status: 4/14/2020, 6:01:05 PM 

[../REVIEW.md](../REVIEW.md)

* [/example-1-returning-functions.js](#example-1-returning-functionsjs) - example - no status
* [/example-2-never-creates-closure.js](#example-2-never-creates-closurejs) - example - no status
* [/example-3-always-creates-closure.js](#example-3-always-creates-closurejs) - example - no status
* [/example-4-sometimes-creates-closure-a.js](#example-4-sometimes-creates-closure-ajs) - example - no status
* [/example-5-sometimes-creates-closure-b.js](#example-5-sometimes-creates-closure-bjs) - example - no status

---

## /example-1-returning-functions.js

* example - no status
* [review source](./example-1-returning-functions.js)

```js
// functions can return functions that were passed as arguments

const argFunc = () => { };

const returnsOldFunction = (x) => { return x }; // does not create a closure

const sameFunctionAsArgument = returnsOldFunction(argFunc);
console.assert(sameFunctionAsArgument === argFunc,
  'no closure created, the returned function was declared outside of "returnsOldfunction"');;


const returnsNewFunction = (x) => {
  return function () { console.log(x) };
}
const newFunction = returnsNewFunction("hi!");
console.assert(newFunction !== argFunc,
  'a closure is created! the returned function was declared inside of "returnsNewFunction"');

// study this function call in JS Tutor to see closure in action:
newFunction();




```

[TOP](#event-loop)

---

## /example-2-never-creates-closure.js

* example - no status
* [review source](./example-2-never-creates-closure.js)

```js
// any function that returns a new function creates a closure
// returning a function that was passed as an argument does not create a closure
// the returned function must be declared inside the function call ("frame" on js tutor)

const doesItClose = (func, arg) => {
  const returnVal = func(arg);
  const returnedAFunction = typeof returnVal === 'function';
  const returnedArgument = arg === returnVal;

  const createsAClosure = returnedAFunction && !returnedArgument;
  return createsAClosure;
}

const never = (x) => {
  return x;
}

const whenPassed4 = doesItClose(never, 4);
console.assert(whenPassed4 === null, "... when passed 4");

const whenPassedAFunction = doesItClose(never, function () { });
console.assert(whenPassedAFunction === null, "... when passed a function");

const whenPassedAnArray = doesItClose(never, []);
console.assert(whenPassedAnArray === null, "... when passed an array");

const whenPassedItself = doesItClose(never, never);
console.assert(whenPassedItself === null, "... when passed itself");




```

[TOP](#event-loop)

---

## /example-3-always-creates-closure.js

* example - no status
* [review source](./example-3-always-creates-closure.js)

```js
// any function that returns a new function creates a closure
// returning a function that was passed as an argument does not create a closure
// the returned function must be declared inside the function call ("frame" on js tutor)

const doesItClose = (func, arg) => {
  const returnVal = func(arg);
  const returnedAFunction = typeof returnVal === 'function';
  const returnedArgument = arg === returnVal;

  const createsAClosure = returnedAFunction && !returnedArgument;
  return createsAClosure;
}

const always = (x) => {
  return function () {
    console.log(x)
  };
}

const whenPassed4 = doesItClose(always, 4);
const alwaysLogs4 = always(4);

const whenPassedAFunction = doesItClose(always, function bye() { });
const alwaysLogsHi = always(function hi() { });

const whenPassedAnArray = doesItClose(always, []);
const alwaysLogsArray = always([]);

const whenPassedItself = doesItClose(always, always);
const alwaysLogsAlways = always(always);

alwaysLogs4(), alwaysLogsHi(), alwaysLogsArray(), alwaysLogsAlways();
alwaysLogs4(), alwaysLogsHi(), alwaysLogsArray(), alwaysLogsAlways();

```

[TOP](#event-loop)

---

## /example-4-sometimes-creates-closure-a.js

* example - no status
* [review source](./example-4-sometimes-creates-closure-a.js)

```js
const doesItClose = (func, arg) => {
  const returnVal = func(arg);
  const returnedAFunction = typeof returnVal === 'function';
  const returnedArgument = arg === returnVal;

  const createsAClosure = returnedAFunction && !returnedArgument;
  return createsAClosure;
}

const sometimes1 = (x) => {
  if (typeof x === "function") {
    return x;
  } else {
    return function () {
      console.log(x)
    };
  }
}

const whenPassed4 = doesItClose(sometimes1, 4);
const resultFrom4 = sometimes1(4);
resultFrom4();

const whenPassedItself = doesItClose(sometimes1, sometimes1);
const resultFromItself = sometimes1(sometimes1);
resultFromItself();

const bye = () => console.log(x);
const whenPassedAFunction = doesItClose(sometimes1, bye);
const hi = () => console.log(x);
const resultFromFunction = sometimes1(hi);
resultFromFunction();

```

[TOP](#event-loop)

---

## /example-5-sometimes-creates-closure-b.js

* example - no status
* [review source](./example-5-sometimes-creates-closure-b.js)

```js
const doesItClose = (func, arg) => {
  const returnVal = func(arg);
  const returnedAFunction = typeof returnVal === 'function';
  const returnedArgument = arg === returnVal;

  const createsAClosure = returnedAFunction && !returnedArgument;
  return createsAClosure;
}

const sometimes2 = (x) => {
  if (typeof x === "function") {
    return function () {
      console.log(x)
    };
  } else {
    return x;
  };
}

const bye = () => console.log(x);
const whenPassedAFunction = doesItClose(sometimes2, bye);
const hi = () => console.log(x);
const resultFromFunction = sometimes2(hi);
resultFromFunction();

const whenPassedItself = doesItClose(sometimes2, sometimes2);
const resultFromItself = sometimes2(sometimes2);
resultFromItself();

const whenPassed4 = doesItClose(sometimes2, 4);
const resultFrom4 = sometimes2(4);
resultFrom4();

```

[TOP](#event-loop)

