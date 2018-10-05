### Destructuring and Restructuring

```js
// most common approach, using extend(..)

var defaults = {
  url: "http://some.base.url/api",
  method: "post",
  headers: [
    "Content-Type: text/plain"
  ]
};

console.log(defaults);

// ************************

var settings = {
  url: "http://some.other.url/",
  data: 42,
  callback: function(resp) { /* .. */ }
};

// underscore extend(..)
ajax( _.extend({},defaults,settings) );

// or: ajax( Object.assign({},defaults,settings) );
```

```js
// instead, IMO better using destructuring and defaults

var defaults = ajaxOptions();  // with no arguments, returns the defaults as an object if necessary

console.log(defaults);

// ************************

var settings = {
  url: "http://some.other.url/",
  data: 42,
  callback: function(resp) { /* .. */ }
};

ajax( ajaxOptions( settings ) );  // with an argument, mixes in the settings w/ the defaults

// ************************

function ajaxOptions({
   url = "http://some.base.url/api",
   method = "post",
   data,
   callback,
   headers: [
     headers0 = "Content-Type: text/plain",
     ...otherHeaders
   ] = []
} = {}) {
   return {
     url, method, data, callback,
     headers: [
       headers0,
       ...otherHeaders
     ]
   };
}
```

Using gathering (has less documentation):

```js
// instead, IMO better using destructuring and defaults

var defaults = ajaxOptions();  // with no arguments, returns the defaults as an object if necessary

console.log(defaults);

// ************************

var settings = {
  url: "http://some.other.url/",
  data: 42,
  callback: function(resp) { /* .. */ }
};

ajax( ajaxOptions( settings ) );  // with an argument, mixes in the settings w/ the defaults

// ************************

function ajaxOptions({
   url = "http://some.base.url/api",
   method = "post",
   headers: [
     headers0 = "Content-Type: text/plain",
     ...otherHeaders
   ] = [],
   ...other
} = {}) {
   return {
     url, method, data, callback,
     headers: [
       headers0,
       ...otherHeaders
     ],
     ...other
   };
}
```

#### Example

```js
var str = "Hello world";

var re = /l(l.)/;
var x = str.match(re); // ["llo","lo"]

```

```js
var str = "Hello world";

var re = /l(l.)/;
var [,word] = str.match(re); // word = "lo"

```

### Exercise 2

#### Fix the following code to get it to return true

```js
var defaults = {
    foo: 0,
    bar: 4,
    bam: {
        qux: 0,
        qam: 14
    }
};

ajax("http://fun.tld",handleResponse);


// *******************************************************

function handleResponse(/* destructuring here */) {
    checkData({
        /* restructuring here */
    });
}

function ajax(url,cb) {
    // fake ajax response:
    cb({
        foo: 2,
        baz: [ 6, 8, 10 ],
        bam: {
            qux: 12
        }
    });
}

function checkData(data) {
    console.log(
        56 === (
            data.foo +
            data.bar +
            data.baz[0] + data.baz[1] + data.baz[2] +
            data.bam.qux +
            data.bam.qam
        )
    );
}

```

#### Solution

```js
var defaults = {
    foo: 0,
    bar: 4,
    bam: {
        qux: 0,
        qam: 14
    }
};

ajax("http://fun.tld",handleResponse);


// *******************************************************

function handleResponse({
    foo = 0,
    bar = 4,
    baz,
    bam: {
        qux = 0,
        qam = 14
    } = {}
} = {}) {

    checkData({
        foo,
        bar,
        baz,
        bam: {
            qux,
            qam
        }
    });
}

function ajax(url,cb) {
    // fake ajax response:
    cb({
        foo: 2,
        baz: [ 6, 8, 10 ],
        bam: {
            qux: 12
        }
    });
}

function checkData(data) {
    console.log(data);
    console.log(
        56 === (
            data.foo +
            data.bar +
            data.baz[0] + data.baz[1] + data.baz[2] +
            data.bam.qux +
            data.bam.qam
        )
    );
}
```

#### Ideal way of doing defaults

```js
function ajaxOptions({
    foo = 0,
    bar = 4,
    baz,
    bam: {
        qux = 0,
        qam = 14
    } = {}
} = {}) {
    // ..
}
```