# JavaScript: The Recent Parts (ES6)

Training by: Kyle Simpson (gitify@gmail.com)

Thursday October 4, 2018

-------

## Rest/Spread Operator

> Imperative: code that focuses on how to do something (the default way that people are taught)
>
> Declarative: code that focuses on the outcome and the what and why (the ideal way of doing code)

### Rest: imperative

```js
function lookupRecord(id) {
    var otherParams = [].slice.call( arguments, 1 );

    // ...
}
```

### Spread: imperative

```js
function lookupRecord(id) {
    var otherParams = [].slice.call( arguments, 1 );
    otherParams.unshift(
        "people-records", id.toUpperCase()
    );
    return db.lookup.apply( null, otherParams );
}
```

### Rest (aka "gather"): declarative

This gathers up items into an array

```js
function lookupRecord(id,...otherParams) {
    // ..
}
```

### Spread: declarative

This does the same thing as the imperative spread

```js
function lookupRecord(id,...otherParams) {
    return db.lookup(
        "people-records", id, ...otherParams
    );
}
```

> If in an assignment context, ... gathers.
>
> If in a value list, ... spreads

### More examples

#### Example 1

```js
var a = [1,2,3];
var b = [4,5,6];
var c = [0].concat(a,b,[7]); // c = [0, 1, 2, 3, 4, 5, 6, 7]
```

```js
var a = [1,2,3];
var b = [4,5,6];
var c = [0,...a,...b,7]); // c = [0, 1, 2, 3, 4, 5, 6, 7]
```

#### Example 2

```js
function foo(a,b) {
    var c = [0].concat(a,b,[7])
}
```

```js
function foo(a,b,...c) { // spreads

}
```

#### Example 3

```js
var a = [1,2,3];
var b = [4,5,6];
var c = [0,...a,...b,7]); // c = [0, 1, 2, 3, 4, 5, 6, 7]

function foo(a,b,...c) { // spreads
    console.log(a,b,c);
}
foo(...a); // gathers
```

#### Example 4

An spread operator on nothing returns an empty array

```js
var a = [1,2,3];
var b = [4,5,6];
var c = [0,...a,...b,7]); // c = [0, 1, 2, 3, 4, 5, 6, 7]

function foo(x,y,z) {
    console.log(x,y,z); // [123], [456], []
    // x = [123],
    // y = [456],
    // z = []
}
foo(a,b);
```

#### Example 5

```js
var a = [1,2,3];
var b = [4,5,6];
var c = [0,...a,...b,7]); // c = [0, 1, 2, 3, 4, 5, 6, 7]

function foo(x,y,...z) {
    console.log(x,y,z);  // 0, 1, [2, 3, 4, 5, 6, 7]
    // x = 0
    // y = 1
    // z = [2, 3, 4, 5, 6, 7]
}
foo(...c);
```

### Exercise 1

Make the following code return true:

```js
function foo() { }

function bar() {
    var a1 = [ 2, 4 ];
    var a2 = [ 6, 8, 10, 12 ];

    return foo(); // foo()
}

console.log(
    bar().join("") === "281012" // false
);
```

My solution:

```js
function foo(x, y, z, ...other) {
    // x = 2
    // y = 4
    // z = 6
    // other = [8, 10, 12]
    return [x, ...other] // [2, 8, 10, 12]
}

function bar() {
    var a1 = [ 2, 4 ];
    var a2 = [ 6, 8, 10, 12 ];

    return foo(...a1, ...a2); // foo(2, 4, 6, 8, 10, 12)
}

console.log(
    bar().join("") === "281012" // true
);
```

How this can be used:

Always remove the first 2 items

```js
function dump2(x, y, ...other) {
    // x = 1
    // y = 2
    // other = [3, 4, 5]
    return other;
}

var a = [1, 2, 3, 4, 5]
var b = dump2(...a);
console.log(b); // [3, 4, 5]
```

> Using the arrow functions doesn't always mean that it's better, and is more readable
>
> Imperative
>
>```js
>[1, 2, 3, 4, 5].map(x => x * 2);
>```
>
> Declarative
>
>```js
>[1, 2, 3, 4, 5].map(function doubleIt(x) {
>    return x * 2;
>})
>```
