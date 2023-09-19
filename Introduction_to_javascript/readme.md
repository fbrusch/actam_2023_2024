# Introduction to Javascript

- We'll introduce Javascript trying to follow the footsteps of our adventure with the birthday paradox in Python
- We'll use the Javascript interpreter in the browser

# Do we have expressions?




```javascript
1
```




    1




```javascript
1+1
```




    2




```javascript
2**1000
```




    1.0715086071862673e+301



## do we have Big Numbers?


```javascript
2n**1000n
```




    10715086071862673209484250490600018105614048117055336074437503883703510511249361224931983788156958581275946729175531468251871452856923140435984577574698574803934567774824230985421074605062371141877954182153046474983581941267398767559165543946077062914571196477686542167660429831652624386837205668069376n



* numbers are represented as floating point by default
* but we also have arbitrary precision integers (BigInt)
  

## Do we have variables?


```javascript
"use strict"; // forces to be annoying with language syntax
a = 1
```




    1



- JavaScript requires explicit variable declaration
* This is done using one of the variable declaration keywords: var, let, const

`var`: This keyword is used to declare a variable. It has _function scope_.
`let`: Introduced in ES6, `let` allows to declare _block-scoped_ local variables. 
`const`: Also introduced in ES6, const is used to declare variables whose values should not be re-assigned.

## Variable variables


```javascript
var a = 1
console.log(a)
a = 2
console.log(a)
```

    1
    2


## Constant (?!) variables


```javascript
const b = 3
b = 4
```


    evalmachine.<anonymous>:1

    const b = 3

    ^

    

    SyntaxError: Identifier 'b' has already been declared

        at Script.runInThisContext (node:vm:129:12)

        at Object.runInThisContext (node:vm:307:38)

        at run ([eval]:1020:15)

        at onRunRequest ([eval]:864:18)

        at onMessage ([eval]:828:13)

        at process.emit (node:events:513:28)

        at emit (node:internal/child_process:944:14)

        at process.processTicksAndRejections (node:internal/process/task_queues:83:21)


## Can we generate random numbers?


```javascript
Math.random()
```




    0.7233167978012027



What is `Math`?


```javascript
Math
```




    Object [Math] {}



It's an *object*!

## Everything is an object

* In JavaScript, nearly everything is an object
* An object is a collection of properties
* A property is an association between a name (or key) and a value
* The value is an object, and can have properties etc
* When the value is a _function_, it's called a _method_.

What are the methods of `Math`?


```javascript
Object.getOwnPropertyNames(Math)
```




    [
      'abs',    'acos',    'acosh',  'asin',
      'asinh',  'atan',    'atanh',  'atan2',
      'ceil',   'cbrt',    'expm1',  'clz32',
      'cos',    'cosh',    'exp',    'floor',
      'fround', 'hypot',   'imul',   'log',
      'log1p',  'log2',    'log10',  'max',
      'min',    'pow',     'random', 'round',
      'sign',   'sin',     'sinh',   'sqrt',
      'tan',    'tanh',    'trunc',  'E',
      'LN10',   'LN2',     'LOG10E', 'LOG2E',
      'PI',     'SQRT1_2', 'SQRT2'
    ]



Do we have structured data?


```javascript
const l = [1,2,3]
l
```


    evalmachine.<anonymous>:1

    const l = [1,2,3]

    ^

    

    SyntaxError: Identifier 'l' has already been declared

        at Script.runInThisContext (node:vm:129:12)

        at Object.runInThisContext (node:vm:307:38)

        at run ([eval]:1020:15)

        at onRunRequest ([eval]:864:18)

        at onMessage ([eval]:828:13)

        at process.emit (node:events:513:28)

        at emit (node:internal/child_process:944:14)

        at process.processTicksAndRejections (node:internal/process/task_queues:83:21)


Can we define functions?


```javascript
function pick_day() {
    return Math.round(Math.random()*364)+1
}
pick_day()
```




    16



Do we have comprehensions?

* In Javascript they are called _maps_:

```javascript
> [1,2,3].map(pick_day)
[337, 334, 195]
````

## Range?

* Ok, do we have `range`?

> [...Array(10)]
[
  undefined, undefined,
  undefined, undefined,
  undefined, undefined,
  undefined, undefined,
  undefined, undefined
]
```

## A list of birthdays!


```javascript
[...Array(27)].map(pick_day)
```




    [
      103,  92, 339, 331, 273, 111,  76,
      292, 332,  25, 282, 231, 248,  42,
      281, 364,  28, 159, 162, 304, 125,
      275, 273, 324,  32,  92, 295
    ]



## Spread operator

* In `[...Array(27)].map(pick_day)`, the `...` is the _spread operator_
* It unpacks elements from arrays or properties from objects into distinct variables
* Here, Array(27) creates an array of 27 empty slots. Spreading it using ... gives you an array of 27 undefined values
* But don't mind, you can consider it black magic...

Can we spot repeated numbers? Yes, with `Set`!


```javascript
var dates = [...Array(27)].map(pick_day)
var s = (new Set(dates))

// are there duplicates?

s.size == dates.length
```




    false



## What's `new`?

* `Set` is a special function, called a _constructor_
* _constructors_ allow to create new objects of a certain type
* hence `new Set([...])`

## anonymous functions

* there's another way to define functions in javascript:

* `const pick_day = () => Math.round(Math.random()*364)+1`
* this is called an _anonymous function_ (or _lambda function_)
* useful for throwaway, use only once functions
* for instance, as arguments for `map`

# Helpers

* let's define some helper functions


```javascript
function create_class(n) {
    return [...Array(n)].map(pick_day)
}

function are_there_dups(vals) { 
    return !(vals.length == (new Set(vals)).size)
}
```


```javascript
people = create_class(27)
console.log(people)
are_there_dups(people)
```

    [
      340, 271,  35, 358, 353, 177, 344,
      355, 192,  37, 150, 202, 263, 232,
      192,  11, 355, 193, 129, 363, 358,
      218,  21, 219, 242,   7,  16
    ]





    true



# Experiments

* let's perform some experiments!


```javascript
[...Array(1000)].map(() => (are_there_dups(create_class(27))))
```




    [
      true,  true, true,  true,  true,  false, false, true,  true,
      true,  true, true,  true,  true,  true,  true,  true,  true,
      true,  true, true,  true,  false, false, false, true,  true,
      true,  true, false, true,  true,  true,  true,  true,  true,
      true,  true, true,  true,  false, false, false, true,  false,
      false, true, true,  true,  false, false, true,  true,  true,
      false, true, false, false, true,  false, true,  false, false,
      false, true, false, false, false, true,  false, false, true,
      false, true, false, true,  true,  true,  true,  true,  true,
      true,  true, true,  false, true,  false, false, true,  false,
      true,  true, true,  true,  true,  false, true,  true,  false,
      true,
      ... 900 more items
    ]



## Trues only

* Ok, how do we count the occurrences of `true`in the experiments?
* We can use the `filter` method of lists
* `filter`takes a _predicate_, that is a function that takes an item and returns a boolean value
* The result is a list with only the values for which the predicate is true



```javascript
[1,2,3,4,5,6,7,8,9,10].filter((x) => x < 5)
```




    [ 1, 2, 3, 4 ]



So here we go:


```javascript
var experiments = [...Array(1000)].map(() => are_there_dups(create_class(27)))
experiments.filter((x) => x == true).length
```




    616


