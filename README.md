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

    // ..
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

#### Example 1 (equivalent code blocks)

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

#### Example 2 (equivalent code blocks)

```js
function foo(a,b) {
    return [0].concat(a,b,[7])
}

var a = [1,2,3];
var b = [4,5,6];
var c = foo(a, b); // c = [0, 1, 2, 3, 4, 5, 6, 7]

```

```js
function foo(a,b,...c) { // spreads
    // a = 1
    // b = 2
    // c = [3]
    return [a,b,...c];
}

var a = [1,2,3];
var b = [4,5,6];
var c = [0,...a,...b,7];
// c = [0, 1, 2, 3, 4, 5, 6, 7]

var d = foo(...a); // gathers
// d = [1, 2, 3]
```

#### Example 3

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

#### Example 4

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

#### Make the following code return true:

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

#### Solution:

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

#### How this can be used:

Function to always remove the first 2 items

```js
function dump2(x, y, ...other) {
    // x = 1
    // y = 2
    // other = [3, 4, 5]
    return other;
}

var a = [1, 2, 3, 4, 5]
var b = dump2(...a);
// b = [3, 4, 5]
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

## Destructuring

> _Decomposing_ a _structure_ into its individual parts

### Destructuring: imperative

```js
var tmp = getSomeRecords();

var first = tmp[0];
var second = tmp[1];

var firstName = first.name;
var firstEmail = first.emal !== undefined ?
    first.email :
    "nobody@none.tld";

var secondName = second.name;
var secondEmail = second.email !== undefined ?
    second.email :
    "nobody@none.tld";
```

### Destructuring: declarative

```js
var [
    {
        name: firstName,
        email: firstEmail = "nobody@none.tld"
    },
    {
        name: secondName,
        email: secondEmail = "nobody@none.tld"
    }
] = getSomeRecords();
```

> The business logic and function calls don't belong in the destructuring

### Examples

#### Example 1

```js
function foo() {
    return [1,2,3];
}

var tmp = foo();
var a = tmp[0]; // 1
var b = tmp[1]; // 2
var c = tmp[2]; // 3
```

```js
function foo() {
    return [1,2,3];
}

var [
    a,
    b,
    c
] = foo();
// a = 1
// b = 2
// c = 3
```

> It is highly recommended that you split the destructuring into multiple lines to make it more readable.
>
> Only use 1 line if there are only 1 or 2 elements

#### Example 2

```js
function foo() {
    return [1,2];
}

var tmp = foo();
var a = tmp[0]; // 1
var b = tmp[1]; // 2
var c = tmp[2]; // undefined
```

```js
function foo() {
    return [1,2];
}

var [
    a,
    b,
    c
] = foo();
// a = 1
// b = 2
// c = undefined
```

#### Example 3

> Only need to destructure the parts that you care about

```js
function foo() {
    return [1,2,3,4];
}

var tmp = foo();
var a = tmp[0]; // 1
var b = tmp[1]; // 2
var c = tmp[2]; // 3
```

```js
function foo() {
    return [1,2,3,4];
}

var [
    a,
    b,
    c
] = foo();
// a = 1
// b = 2
// c = 3
```

#### Example 4

```js
function foo() {
    return;
}

var tmp = foo();
var a = tmp[0]; // Error
var b = tmp[1]; // Error
var c = tmp[2]; // Error
```

```js
function foo() {
    return;
}

var [
    a,
    b,
    c
] = foo(); // Error
```

#### Example 5

```js
function foo() {
    return;
}

var tmp = foo() || [];
var a = tmp[0]; // undefined
var b = tmp[1]; // undefined
var c = tmp[2]; // undefined
```

```js
function foo() {
    return;
}

var [
    a,
    b,
    c
] = foo() || []; // []
// a = undefined
// b = undefined
// c = undefined
```

#### Example 6

```js
function foo() {
    return [1,2,3,4];
}

var tmp = foo() || [];
var a = tmp[1]; // 2
var b = tmp[2]; // 3
var c = tmp[3]; // 4
```

```js
function foo() {
    return [1,2,3,4];
}

var [
    ,
    a,
    b,
    c
] = foo() || [];
// a = 2
// b = 3
// c = 4
```

#### Example 7

```js
function foo() {
    return [1,2,3,4,5,6,7,8];
}

var [
    , // Skip 1
    , // Skip 2
    , // Skip 3
    , // Skip 4
    , // Skip 5
    a,
    b,
    c
] = foo() || [];
// a = 6
// b = 7
// c = 8
```

#### Example 8

```js
function foo() {
    return [1,2,3,4,5,6];
}

var tmp = foo() || [];
var a = tmp[1]; // 1
var b = tmp[2]; // 2
var c = tmp.slice(2); // [3, 4, 5, 6]
```

```js
function foo() {
    return [1,2,3,4,5,6];
}

var [
    a,
    b
    ...c
] = foo() || [];
// a = 1
// b = 2
// c = [3,4,5,6]
```

#### Example 9

Shallow copy

```js
function foo() {
    return [1,2,3,4,5,6];
}

var [
    ...c
] = foo() || [];
// 
```

#### Example 10

```js
function foo() {
    return [1,2];
}

var [
    a,
    b
    ...c
] = foo() || [];
// a = 1
// b = 2
// c = []
```

#### Example 11

```js
function foo() {
    return [1,2];
}

var tmp = foo() || [];
var a = tmp[0]; // 1
var b = tmp[1]; // 2
var c = tmp[2] !== undefined ? tmp[2] : 42; // 42
```

```js
function foo() {
    return [1,2];
}

var [
    a,
    b,
    c = 42
] = foo() || [];
// a = 1
// b = 2
// c = 42
```

#### Example 12

```js
function foo() {
    return [1,2,3];
}

var a,b,c,tmp;

tmp = foo();
a = tmp[0]; // 1
b = tmp[1]; // 2
c = tmp[2]; // 3
```

```js
function foo() {
    return [1,2,3];
}

var a,b,c;

[
    a,
    b,
    c
] = foo();
// a = 1
// b = 2
// c = 3
```

#### Example 13

```js
function foo() {
    return [1,2,3];
}

var tmp;
var o = {};

tmp = foo();
o.a = tmp[0];
o.bbbb = tmp[1];
// o = {a: 1, bbbb: 2}
```

```js
function foo() {
    return [1,2,3];
}

var o = {};

[
    o.a, // 1
    o.bbbb // 2
] = foo();
// o = {a: 1, bbbb: 2}
```

```js
function foo() {
    return [1,2,3];
}

var o = {};
var c;

[
    o.a, // 1
    c // 2
] = foo();
// o = {a: 1}
// c = 2
```

> The [] brackets in the destructuring mean that it is an array pattern, but it is not an actual array

#### Example 14

```js
var x = 1;
var y = 2;

var tmp = x;
x = y; // x = 2
y = tmp; // y = 1
```

```js
var x = 1;
var y = 2;

[y,x] = [x,y];
// x = 2
// y = 1
```

#### Example 15

```js
var x = 1;
var y = 2;

var tmp = [x,y];
x = tmp[1]; // x = 2
y = tmp[2]; // y = 1
```

```js
var x = 1;
var y = 2;

[y,x] = [x,y];
// x = 2
// y = 1
```