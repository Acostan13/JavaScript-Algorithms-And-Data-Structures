# Big O Notation

## Time Complexity

**Example**
Suppose we want to write a function that calculates the sum of all numbers from 1 up to (and including) some number ```n```.


### Slower Solution
```JavaScript
function addUpTo(n) {
  let total = 0;
  for (let i = 1; i <= n; i++) {
    total += i;  // Number of operations is (eventually) bounded by a multiple of n => O(n)
  }
  return total;
}

var t1 = performance.now();
addUpTo(1000000000);
var t2 = performance.now();
console.log(`Time Elapsed: ${(t2 - t1) / 1000} seconds.`) // => ~0.91 seconds
```


### Faster Solution
```JavaScript
function addUpTo(n) {
  return n * (n + 1) / 2; // Always 3 operations => O(1)
}

var time1 = performance.now();
addUpTo(1000000000);
var time2 = performance.now();
console.log(`Time Elapsed: ${(time2 - time1) / 1000} seconds.`) // Significantly Faster! => ~0.000025 seconds
```

The Problem with Time
- Different machines will record different times
- The same machine will record different times!
- For fast algorithms, speed measurements may not be precise enough?

### Another Example ###
```JavaScript
function countUpAndDown(n) {
  console.log("Going up!");
  for (let i = 0; i < n; i++) { // => O(n)
    console.log(i);
  }
  console.log("At the top!\nGoing down...");
  for (let j = n - 1; j >= 0; j--) { // => O(n)
    console.log(j);
  }
  console.log("Back down. Bye!");
}
```

### Yet Another Example ###
```JavaScript
function printAllPairs(n) {
  for (var i = 0; i < n; i++) {
    for (var j = 0; j < n; j++) { // O(n) operation inside of an O(n) operation => O(n^2)
      console.log(i, j);
    }
  }
}
```

### Simplifying Big O Expressions
When determining the time complexity of an algorithm, there are some helpful rule of thumbs for big O expressions.
- Constants don't matter
- Smaller Terms Don't Matter

### Big O Shorthands ###
- Arithmetic operations are constant
- Variable assignment is constant
- Accessing elements in an array (by index) or object (by key) is constant
- In a loop, the the complexity is the length of the loop times the complexity of whatever happens inside of the loop


### A Couple More Examples ###

```JavaScript
function logAtLeast5(n) {
  for (var i = 1; i <= Math.max(5, n); i++) { // => O(n)
    console.log(i);
  }
}
```
```JavaScript
function logAtMost5(n) {
  for (var i = 1; i <= Math.min(5, n); i++) { // => O(1)
    console.log(i);
  }
}
```

## Space Complexity

- Most primitives (booleans, numbers, undefined, null) are constant space
- Strings require O(n) space (where n is the string length)
- Reference types are generally O( n), where n is the length (for arrays) or the number of keys (for objects)

### An Example ###
```JavaScript
function sum(arr) {
  let total = 0; // One number
  for (let i = 0; i < arr.length; i++) { // i => Another number
    total += arr[i];
  }
  return total;
} // 2 numbers => O(1) space
```

### Another Example ###
```JavaScript
function double(arr) {
  let newArr = [];
  for (let i = 0; i < arr.length; i++) {
    newArr.push(2 * arr[i]);
  }
  return newArr; // n numbers => O(n) space
}
```
