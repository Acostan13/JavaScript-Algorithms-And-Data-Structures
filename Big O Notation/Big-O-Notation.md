# Big O Notation

## Timing of Code

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
console.log(`Time Elapsed: ${(time2 - time1) / 1000} seconds.`) // Significantly Faster! => ~0.000024999999324791133 seconds
```

### The Problem with Time
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


