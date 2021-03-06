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

#### Example 16 Associative

```js
var x = 1;
var y = 2;
var z = 3;

x = null;
y = null;
z = null;
```

```js
var x = 1;
var y = 2;
var z = 3;

x = y = z = null;
// x = null
// y = null
// z = null
```

Order of operations: 
```js
x + y * z
// equivalent to:
x + (y * z)

x = y = z = null;
// equivalent to:
x = (y = (z = null));
```

Completion results:
![completion result](images/1.jpg)

Note that the completion value of `var x = 1;` is undefined, while the completion value of `x = x + 3;` is the result of 4.

```js
function foo() {
    return [1,2,3];
}
var tmp = foo();
var a = foo[0];
var b = foo[1];
var c = foo[2];
// tmp = [1,2,3]
// a = 1
// b = 2
// c = 3
```

```js
function foo() {
    return [1,2,3];
}
var a,b,c, tmp;
a = (tmp = foo())[0];
// tmp = [1,2,3]
// a = [1]
```

```js
function foo() {
    return [1,2,3];
}

var a, b, c, tmp;
[a,b,c] = tmp = foo();
// a = 1
// b = 2
// c = 3
// tmp = [1,2,3]
```

```js
function foo() {
    return [1,2,3];
}

var a, b, c, tmp;
tmp = [a,b,c] foo();
// tmp = [1,2,3]
// a = 1
// b = 2
```

```js
function foo() {
    return [1,2,[3,4,5]];
}
var tmp = foo() || [];
var a = foo[0];
var b = foo[1];
var c = foo[2];
```

```js
function foo() {
    return [1,2,[3,4,5]];
}

var [
    a,
    b,
    c
] = foo() | [];
```

```js
function foo() {
    return [1,2,[3,4,5]];
}
var tmp = foo() || [];
var a = foo[0];
var b = foo[1];
var tmp2 = tmp[2];
var c = tmp2[0];
var d = tmp2[1];
```

```js
function foo() {
    return [1,2,[3,4,5]];
}

var [
    a,
    b,
    [
        c,
        d
    ]
] = foo() | [];
```

#### Example

```js
function foo() {
    return [1,2];
}
var tmp = foo() || [];
var a = foo[0]; // 1
var b = foo[1]; // 2
var tmp2 = tmp[2]; // undefined
var c = tmp2[0]; // Error
var d = tmp2[1]; // Error
```

```js
function foo() {
    return [1,2];
}
var tmp = foo() || [];
var a = foo[0];
var b = foo[1];
var tmp2 = tmp[2] !== undefined ? tmp[2] : [];
var c = tmp2[0]; // undefined
var d = tmp2[1]; // undefined
```

```js
function foo() {
    return [1,2];
}

var [
    a,
    b,
    [
        c,
        d
    ] = []
] = foo() | [];
// a = 1
// b = 2
// c = undefined
// d = undefined
```

#### Example

```js
function foo(x) {
    // x = 42
}

foo(42);
```

```js
function foo(tmp) {
    var x = tmp[0]; // x = 3
    var y = tmp[1]; // y = 4
}

foo([3,4]);
```

```js
function foo([x,y]) {

}
foo([3,4]);
```

```js
function foo([x,y]) {

}
foo(); // Error
```

```js
function foo([x = 1,y = 2]) {

}
foo();
```

```js
function foo(tmp) {
    var tmp = tmp !== undefined ? tmp : [];
    var x = tmp[0]; // undefined
    var y = tmp[1]; // undefined
}
foo([]);
```

```js
function foo([x,y] = [2,3]) {
    // x = undefined
    // y = undefined
}
foo([]);
```

```js
function foo(tmp) {
    var tmp = tmp !== undefined ? tmp : [];
    var x = tmp[0] !== undefined ? tmp[0] : 2; // x = 2
    var y = tmp[1] !== undefined ? tmp[1] : 3; // y = 3
}
foo([]);
```

```js
function foo([x = 2,y = 3]) {
    // x = 2
    // y = 3
}
foo([]);
```

> Use a linter (ex. ESLint) to make sure you remember to set the default for gracefully handle errors

### Object Destructuring

```js
function foo() {
    return { a: 1, b: 2, c: 3};
}

var tmp = foo() | {};
var a = tmp.a;
var b = tmp.b;
var c = tmp.c;
```

```js
function foo() {
    return { a: 1, b: 2, c: 3};
}

var {
    a: a,
    b: b,
    c: c
} = foo() || {};
```

Order doesn't matter
```js
function foo() {
    return { a: 1, b: 2, c: 3};
}

var {
    b: b,
    a: a,
    c: c
} = foo() || {};
```

Shorthand for if the source and target are the same
```js
function foo() {
    return { a: 1, b: 2, c: 3};
}

var {
    a,
    b,
    c
} = foo() || {};
```

#### Example

```js
var o = {
    prop: val,
    target: source
};

var {
    source: target
} = o;
```

The property name always shows up on the left
```js
var o = {
    prop: val,
    target: source
};

var {
    prop: val,
    source: target
} = o;
```

```js
var o = {
    prop: val
};

var {
    prop: val
} = o;
```

#### Example

```js
function foo() {
    return { a: 1, b: 2};
}

var tmp = foo() | {};
var a = tmp.a;
var b = tmp.b;
var c = tmp.c; // undefined
```

```js
function foo() {
    return { a: 1, b: 2};
}

var {
    a,
    b,
    c
} = foo() || {};
// c = undefined
```

#### Example: setting defaults

```js
function foo() {
    return { a: 1, b: 2};
}

var tmp = foo() | {};
var a = tmp.a;
var b = tmp.b;
var c = tmp.c !== undefined ? tmp.c : 3;
```

