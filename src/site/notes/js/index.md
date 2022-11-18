---
{"dg-publish":true,"dg-permalink":"js","permalink":"/js/","dgHomeLink":true,"dgPassFrontmatter":false}
---


# JS Basics

## Looping Arrays

### for Loop

```js
for (initialization; condition; update statement) {
// code block to be executed
}
```

### while Loop

condition: defines the loop stop condition

```js
while (condition) {
// code block to be executed
}
```

### do/while Loop

condition: defines the loop stop condition

```js
do {
// code block to be executed
} while (condition)
```

### for/of Loop

```js
for (let index of array) {
console.log(index.name)
}
```

```js
for (let index in array) {
console.log(array[index])
}
```

### forEach()

The forEach() will execute the provided callback function once for each array element.

```js
array.forEach(callback(currentValue [, index [, array]])[, thisArg]);
```

### map()

The difference between the forEach and map, is that the map() method creates and returns a new array based on the result of the provided callback function.

```js
var new_array = array.map(function callback(currentValue[, index[, array]]) {
// Return element for new_array
}[, thisArg])
```

```js
var words = ['uno', 'dos', 'tres', 'cuatro'];
words.forEach(function(word) {
console.log(word);
if (word === 'dos') {
words.shift();
}
});
```

## Reduce

The array.reduce() is a method on array that accepts 2 arguments:

```js
const value = array.reduce(callback[, initialValue]);
```

The callback is an obligatory argument that is a function performing the reduce operation, and the second optional argument is the initial value.

JavaScript invokes the callback function upon each item of the array with 4 arguments: the accumulator value, the current array item, the current array item index, and the array itself. The callback function must return the updated accumulator value.

## Index Of

```js
var array = [2, 9, 9];
array.indexOf(2); // 0
array.indexOf(7); // -1
array.indexOf(9, 2); // 2
array.indexOf(2, -1); // -1
array.indexOf(2, -3); // 0
```

## Removing Values

JavaScript gives us four methods to add or remove items from the beginning or end of arrays:

### pop()

```js
Remove an item from the end of an array
let cats = ['Bob', 'Willy', 'Mini'];

cats.pop(); // ['Bob', 'Willy']
pop() returns the removed item.
```

### push()

```js
Add items to the end of an array
let cats = ['Bob'];

cats.push('Willy'); // ['Bob', 'Willy']

cats.push('Puff', 'George'); // ['Bob', 'Willy', 'Puff', 'George']
push() returns the new array length.
```

### shift()

```js
Remove an item from the beginning of an array
let cats = ['Bob', 'Willy', 'Mini'];

cats.shift(); // ['Willy', 'Mini']
shift() returns the removed item.

unshift()
: Add items to the beginning of an array
let cats = ['Bob'];

cats.unshift('Willy'); // ['Willy', 'Bob']

cats.unshift('Puff', 'George'); // ['Puff', 'George', 'Willy', 'Bob']
unshift() returns the new array length.
```

### splice()

```js
Array.prototype.splice()
const months = ['Jan', 'March', 'April', 'June'];
months.splice(1, 0, 'Feb');
// inserts at index 1
console.log(months);
// expected output: Array ["Jan", "Feb", "March", "April", "June"]

months.splice(4, 0, 'May');
// replaces 1 element at index 4
console.log(months);
// expected output: Array ["Jan", "Feb", "March", "April", "May"]
```
