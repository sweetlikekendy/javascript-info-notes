# Javascript.info Notes

My notes from the [javascript.info](https://javascript.info) website.

**Table of Contents**

- [Javascript.info Notes](#javascriptinfo-notes)
- [**Objects**](#objects)
  - [**Object references and copying**](#object-references-and-copying)
  - [**Garbage Collection**](#garbage-collection)
  - [**Object methods, this**](#object-methods-this)
  - [**Constructor, operator "new"**](#constructor-operator-new)
  - [**Optional chaining '?.'**](#optional-chaining-)
  - [**Summary**](#summary)
  - [**Symbol Type**](#symbol-type)
  - [**Object to primitive conversion**](#object-to-primitive-conversion)
  - [**ToPrimitive**](#toprimitive)
- [**Data types**](#data-types)
  - [**Primative Methods**](#primative-methods)
  - [**Numbers**](#numbers)
  - [**toString(base)**](#tostringbase)
  - [**Rounding**](#rounding)
  - [**Imprecise Calculations**](#imprecise-calculations)
  - [**parseInt and parseFloat**](#parseint-and-parsefloat)
  - [**Other math functions**](#other-math-functions)
  - [**Strings**](#strings)
  - [**Special Characters**](#special-characters)
  - [**Accessing characters**](#accessing-characters)
  - [**Strings are immutable**](#strings-are-immutable)
  - [**Searching for a substring**](#searching-for-a-substring)
  - [**The bitwise NOT trick**](#the-bitwise-not-trick)
  - [**Searching in array**](#searching-in-array)
    - [**includes, startsWith, endsWith**](#includes-startswith-endswith)
    - [**Getting a substring**](#getting-a-substring)
  - [**Comparing strings**](#comparing-strings)
  - [**Summary**](#summary-1)
  - [**Arrays**](#arrays)
  - [**A word about "length"**](#a-word-about-length)
  - [**Array methods**](#array-methods)
  - [**splice**](#splice)
  - [**slice**](#slice)
  - [**concat**](#concat)
  - [**forEach**](#foreach)
  - [**Searching in array**](#searching-in-array-1)
    - [**`indexOf`/`lastIndexOf` and includes**](#indexoflastindexof-and-includes)
    - [**`find` and `findIndex`**](#find-and-findindex)
    - [**`filter`**](#filter)
  - [**Transform an array**](#transform-an-array)
    - [**`map`**](#map)
    - [**`sort(fn)`**](#sortfn)
    - [**`reverse`**](#reverse)
    - [**`split`** and **`join`**](#split-and-join)
    - [**`reduce`**/**`reduceRight`**](#reducereduceright)
    - [**`Array.isArray(value)`**](#arrayisarrayvalue)
    - [**Most methods support "thisArg"**](#most-methods-support-thisarg)
  - [**Iterables**](#iterables)
    - [**Array-likes**](#array-likes)
    - [**Array.from**](#arrayfrom)
  - [**Map and Set**](#map-and-set)
    - [**Map**](#map-1)
      - [**How Map compares keys**](#how-map-compares-keys)
      - [**Chaining**](#chaining)
    - [**Iteration over Map**](#iteration-over-map)
    - [**Object.entries: Map from Object**](#objectentries-map-from-object)
    - [**Object.fromEntries: Object from Map**](#objectfromentries-object-from-map)
    - [**Set**](#set)
  - [**WeakMap and WeakSet**](#weakmap-and-weakset)
    - [**Use case: caching**](#use-case-caching)
  - [**Object.keys, values, entries**](#objectkeys-values-entries)
    - [**Transforming objects**](#transforming-objects)
  - [**Destructuring assignment**](#destructuring-assignment)
    - [**Array Destructuring**](#array-destructuring)
      - [**The rest '...'**](#the-rest-)
      - [**Default values**](#default-values)
    - [**Object destructuring**](#object-destructuring)
      - [**Nested Destructuring**](#nested-destructuring)
      - [**Smart function parameters**](#smart-function-parameters)
    - [**Data and time**](#data-and-time)
      - [`new Date(milliseconds)`](#new-datemilliseconds)
      - [`new Date(datestring)`](#new-datedatestring)
      - [`new Date(year, month, date, hours, minutes, seconds, ms)`](#new-dateyear-month-date-hours-minutes-seconds-ms)
    - [**Access date components**](#access-date-components)
      - [`getFullYear()`](#getfullyear)
      - [`getMonth()`](#getmonth)
      - [`getDate()`](#getdate)
      - [`getHours()`, `getMinutes()`, `getSeconds()`, `getMilliseconds()`](#gethours-getminutes-getseconds-getmilliseconds)
      - [`getDay()`](#getday)
      - [`getTime()`](#gettime)
      - [`getTimezoneOffset()`](#gettimezoneoffset)
    - [**Autocorrection**](#autocorrection)
    - [**Date to number, date diff**](#date-to-number-date-diff)
    - [**Date.now()**](#datenow)
    - [**Benchmarking**](#benchmarking)
    - [**`Date.parse` from a string**](#dateparse-from-a-string)
  - [**Error Handling**](#error-handling)
    - [**Try...catch**](#trycatch)
    - [**Error object**](#error-object)
    - [**Optional "catch" binding**](#optional-catch-binding)
    - [**Error Object**](#error-object-1)
      - [`name`](#name)
      - [`message`](#message)
      - [`stack`](#stack)
    - [**"Throw" operator**](#throw-operator)
    - [**Rethrowing**](#rethrowing)
    - [**try…catch…finally**](#trycatchfinally)
    - [**Global Catch**](#global-catch)
  - [**Custom errors, extending Error**](#custom-errors-extending-error)

# **[Objects](https://javascript.info/object-basics)**

## **[Object references and copying](https://javascript.info/object-copy)**

- Objects are assigned and copied by reference
  ![Object reference example](4-objects/images/references/example.png)
- Object.assign for the so-called “shallow copy”
- Nested objs have to be deeply cloned. Use [`_.cloneDeep(obj)`](https://lodash.com/docs/4.17.15#cloneDeep) from [lodash](https://lodash.com/docs/) lib

## **[Garbage Collection](https://javascript.info/garbage-collection)**

- Garbage collection is performed automatically. We cannot force or prevent it.

- Objects are retained in memory while they are reachable.

```js
function marry(man, woman) {
  woman.husband = man
  man.wife = woman

  return {
    father: man,
    mother: woman,
  }
}

let family = marry(
  {
    name: "John",
  },
  {
    name: "Ann",
  }
)

delete family.father
delete family.mother.husband
```

Deleted reference example

![Deleted references example](4-objects/images/garbage-collection/delete-references-example.png)

Garbage collected

![Garbage collected example](4-objects/images/garbage-collection/garbage-collected-example.png)

- Being referenced is not the same as being reachable (from a root): a pack of interlinked objects can become unreachable as a whole.

## **[Object methods, this](https://javascript.info/object-methods)**

- A method that calls `this` will allow access to the properties of the object in which the method is called

- Arrow functions have no `this`

## **[Constructor, operator "new"](https://javascript.info/constructor-new)**

```js
function User(name) {
  this.name = name
  this.isAdmin = false
}

let user = new User("Jack")

alert(user.name) // Jack
alert(user.isAdmin) // false

// essentially the constructor does this
function User(name) {
  // this = {};  (implicitly)

  // add properties to this
  this.name = name
  this.isAdmin = false

  // return this;  (implicitly)
}
```

- Usually, constructors do not have a return statement. Their task is to write all necessary stuff into this, and it automatically becomes the result.
- If return is called with an object, then the object is returned instead of this.
- If return is called with a primitive, it’s ignored.

## **[Optional chaining '?.'](https://javascript.info/optional-chaining)**

- Example with `value?.prop`
  - works as `value.prop`, if `value` exists,
  - otherwise (when `value` is undefined/null) it returns `undefined`.
- `user?.address.street.name` the `?.` allows `user` to safely be `null/undefined` (and returns `undefined` in that case), but that’s only for `user`

## **Summary**

The optional chaining `?.` syntax has three forms:

- `obj?.prop `– returns `obj.prop` if `obj` exists, otherwise `undefined`.
- `obj?.[prop]` – returns `obj[prop]` if `obj` exists, otherwise `undefined`.
- `obj.method?.()` – calls `obj.method()` if `obj.method` exists, otherwise returns `undefined`.

## **[Symbol Type](https://javascript.info/symbol)**

- `Symbol` is a primitive type for unique identifiers.

- Symbols are created with `Symbol()` call with an optional description (name).

- Symbols are always different values, even if they have the same name. If we want same-named symbols to be equal, then we should use the global registry: `Symbol.for(key)` returns (creates if needed) a global symbol with `key` as the name. Multiple calls of `Symbol.for` with the same `key` return exactly the same symbol.

- Two main use cases:

  1. “Hidden” object properties. If we want to add a property into an object that “belongs” to another script or a library, we can create a symbol and use it as a property key. A symbolic property does not appear in `for..in`, so it won’t be accidentally processed together with other properties. Also it won’t be accessed directly, because another script does not have our symbol. So the property will be protected from accidental use or overwrite.

  2. So we can “covertly” hide something into objects that we need, but others should not see, using symbolic properties. There are many system symbols used by JavaScript which are accessible as `Symbol.\*`. We can use them to alter some built-in behaviors. For instance, later in the tutorial we’ll use `Symbol.iterator` for iterables, `Symbol.toPrimitive` to setup object-to-primitive conversion and so on.

## **[Object to primitive conversion](https://javascript.info/object-toprimitive)**

## **ToPrimitive**

- `string` object-to-string conversion
- `number` obejct-to-number conversion
- `default`

- Conversion algorithm:

  1. Call obj `[Symbol.toPrimitive](hint)` if the method exists,
  2. Otherwise if hint is `"string"` try `obj.toString()` and `obj.valueOf()`, whatever exists.
  3. Otherwise if hint is "number" or "default" try `obj.valueOf()` and `obj.toString()`, whatever exists.

# **[Data types](https://javascript.info/data-types)**

## **[Primative Methods](https://javascript.info/primitives-methods)**

- 7 types of primatives: `string`, `number`, `bigint`, `boolean`, `symbol`, `null`, and `undefined`

## **[Numbers](https://javascript.info/number)**

- Regular JavaScript numbers are stored in 64-bit format, aka "double precision floating point number"

## **toString(base)**

- base=16 is used for hex colors, character encodings etc, digits can be 0..9 or A..F
- base=2 mostly for debugging bitwise operations, digits can be 0 or 1
- base=36 the whole latin alphabet is ued to represent a number.
  - use case: turn a long numeric identifier into something short. Ex: make a short url

```js
alert((12345).toString(36)) // 2n9c
```

## **Rounding**

- `Math.floor`
- `Math.ceil`
- `Math.round`
- `Math.trunc`

|      | Math.floor | Math.ceil | Math.round | Math.trunc |
| ---- | ---------- | --------- | ---------- | ---------- |
| 3.1  | 3          | 4         | 3          | 3          |
| 3.6  | 3          | 4         | 4          | 3          |
| -1.1 | -2         | -1        | -1         | -1         |
| -1.6 | -2         | -1        | -2         | -1         |

## **Imprecise Calculations**

```js
alert(0.1 + 0.2 == 0.3) // false
alert(0.1 + 0.2) // 0.30000000000000004

// fix
let sum = 0.1 + 0.2
alert(sum.toFixed(2)) // 0.30
```

## **parseInt and parseFloat**

```js
alert(+"100px") // NaN

alert(parseInt("100px")) // 100
alert(parseFloat("12.5em")) // 12.5

alert(parseInt("12.3")) // 12, only the integer part is returned
alert(parseFloat("12.3.4")) // 12.3, the second point stops the reading

alert(parseInt("a123")) // NaN, the first symbol stops the process
```

## **Other math functions**

- `Math.random()`, `Math.max(a, b, c, ...)`, `Math.pow(n, power)`

## **[Strings](https://javascript.info/string)**

## **Special Characters**

| Character        | Description                                                                                                                                                                            |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `\n`             | New line                                                                                                                                                                               |
| `\r`             | Carriage return: not used alone. Windows text files use a combination of two characters `\r\n` to represent a line break.                                                              |
| `\'`, `\"`       | Quotes                                                                                                                                                                                 |
| `\\`             | Backslash                                                                                                                                                                              |
| `\t`             | Tab                                                                                                                                                                                    |
| `\b`, `\f`, `\v` | Backspace, Form Feed, Vertical Tab – kept for compatibility, not used nowadays.                                                                                                        |
| `\xXX`           | Unicode character with the given hexadecimal Unicode `XX`, e.g. `'\x7A'` is the same as 'z'.                                                                                           |
| `\uXXXX`         | A Unicode symbol with the hex code `XXXX` in UTF-16 encoding, for instance `\u00A9` – is a Unicode for the copyright symbol ©. It must be exactly 4 hex digits.                        |
| `\u{X…XXXXXX}`   | (1 to 6 hex characters) A Unicode symbol with the given UTF-32 encoding. Some rare characters are encoded with two Unicode symbols, taking 4 bytes. This way we can insert long codes. |

## **Accessing characters**

```js
let str = `Hello`

// the first character
alert(str[0]) // H
alert(str.charAt(0)) // H

// the last character
alert(str[str.length - 1]) // o

alert(str[1000]) // undefined
alert(str.charAt(1000)) // '' (an empty string)

// iterate over string
for (let char of "Hello") {
  alert(char) // H,e,l,l,o (char becomes "H", then "e", then "l" etc)
}
```

## **Strings are immutable**

```js
let str = "Hi"

str[0] = "h" // error
alert(str[0]) // doesn't work
```

## **Searching for a substring**

`str.indexOf(substr, pos)` [.indexOf mdn link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)

- beginning to end

`str.lastIndexOf(substr, position)`

- end to beginning

```js
let str = "Widget with id"

alert(str.indexOf("Widget")) // 0, because 'Widget' is found at the beginning
alert(str.indexOf("widget")) // -1, not found, the search is case-sensitive

alert(str.indexOf("id")) // 1, "id" is found at the position 1 (..idget with id)

alert(str.indexOf("id", 2)) // 12 w'id'get with 'id'

if (str.indexOf("Widget")) {
  alert("We found it") // doesn't work!
}

if (str.indexOf("Widget") != -1) {
  alert("We found it") // works now!
}
```

## **The bitwise NOT trick**

- Converts the number to a 32-bit integer (removes the decimal part if exists) and then reverses all bits in its binary representation

```js
alert(~2) // -3, the same as -(2+1)
alert(~1) // -2, the same as -(1+1)
alert(~0) // -1, the same as -(0+1)
alert(~-1) // 0, the same as -(-1+1)
```

```js
let str = "Widget"

// if (~str.indexOf(...)) reads as “if found”
if (~str.indexOf("Widget")) {
  alert("Found it!") // works
}
```

## **Searching in array**

### **includes, startsWith, endsWith**

- `str.includes(substr, pos)` returns true/false depending on whether `str` contains `substr`
- `str.startsWith` & `str.endsWith`

```js
alert("Widget".startsWith("Wid")) // true, "Widget" starts with "Wid"
alert("Widget".endsWith("get")) // true, "Widget" ends with "get"
```

### **Getting a substring**

3 methods: `substring`, `substr` and `slice`

- `str.slice(start [, end])` returns the part of the string from `start` to (but not including) `end`. **it is enough to just remember this one**

```js
let str = "stringify"
alert(str.slice(0, 5)) // 'strin', the substring from 0 to 5 (not including 5)
alert(str.slice(0, 1)) // 's', from 0 to 1, but not including 1, so only character at 0

// without first arg
alert(str.slice(2)) // 'ringify', from the 2nd position till the end

// start at the 4th position from the right, end at the 1st from the right
alert(str.slice(-4, -1)) // 'gif'
```

- `str.substring(start [,end])` 4eturns the part of the string between `start` and `end`. _negative args are not supported_

```js
let str = "stringify"

// these are same for substring
alert(str.substring(2, 6)) // "ring"
alert(str.substring(6, 2)) // "ring"

// ...but not for slice:
alert(str.slice(2, 6)) // "ring" (the same)
alert(str.slice(6, 2)) // "" (an empty string)
```

- `str.substr(start, [,length])` returns the part of the string from `start`, with the given `length`

```js
let str = "stringify"
alert(str.substr(2, 4)) // 'ring', from the 2nd position get 4 characters
alert(str.substr(-4, 2)) // 'gi', from the 4th position get 2 characters
```

## **Comparing strings**

- Lowercase > Uppercase
- Letters w/ diacriticial marks are "out of order"

```js
alert("Österreich" > "Zealand") // true
```

- `str.localeCompare(str2[, locales[, options]] )` [mdn link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare) returns an integer indicating whether str is less, equal or greater than str2 according to the language rules:

  - negative number if str is less than str2.
  - positive number if str is greater than str2.
  - 0 if they are equivalent.

## **Summary**

There are several other helpful methods in strings:

- `str.trim()` – removes (“trims”) spaces from the beginning and end of the string.
- `str.repeat(n)` – repeats the string n times.
- …and more to be found in the [manual](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String).

## **[Arrays](https://javascript.info/array)**

![Push/pop fast, Shift/unshift slow](5-data_types/images/arrays/push_pop_shift_unshift.png)

- `push(...items)` adds `items` to the end.
- `pop()` removes the element from the end and returns it.
- `shift()` removes the element from the beginning and returns it.
- `unshift(...items)` adds `items` to the beginning.
- loops an array without index `for (let item of arr)`

## **A word about "length"**

```js
let arr = [1, 2, 3, 4, 5]

arr.length = 2 // truncate to 2 elements
alert(arr) // [1, 2]

arr.length = 5 // return length back
alert(arr[3]) // undefined: the values do not return

arr.length = 0 // simple way to clear an array
```

## **[Array methods](https://javascript.info/array-methods)**

## **[splice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)**

`arr.splice(start[, deleteCount, elem1, ..., elemN])` returns elements removed

```js
// REMOVE AND REPLACE ==============================================

let arr = ["I", "study", "JavaScript", "right", "now"]

// remove 3 first elements and replace them with another
arr.splice(0, 3, "Let's", "dance")

alert(arr) // now ["Let's", "dance", "right", "now"]

// DELETE AND RETURN ===============================================

let arr = ["I", "study", "JavaScript", "right", "now"]

// remove 2 first elements
let removed = arr1.splice(0, 2)

alert(removed) // "I", "study" <-- array of removed elements

// INSERTING =======================================================
let arr = ["I", "study", "JavaScript"]

// from index 2
// delete 0
// then insert "complex" and "language"
arr.splice(2, 0, "complex", "language")

alert(arr) // "I", "study", "complex", "language", "JavaScript"

// NEGATIVE INDEX ALLOWED ==========================================

let arr = [1, 2, 5]

// from index -1 (one step from the end)
// delete 0 elements,
// then insert 3 and 4
arr.splice(-1, 0, 3, 4)

alert(arr) // 1,2,3,4,5
```

## **[slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)**

`arr.slice([start], [end])` returns a new array copying to it all items from index `start` to `end` (not including `end`). Both `start` and `end` can be negative, in that case position from array end is assumed.

```js
let arr = ["t", "e", "s", "t"]

alert(arr.slice(1, 3)) // e,s (copy from 1 to 3)

alert(arr.slice(-2)) // s,t (copy from -2 till the end)
```

## **[concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)**

`arr.concat(arg1, arg2...)` returns a new array that includes values from other arrays and additional items.

If `argN` is an array, all elements are copied. Otherwise, the argument itself is copied.

## **[forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/foreach)**

```js
arr.forEach(function (item, index, array) {
  // ... do something with item
})
```

Allows to run a function for every element of the array.

## **Searching in array**

### **`indexOf`/`lastIndexOf` and includes**

```js
let arr = [1, 0, false]

alert(arr.indexOf(0)) // 1
alert(arr.indexOf(false)) // 2
alert(arr.indexOf(null)) // -1

alert(arr.includes(1)) // true

// Includes handles NaN ===========================================
const arr = [NaN]

alert(arr.indexOf(NaN)) // -1 (should be 0, but === equality doesn't work for NaN)
alert(arr.includes(NaN)) // true (correct)
```

### **[`find`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find) and `findIndex`**

```js
let result = arr.find(function (item, index, array) {
  // if true is returned, item is returned and iteration is stopped
  // for falsy scenario returns undefined
})
```

```js
// Mainly used for an array of objects
let users = [
  { id: 1, name: "John" },
  { id: 2, name: "Pete" },
  { id: 3, name: "Mary" },
]

let user = users.find((item) => item.id == 1)

alert(user.name) // John
```

- [`arr.findIndex`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)) returns the index where the element was found instead of the element. Returns `-1` if nothing is found

### **[`filter`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)**

Used if there are more than one instance in an array. Returns an array of all matching elements

```js
let results = arr.filter(function (item, index, array) {
  // if true item is pushed to results and the iteration continues
  // returns empty array if nothing found
})
```

```js
let users = [
  { id: 1, name: "John" },
  { id: 2, name: "Pete" },
  { id: 3, name: "Mary" },
]

// returns array of the first two users
let someUsers = users.filter((item) => item.id < 3)

alert(someUsers.length) // 2
```

## **Transform an array**

### **[`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)**

```js
let result = arr.map(function (item, index, array) {
  // returns the new value instead of item
})
```

### **[`sort(fn)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)**

All elements are converted to strings during compare.

```js
let arr = [1, 2, 15]

// to sort numbers
arr.sort((a, b) => a - b) // 1, 2, 15

let countries = ["Österreich", "Andorra", "Vietnam"]

alert(countries.sort((a, b) => a.localeCompare(b))) // Andorra,Österreich,Vietnam (correct!)
```

### **[`reverse`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)**

### **`split`** and **`join`**

**[split](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/split)** and **[join](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)**

```js
// Split example
let names = "Bilbo, Gandalf, Nazgul"

let arr = names.split(", ")

for (let name of arr) {
  alert(`A message to ${name}.`) // A message to Bilbo  (and other names)
}

//  split with length arg
let arr = "Bilbo, Gandalf, Nazgul, Saruman".split(", ", 2)

alert(arr) // Bilbo, Gandalf
```

```js
// Join example
let arr = ["Bilbo", "Gandalf", "Nazgul"]

let str = arr.join(";") // glue the array into a string using ;

alert(str) // Bilbo;Gandalf;Nazgul
```

### **`reduce`**/**`reduceRight`**

**[reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)**/**[reduceRight](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)**

Used to calculate a single value based on the array.

```js
let value = arr.reduce(
  function (accumulator, item, index, array) {
    // ...
  },
  [initial]
)
```

### **[`Array.isArray(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_ObjectArray/isArray)**

To check if array or not. Returns `true` or `false`.

### **Most methods support "thisArg"**

The value of `thisArg` parameter becomes `this` for `func`.

## **[Iterables](https://javascript.info/iterable)**

Allows us to make any object useable in a `for..of` loop.

```js
let range = {
  from: 1,
  to: 5,
}

// 1. call to for..of initially calls this
range[Symbol.iterator] = function () {
  // ...it returns the iterator object:
  // 2. Onward, for..of works only with this iterator, asking it for next values
  return {
    current: this.from,
    last: this.to,

    // 3. next() is called on each iteration by the for..of loop
    next() {
      // 4. it should return the value as an object {done:.., value :...}
      if (this.current <= this.last) {
        return { done: false, value: this.current++ }
      } else {
        return { done: true }
      }
    },
  }
}

// now it works!
for (let num of range) {
  alert(num) // 1, then 2, 3, 4, 5
}
```

### **Array-likes**

Array-likes: objects that have indexes and `length`, so they look like arrays.

### **[Array.from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)**

Takes an iterable or array-like value and makes a "real" `Array` from it.

`Array.from(obj[, mapFn, thisArg])`

`mapFn` can be a function that will be applied to each element before adding it to the array.

`thisArg` allows us to set `this`

## **[Map and Set](https://javascript.info/map-set)**

### **[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)**

Like an object, but `Map` allows keys of any type

Methods and properties are:

- `new Map()` – creates the map.
- `map.set(key, value)` – stores the value by the key.
- `map.get(key)` – returns the value by the key, undefined if key doesn’t exist in map.
- `map.has(key)` – returns true if the key exists, false otherwise.
- `map.delete(key)` – removes the value by the key.
- `map.clear()` – removes everything from the map.
- `map.size` – returns the current element count.

Unlike objects, keys are not converted to strings. Any type of key is possible.

**Map can also use objects as keys.**

#### **How Map compares keys**

To test keys for equivalence, Map uses the algorithm [SameValueZero](https://tc39.es/ecma262/#sec-samevaluezero). It is roughly the same as strict equality `===`, but the difference is that NaN is considered equal to NaN. So NaN can be used as the key as well.

This algorithm can’t be changed or customized.

#### **Chaining**

Every`map.set` call returns the map itself, so we can chain calls.

```js
map.set("1", "str1").set(1, "num1").set(true, "bool1")
```

### **Iteration over Map**

3 methods:

- `map.keys()` – returns an iterable for keys,
- `map.values()` – returns an iterable for values,
- `map.entries()` – returns an iterable for entries `[key, value]`, it’s used by default in `for..of`.

Map also has builtin `forEach` method

```js
// runs the function for each (key, value) pair
recipeMap.forEach((value, key, map) => {
  alert(`${key}: ${value}`) // cucumber: 500 etc
})
```

### **Object.entries: Map from Object**

We can use `Object.entries(obj)` to create a map on existing object.

```js
let obj = {
  name: "John",
  age: 30,
}

let map = new Map(Object.entries(obj))

alert(map.get("name")) // John
```

### **Object.fromEntries: Object from Map**

`Object.fromEntries` given an array of [key, value] pairs, it creates an object from them.

We can use `Object.fromEntries` to get a plain object from `Map`
.

```js
let map = new Map()
map.set("banana", 1)
map.set("orange", 2)
map.set("meat", 4)

// A call to map.entries() returns an iterable of key/value pairs, exactly in the right format for Object.fromEntries.
let obj = Object.fromEntries(map.entries()) // make a plain object (*)

// You can omit .entries() because Object.fromEntries expects an iterable object as the arg
let obj = Object.fromEntries(map) // omit .entries()

// done!
// obj = { banana: 1, orange: 2, meat: 4 }

alert(obj.orange) // 2
```

### **Set**

Methods and properties:

- `new Set([iterable])` – creates the set, with optional `iterable` (e.g. array) of values for initialization.
- `set.add(value)` – adds a value (does nothing if `value` exists), returns the set itself.
- ` set.delete(value)` – removes the value, returns `true` if `value` existed at the moment of the call, otherwise `false`.
- `set.has(value)` – returns `true` if the value exists in the set, otherwise `false`.
- `set.clear()` – removes everything from the set.
- `set.size` – is the elements count.

**Iteration over Map and Set is always in the insertion order, so we can’t say that these collections are unordered, but we can’t reorder elements or directly get an element by its number.**

## **[WeakMap and WeakSet](https://javascript.info/weakmap-weakset)**

### **Use case: caching**

```js
// 📁 cache.js
let cache = new Map()

// calculate and remember the result
function process(obj) {
  if (!cache.has(obj)) {
    let result = /* calculations of the result for */ obj

    cache.set(obj, result)
  }

  return cache.get(obj)
}

// Now we use process() in another file:

// 📁 main.js
let obj = {
  /* let's say we have an object */
}

let result1 = process(obj) // calculated

// ...later, from another place of the code...
let result2 = process(obj) // remembered result taken from cache

// ...later, when the object is not needed any more:
obj = null

alert(cache.size) // 1 (Ouch! The object is still in cache, taking memory!)
```

**Summary**

`WeakMap` is `Map`-like collection that allows only objects as keys and removes them together with associated value once they become inaccessible by other means.

`WeakSet` is `Set`-like collection that stores only objects and removes them once they become inaccessible by other means.

Their main advantages are that they have weak reference to objects, so they can easily be removed by garbage collector.

That comes at the cost of not having support for `clear`, `size`, `keys`, `values`...

`WeakMap` and `WeakSet` are used as “secondary” data structures in addition to the “primary” object storage. Once the object is removed from the primary storage, if it is only found as the key of `WeakMap` or in a `WeakSet`, it will be cleaned up automatically.

## **[Object.keys, values, entries](https://javascript.info/keys-values-entries)**

Following methods are available:

- `Object.keys(obj)` – returns an array of keys.
- `Object.values(obj)` – returns an array of values.
- `Object.entries(obj)` – returns an array of `[key, value]` pairs.

|             | Map          | Object                                 |
| ----------- | ------------ | -------------------------------------- |
| Call syntax | `map.keys()` | `Object.keys(obj)`, but not obj.keys() |
| Returns     | iterable     | “real” Array                           |

### **Transforming objects**

1. Use `Object.entries(obj)` to get an array of key/value pairs from `obj`.
2. Use array methods on that array, e.g. map.
3. Use `Object.fromEntries(array)` on the resulting array to turn it back into an object.

```js
let prices = {
  banana: 1,
  orange: 2,
  meat: 4,
}

let doublePrices = Object.fromEntries(
  // convert to array, map, and then fromEntries gives back the object
  Object.entries(prices).map(([key, value]) => [key, value * 2])
)

alert(doublePrices.meat) // 8
```

## **[Destructuring assignment](https://javascript.info/destructuring-assignment)**

### **Array Destructuring**

```js
// we have an array with the name and surname
let arr = ["John", "Smith"]

// destructuring assignment
// sets firstName = arr[0]
// and surname = arr[1]
let [firstName, surname] = arr
```

#### **The rest '...'**

```js
let [name1, name2, ...rest] = [
  "Julius",
  "Caesar",
  "Consul",
  "of the Roman Republic",
]

// rest is array of items, starting from the 3rd one
alert(rest[0]) // Consul
alert(rest[1]) // of the Roman Republic
alert(rest.length) // 2
```

#### **Default values**

```js
let [firstName, surname] = []

alert(firstName) // undefined
alert(surname) // undefined

// default values
let [name = "Guest", surname = "Anonymous"] = ["Julius"]

alert(name) // Julius (from array)
alert(surname) // Anonymous (default used)
```

### **Object destructuring**

```js
let [firstName, surname] = []

alert(firstName) // undefined
alert(surname) // undefined

// default values
let [name = "Guest", surname = "Anonymous"] = ["Julius"]

alert(name) // Julius (from array)
alert(surname) // Anonymous (default used)
```

#### **Nested Destructuring**

```js
let options = {
  size: {
    width: 100,
    height: 200,
  },
  items: ["Cake", "Donut"],
  extra: true,
}

// destructuring assignment split in multiple lines for clarity
let {
  size: {
    // put size here
    width,
    height,
  },
  items: [item1, item2], // assign items here
  title = "Menu", // not present in the object (default value is used)
} = options
```

#### **Smart function parameters**

When creating functions with a lot of parameters, just pass in an object of the arguments as a paramater instead. Inside the function, you can destructure the arguments from the object.

```js
// we pass object to function
let options = {
  title: "My menu",
  items: ["Item1", "Item2"],
}

// ...and it immediately expands it to variables
function showMenu({
  title = "Untitled",
  width = 200,
  height = 100,
  items = [],
}) {
  // title, items – taken from options,
  // width, height – defaults used
  alert(`${title} ${width} ${height}`) // My Menu 200 100
  alert(items) // Item1, Item2
}

// nested destructuring
function showMenu({
  title = "Untitled",
  width: w = 100, // width goes to w
  height: h = 200, // height goes to h
  items: [item1, item2], // items first element goes to item1, second to item2
}) {
  alert(`${title} ${w} ${h}`) // My Menu 100 200
  alert(item1) // Item1
  alert(item2) // Item2
}

showMenu(options)
```

Destructuring assumes that `showMenu()` does have an argument. If we want all values by default, then we should specify an empty object:

```js
function showMenu({ title = "Menu", width = 100, height = 200 } = {}) {
  alert(`${title} ${width} ${height}`)
}

showMenu() // Menu 100 200
```

### **[Data and time](https://javascript.info/date)**

#### `new Date(milliseconds)`

An integer number representing the number of milliseconds that has passed since the beginning of 1970 is called a timestamp.

Dates before 01.01.1970 have negative timestamps

#### `new Date(datestring)`

If there is a single argument, and it’s a string, then it is parsed automatically. Uses `Date.parse`.

#### `new Date(year, month, date, hours, minutes, seconds, ms)`

- The `year` must have 4 digits: `2013` is okay, `98` is not.
- The `month` count starts with `0` (Jan), up to `11` (Dec).
- The `date` parameter is actually the day of month, if absent then `1` is assumed.
- If `hours/minutes/seconds/ms` is absent, they are assumed to be equal `0`.

```js
new Date(2011, 0, 1, 0, 0, 0, 0) // 1 Jan 2011, 00:00:00
new Date(2011, 0, 1) // the same, hours etc are 0 by default

let date = new Date(2011, 0, 1, 2, 3, 4, 567)
alert(date) // 1.01.2011, 02:03:04.567
```

### **Access date components**

#### [`getFullYear()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getFullYear)

Get the year (4 digits)

#### [`getMonth()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getMonth)

Get the month, from 0 to 11.

#### [`getDate()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getDate)

Get the day of month, from 1 to 31, the name of the method does look a little bit strange.

#### `getHours()`, `getMinutes()`, `getSeconds()`, `getMilliseconds()`

Mdn links:
[`getHours()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getHours), [`getMinutes()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getMinutes), [`getSeconds()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getSeconds), [`getMilliseconds()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getMilliseconds)

Get the corresponding time components.

#### [`getDay()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getDay)

Get the day of week, from 0 (Sunday) to 6 (Saturday). The first day is always Sunday, in some countries that’s not so, but can’t be changed.

**All the methods above return the components relative to the local time zone.**

No UTC-variant:

#### [`getTime()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime)

Returns the timestamp for the date – a number of milliseconds passed from the January 1st of 1970 UTC+0.

#### [`getTimezoneOffset()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getTimezoneOffset)

Returns the difference between UTC and the local time zone, in minutes:

### **Autocorrection**

We can set out-of-range values, and it will auto-adjust itself.

### **Date to number, date diff**

`Date` object convert to number = timestamp = same as `date.getTime()`.

Dates can be subtracted, result is difference in ms.

### **Date.now()**

If we just want to measure time. `Date.now()` = `new Date().getTime()`. `Date.now()` doesnt create an intermediate `Date` object. It's faster and doesn't put pressure on garbage collection.

### **Benchmarking**

```js
function diffSubtract(date1, date2) {
  return date2 - date1
}

function diffGetTime(date1, date2) {
  return date2.getTime() - date1.getTime()
}

function bench(f) {
  let date1 = new Date(0)
  let date2 = new Date()

  let start = Date.now()
  for (let i = 0; i < 100000; i++) f(date1, date2)
  return Date.now() - start
}

let time1 = 0
let time2 = 0

// added for "heating up" prior to the main loop
bench(diffSubtract)
bench(diffGetTime)

// run bench(diffSubtract) and bench(diffGetTime) each 10 times alternating
for (let i = 0; i < 10; i++) {
  time1 += bench(diffSubtract)
  time2 += bench(diffGetTime)
}

alert("Total time for diffSubtract: " + time1)
alert("Total time for diffGetTime: " + time2)
```

### **[`Date.parse`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/parse) from a string**

`YYYY-MM-DDTHH:mm:ss.sssZ` returns the timestamp

- `YYYY-MM-DD` – is the date: year-month-day. Can also use `YYYY-MM` or `YYYY`.
- The character `"T"` is used as the delimiter.
- `HH:mm:ss.sss` – is the time: hours, minutes, seconds and milliseconds.
- The optional `"Z"` part denotes the time zone in the format `+-hh:mm`. A single letter `Z` would mean UTC+0.

## **[Error Handling](https://javascript.info/error-handling)**

### **[Try...catch](https://javascript.info/try-catch)**

![Try catch flow diagram](10-error_handling/images/try-catch/try-catch-flow.png)

### **Error object**

```js
try {
  // ...
} catch (err) {
  // <-- the "error object", could use another word instead of err
  // ...
}
```

### **Optional "catch" binding**

If we don't need error details, `catch` may be omitted.

### **Error Object**

#### `name`

Error name. For instance, for an undefined variable that’s "ReferenceError".

#### `message`

Textual message about error details.
There are other non-standard properties available in most environments. One of most widely used and supported is:

#### `stack`

Current call stack: a string with information about the sequence of nested calls that led to the error. Used for debugging purposes.

### **"Throw" operator**

Built in constructors for standard errors: `Error`, `SyntaxError`, `ReferenceError`, `TypeError`

For built-in errors, the `name` property is the same name as the constructor. The `message` property is taken from the argument passed in the constructor.

Ex:

```js
let error = new Error("Things happen o_O")

alert(error.name) // Error
alert(error.message) // Things happen o_O
```

Example of throwing an error:

```js
let json = '{ "age": 30 }' // incomplete data

try {
  let user = JSON.parse(json) // <-- no errors

  if (!user.name) {
    throw new SyntaxError("Incomplete data: no name") // (*)
  }

  alert(user.name)
} catch (err) {
  alert("JSON Error: " + err.message) // JSON Error: Incomplete data: no name
}
```

### **Rethrowing**

**Catch should only process errors that it knows and “rethrow” all others.**

Rethrowing technique:

1. Catch gets all errors.
2. In the `catch (err) {...}` block we analyze the error object `err`.
3. If we don’t know how to handle it, we do `throw err`.

### **try…catch…finally**

`finally` block will always execute after a successful `try` block or after a `catch` block.

The `finally` clause works for any exit from `try...catch`. That includes an explicit `return`.

Ex:

```js
function func() {
  try {
    return 1
  } catch (err) {
    /* ... */
  } finally {
    alert("finally")
  }
}

alert(func()) // finally alerted first, then 1
```

### **Global Catch**

For a fatal error outside of `try...catch`.

```js
window.onerror = function (message, url, line, col, error) {
  // ...
}
```

| Parameter     | Description                                   |
| ------------- | --------------------------------------------- |
| `message`     | Error message.                                |
| `url`         | URL of the script where error happened.       |
| `line`, `col` | Line and column numbers where error happened. |
| `error`       | Error object.                                 |

The role of the global handler window.onerror is usually not to recover the script execution. There are also web-services that provide error-logging for such cases, like https://errorception.com or http://www.muscula.com.

They work like this:

1. We register at the service and get a piece of JS (or a script URL) from them to insert on pages.
2. That JS script sets a custom window.onerror function.
3. When an error occurs, it sends a network request about it to the service.
4. We can log in to the service web interface and see errors.

## **[Custom errors, extending Error](https://javascript.info/custom-errors)**