```js
function foo() {
    return { a: 1, b: 2};
}

var {
    a,
    b,
    c: c = 3
} = foo() || {};
```

#### Example: setting defaults

```js
function foo() {
    return { a: 1, b: 2};
}

var tmp = foo() | {};
var a = tmp.a;
var b = tmp.b;
var CCC = tmp.c !== undefined ? tmp.c : 3;
```

```js
function foo() {
    return { a: 1, b: 2};
}

var {
    a,
    b,
    c: CCC = 3 // do we have a c property, if it is undefined set CCC to 3, if c is present set CCC to c
} = foo() || {};
```

#### Example: unknown properties

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4};
}

var tmp = foo() | {};
var a = tmp.a;
var b = tmp.b;
var other = {
    c: tmp.c,
    d: tmp.d
}
```

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4};
}

var {
    a,
    b,
    ...other
} = foo() || {};
```

### Object Spread

```js
var o = { x: 1, y: 2, z: 3 };

var p = { ...o, w: 4};
// w = 4
```

```js
var o = { x: 1, y: 2, z: 3, w: 6 };

var p = { w: 4, ...o };
// w = 6
```

### Shallow copying

```js
var x = [...y];
var x = {...y};
```

#### Example

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4 };
}

var o = {};

var tmp = foo() || {};
o.x = tmp.a;
o.b = tmp.b;
o.CCC = tmp.c;
```

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4 };
}

var o = {};

{
    a: o.x, // Syntax Error due to the brackets making it a block statement
    b: o.b,
    c: o.CCC
} = foo() | {};
```

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4 };
}

var o = {};

var{
    a: x,
    b: b,
    c: CCC
} = foo() | {};
```

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4 };
}

var o = {};

({
    a: o.x,
    b: o.b,
    c: o.CCC
} = foo() | {});
```

#### Example

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4 };
}

var o = {};

var tmp = foo() || {};
o.x = tmp.a;
o.b = tmp.b;
o.w = tmp.b;
o.CCC = tmp.c;
```

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4 };
}

var o = {};

({
    a: o.x,
    b: o.b,
    b: o.w,
    c: o.CCC
} = foo() | {});
```

#### Example

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4 };
}

var o = {};

({
    a: o.x,
    b: o.b = 2,
    b: o.w = 3,
    c: o.CCC
} = foo() | {});
```

#### Example

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4 };
}

function bar() { return }

var o = {};

({
    a: o.x,
    b: o.b = 2,
    b: o.w = bar(), // can use a valid expression
    c: o.CCC
} = foo() | {});
```

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4 };
}

var o = {};

({
    a: o.x,
    b: o.b = 2,
    b: o.w = bar(b), // Error, since bar doesn't know where b is coming from
    c: o.CCC
} = foo() | {});
```

#### Example

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4 };
}

var o = {};
var OBJ;

OBJ = {
    a: o.x,
    b: o.b = 2,
    b: o.w = 3,
    c: o.CCC
} = foo() | {};
```

```js
function foo() {
    return { a: 1, b: 2, c: 3, d: 4 };
}

var o = {};

var OBJ = {
    a: o.x,
    b: o.b = 2,
    b: o.w = 3,
    c: o.CCC
} = foo() | {};
```


#### Example

```js
function foo() {
    return {
        a: 1,
        b: {
            c: 3,
            d: 4
        },
        e: 5
    }
}

var tmp = foo() || {};
var tmp2 = tmp.b;
var c = tmp2.c;
var d = tmp2.d;
```

```js
function foo() {
    return {
        a: 1,
        b: {
            c: 3,
            d: 4
        },
        e: 5
    }
}

var {
    b: {
        c,
        d
    }
} = foo() | {};
```

```js
function foo() {
    return {
        a: 1,
        b: {
            c: 3,
            d: 4
        },
        e: 5
    }
}

var {
    b,
    b: {
        c,
        d
    }
} = foo() | {};
```

#### Example

```js
function foo() {
    return {
        a: 1,
        e: 5
    }
}

var tmp = foo() || {};
var tmp2 = tmp.b; // undefined
var c = tmp2.c; // Error
var d = tmp2.d;
```

```js
function foo() {
    return {
        a: 1,
        e: 5
    }
}

var {
    b,
    b: {
        c, // Error
        d
    }
} = foo() | {};
```

#### Example

```js
function foo() {
    return {
        a: 1,
        e: 5
    }
}

var tmp = foo() || {};
var tmp2 = tmp.b !== undefined ? tmp.b || {};;
var c = tmp2.c; // undefined
var d = tmp2.d;
```

```js
function foo() {
    return {
        a: 1,
        e: 5
    }
}

var {
    b,
    b: {
        c,
        d
    } = {}
} = foo() | {};
```

#### Example

```js
function foo({ x } = {}) {
    // ..
}

foo({ x: 3 });
```

#### Example (named arguments)

```js
function foo(x,y,z,w,r,e,f,g) {
    // ..
}

foo(@r = 3); // Named arguments feature not in JavaScript
```

```js
function foo({ x,y,z,w,r,e,f,g } = {}) {
    // ..
}

foo({ w: 3, y: 2 });
```

#### Example (passing booleans)

```js
function foo({ x,y,z,w,r,e,f,g,updateStatus } = {}) {
    // ..
}

foo({ w: 3, y: 2 });

bar(/*updateStatus=*/true);
```

>You still need to remember the names of the object keys, but you can decide on a common naming scheme like:
>
>cb for callbacks
>
>v for values
>
>etc.