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
    - [**indexOf/lastIndexOf and includes**](#indexoflastindexof-and-includes)
    - [**find and findIndex**](#find-and-findindex)
    - [**filter**](#filter)
  - [**Transform an array**](#transform-an-array)
    - [**map**](#map)
    - [**sort(fn)**](#sortfn)
    - [**reverse**](#reverse)
    - [**split** and **[join](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)**](#split-and-join)
    - [**reduce**/**[reduceRight](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)**](#reducereduceright)
    - [**Array.isArray(value)**](#arrayisarrayvalue)
    - [**Most methods support "thisArg"**](#most-methods-support-thisarg)

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

| Number | Math.floor | Math.ceil | Math.round | Math.trunc |
| ------ | ---------- | --------- | ---------- | ---------- |
| 3.1    | 3          | 4         | 3          | 3          |
| 3.6    | 3          | 4         | 4          | 3          |
| -1.1   | -2         | -1        | -1         | -1         |
| -1.6   | -2         | -1        | -2         | -1         |

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
alert(str[0]) // doesn't wor
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

### **indexOf/lastIndexOf and includes**

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

### **[find](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find) and findIndex**

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

- `arr.findIndex` returns the index where the element was found instead of the element. Returns `-1` if nothing is found

### **[filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)**

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

### **[map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)**

```js
let result = arr.map(function (item, index, array) {
  // returns the new value instead of item
})
```

### **[sort(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)**

All elements are converted to strings during compare.

```js
let arr = [1, 2, 15]

// to sort numbers
arr.sort((a, b) => a - b) // 1, 2, 15

let countries = ["Österreich", "Andorra", "Vietnam"]

alert(countries.sort((a, b) => a.localeCompare(b))) // Andorra,Österreich,Vietnam (correct!)
```

### **[reverse](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)**

### **[split](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/split)** and **[join](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)**

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

### **[reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)**/**[reduceRight](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight)**

Used to calculate a single value based on the array.

```js
let value = arr.reduce(
  function (accumulator, item, index, array) {
    // ...
  },
  [initial]
)
```

### **[Array.isArray(value)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_ObjectArray/isArray)**

To check if array or not. Returns `true` or `false`.

### **Most methods support "thisArg"**

The value of `thisArg` parameter becomes `this` for `func`.
