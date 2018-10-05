## Interpolated String Literals

> These are also known as Template Strings, which is misleading since it is only used once and therefore not a template

### String Interpolation: Imperative

```js
var name = "Kyle Simpson";
var email = "gitify@gmail.com";
var title = "Teacher";

var msg = "Welcome to this class! Your " +
    title + " is " + name + ", contact: " +
    email + ".";

// Welcome to this class! Your teacher is Kyle Simpson, contact: getify@gmail.com.
```

### String Interpolation: Declarative

```js
var name = "Kyle Simpson";
var email = "gitify@gmail.com";
var title = "Teacher";

var msg = `Welcome to this class! Your ${title} is ${name}, contact: ${email}.`;

// Welcome to this class! Your teacher is Kyle Simpson, contact: getify@gmail.com.
```

> A multi-line string is a string that contains multiple lines.
>
> A string that is defined across multiple lines in your code is not a multi-line string; it is called a line continuation.

```js
var name = "Kyle Simpson";
var email = "gitify@gmail.com";
var title = "Teacher";

var msg = "Welcome to this class! Your \n" +
    title + " is " + name + ", contact: " +
    email + ".";

// Welcome to this class! Your 
// teacher is Kyle Simpson, contact: getify@gmail.com.
```

```js
var name = "Kyle Simpson",
var email = "gitify@gmail.com";
var title = "Teacher";

var msg = `Welcome to this class! Your
${title} is ${name}, contact: ${email}.`;

// Welcome to this class! Your
// teacher is Kyle Simpson, contact: getify@gmail.com.
```

> Don't use a find-and-replace all of your quotes to use the backticks, because certain things need to be regular strings

### String Interpolation: Tagged

```js
var amount = 12.3;

var msg =
    formatCurrency
`The total for your order is ${amount}`;

// The total for your
// order is $12.30
```

> The tag function is called first (ex. formatCurrency) and this is processed first
>
> Formatting functions are generally used

```js
function formatCurrency(strings,...values) {
    var str = "";
    for (let i = 0; i < strings.length; i++) {
        if (i > 0) {
            if (typeof values[i-1] == "number") {
                str += `$${values[i-1].toFixed(2)}`;
            }
            else {
                str += values[i-1];
            }
        }
        str += strings[i];
    }
    return str;
}

```

### Exercise 3

Convert the following code to using tagged string interpolation, resulting in a 'true' condition

```js
function upper(strings,...values) {}

var name = "kyle",
    twitter = "getify",
    classname = "es6 workshop";

console.log(
    `Hello ____ (@____), welcome to the ____!` ===
    "Hello KYLE (@GETIFY), welcome to the ES6 WORKSHOP!"
);
```

Solution

```js
function upper(strings,...values) {
    var str = "";
    for (let i = 0; i < strings.length; i++) {
        if (i > 0) {
            str += `${values[i-1].toUpperCase()}`;
        }
        str += strings[i];
    }
    return str;
}

var name = "kyle",
    twitter = "getify",
    classname = "es6 workshop";

console.log(
    upper
    `Hello ${name} (@${twitter}), welcome to the ${classname}!` ===
    "Hello KYLE (@GETIFY), welcome to the ES6 WORKSHOP!"
);
```

### Example: Logger

https://gist.github.com/getify/d8e5349d7cf654c313ff623f0506b8d8

```js
function logger(strings,...values) {
    var str = "";
    for (let i = 0; i < strings.length; i++) {
        if (i > 0) {
            if (values[i-1] && typeof values[i-1] == "object") {
                if (values[i-1] instanceof Error) {
                    if (values[i-1].stack) {
                        str += values[i-1].stack;
                        continue;
                    }
                }
                else {
                    try {
                        str += JSON.stringify(values[i-1]);
                        continue;
                    }
                    catch (err) {}
                }
            }
            str += values[i-1];
        }
        str += strings[i];
    }
    console.log(str);
    return str;
}

var v = 42;
var o = { a: 1, b: [2,3,4] };

logger`This is my value: ${v} and another: ${o}`;
// This is my value: 42 and another: {"a":1,"b":[2,3,4]}
```