# Problem Solving Patterns

## Frequency Counters

This pattern uses objects or sets to collect values/frequencies of values

This can often avoid the need for nested loops or O(N^2) operations with arrays / strings

### Example

Write a function called **same**, which accepts two arrays. The function should return true if every value in the array has it's corresponding value squared in the second array. The frequency of values must be the same.

**Initial Solution**

- Time Complexity - O(n^2)

```JavaScript
same = (arr1, arr2) => {
    if(arr1.length !== arr2.length){
        return false;
    }

    for(let i = 0; i < arr1.length; i++){
        let correctIndex = arr2.indexOf(arr1[i] ** 2)
        if(correctIndex === -1) {
            return false;
        }
        arr2.splice(correctIndex,1)
    }
    return true
}

same([1,2,3], [4,1,9]) // true
same([1,2,3], [1,9]) // false
same([1,2,1], [4,4,1]) // false (must be same frequency)
```

**Refactored Solution**

- Time Complexity - O(n)

```JavaScript
same = (arr1, arr2) => {
    if(arr1.length !== arr2.length){
        return false;
    }

    // objects which keep track of frequency of items in arrays
    let frequencyCounter1 = {}
    let frequencyCounter2 = {}

    // adds counter value to their respective object
    for(let val of arr1){
        frequencyCounter1[val] = (frequencyCounter1[val] || 0) + 1
    }
    for(let val of arr2){
        frequencyCounter2[val] = (frequencyCounter2[val] || 0) + 1
    }

    // verifies if all values in arr1 are within arr2
    for(let key in frequencyCounter1){
        if(!(key ** 2 in frequencyCounter2)){
            return false
        }
        if(frequencyCounter2[key ** 2] !== frequencyCounter1[key]){
            return false
        }
    }
    return true
}

same([1,2,3,2], [9,1,4,4]) // true
```

### Challenge: Anagrams

Given two strings, write a function to determine if the second string is an anagram of the first. An anagram is a word, phrase, or name formed by rearranging the letters of another, such as _cinema_, formed from _iceman_.

```JavaScript
validAnagram('', '') // true
validAnagram('aaz', 'zza') // false
validAnagram('anagram', 'nagaram') // true
validAnagram("rat","car") // false) // false
validAnagram('awesome', 'awesom') // false
validAnagram('qwerty', 'qeywrt') // true
validAnagram('texttwisttime', 'timetwisttext') // true
```

**Initial Solution**

- Time Complexity - O(n)

```JavaScript
validAnagram = (word1, word2) => {
    // edge case if both words are identical
    if(word1 === word2) {
        return true
    }
    // objects which keep track of frequency of items in arrays
    let frequencyCounter1 = {}
    let frequencyCounter2 = {}

    for (let letter of word1) {
        frequencyCounter1[letter] = (frequencyCounter1[letter] || 0) + 1
    }
    for (let letter of word2) {
        frequencyCounter2[letter] = (frequencyCounter2[letter] || 0) + 1
    }
    // If number of properties is different,
    // objects are not equivalent
    if (frequencyCounter1.length != frequencyCounter2.length) {
        return false
    }

    for (let key in frequencyCounter1) {
        // If values of same property are not equal,
        // objects are not equivalent
        if (frequencyCounter1[key] !== frequencyCounter2[key]) {
        return false
        }
    }
    // If we made it this far, objects
    // are considered equivalent
    return true
}
```

**Refactored Solution**

- Time Complexity - O(n)

```JavaScript
function validAnagram(first, second) {
  if (first.length !== second.length) {
    return false
  }

  const lookup = {}

  for (let i = 0; i < first.length; i++) {
    let letter = first[i]
    // if letter exists, increment, otherwise set to 1
    lookup[letter] ? lookup[letter] += 1 : lookup[letter] = 1
  }

  for (let i = 0; i < second.length; i++) {
    let letter = second[i]
    // can't find letter or letter is zero then it's not an anagram
    if (!lookup[letter]) {
      return false
    } else {
      lookup[letter] -= 1
    }
  }

  return true;
}

// {a: 0, n: 0, g: 0, r: 0, m: 0,s:1}
validAnagram('anagrams', 'nagaramm')
```

## Multiple Pointers Pattern

Creating **pointers** or values that correspond to an index or position and move towards the beginning, end or middle based on a certain condition. **Very** efficient for solving problems with minimal space complexity as well.

### Example

Write a function called **sumZero** which accepts a **sorted** array of integers. The function should find the **first** pair where the sum is 0. Return an array that includes both values that sum to zero or undefined if a pair does not exist.

```Javascript
sumZero([-3,-2,-1,0,1,2,3]) // [-3,3]
sumZero([-2,0,1,3]) // undefined
sumZero([1,2,3]) // undefined
```

**Naive Solution**

- Time Complexity - O(n^2)
- Space Complexity - O(1)

```JavaScript
sumZero = (arr) => {
    for(let i = 0; i < arr.length; i++){
        for(let j = i+1; j < arr.length; j++){
            if(arr[i] + arr[j] === 0){
                return [arr[i], arr[j]]
            }
        }
    }
}

sumZero([-4, -3, -2, -1, 0, 1, 2, 5]) // [-2,2]
```

**Refactored Solution**

- Time Complexity - O(n)
- Space Complexity - O(1)

```JavaScript
sumZero = (arr) => {
    let left = 0
    let right = arr.length - 1
    while(left < right){
        let sum = arr[left] + arr[right];
        if(sum === 0){
            return [arr[left], arr[right]];
        } else if(sum > 0){
            right--
        } else {
            left++
        }
    }
}
sumZero([-4, -3, -2, -1, 0, 1, 2, 3, 10]) // [-3,3]
```

### Challenge: Count Unique Values

Implement a function called `countUniqueValues`, which accepts a sorted array, and counts the unique values in the array. There can be negative numbers in the array, but it will always be sorted.

```JavaScript
countUniqueValues([1,1,1,1,1,2]) // 2
countUniqueValues([1,2,3,4,4,4,7,7,12,12,13]) // 7
countUniqueValues([]) // 0
countUniqueValues([-2,-1,-1,0,1]) // 4
```

**Initial Solution**

- Time Complexity - O(n)
- Space Complexity - O(1)

```JavaScript
countUniqueValues = (arr) => {

  if(arr.length === 0) {
    return 0
  }

  let left = 0
  let right = 1

  while(right < arr.length){
    if(arr[left] === arr[right]){
      right++
    } else {
      left++
      arr[left] = arr[right]
    }
  }
  return left + 1
}

countUniqueValues([1,1,1,1,1,2]) // 2
countUniqueValues([1,2,3,4,4,4,7,7,12,12,13]) // 7
countUniqueValues([]) // 0
countUniqueValues([-2,-1,-1,0,1]) // 4
```

**Refactored Solution**

- Time Complexity - O(n)
- Space Complexity - O(1)

```JavaScript
countUniqueValues = (arr) => {
    if(arr.length === 0) return 0;
    let i = 0
    for(let j = 1; j < arr.length; j++){
        if(arr[i] !== arr[j]){
            i++
            arr[i] = arr[j]
        }
    }
    return i + 1
}

countUniqueValues([1,1,1,1,1,2]) // 2
countUniqueValues([1,2,3,4,4,4,7,7,12,12,13]) // 7
countUniqueValues([]) // 0
countUniqueValues([-2,-1,-1,0,1]) // 4
```

## Sliding Window Pattern

This pattern involves creating a **window** which can either be an array or number from one position to another.
Depending on a certain condition, the window either increases or closes (and a new window is created).
Very useful for keeping track of a subset of data in an array/string etc.

### Example

Write a function called maxSubarraySum which accepts an array of integers and a number called _n_. The function should calculate the maximum sum of n consecutive elements in the array.

```JavaScript
maxSubarraySum([1,2,5,2,8,1,5],2) // 10
maxSubarraySum([1,2,5,2,8,1,5],4) // 17
maxSubarraySum([4,2,1,6],1) // 6
maxSubarraySum([4,2,1,6,2],4) // 13
maxSubarraySum([],4) // null
```

**Naive Solution**

- Time Complexity - O(n^2)

```JavaScript
maxSubarraySum = (arr, num) => {
  if ( num > arr.length){
    return null
  }
  var max = -Infinity
  for (let i = 0; i < arr.length - num + 1; i ++){
    temp = 0
    for (let j = 0; j < num; j++){
      temp += arr[i + j]
    }
    if (temp > max) {
      max = temp
    }
  }
  return max
}
```

**Refactored Solution**

- Time Complexity - O(n)

```JavaScript
maxSubarraySum = (arr, num) => {
  let maxSum = 0
  let tempSum = 0
  if (arr.length < num) return null
  for (let i = 0; i < num; i++) {
    maxSum += arr[i]
  }
  tempSum = maxSum;
  for (let i = num; i < arr.length; i++) {
    tempSum = tempSum - arr[i - num] + arr[i]
    maxSum = Math.max(maxSum, tempSum)
  }
  return maxSum
}
```

## Divide and Conquer

This pattern involves dividing a data set into smaller chunks and then repeating a process with a subset of data.
This pattern can tremendously **decrease time complexity**

### Example

Given a sorted array of integers, write a function called search, that accepts a value and returns the index where the value passed to the function is located. If the value is not found, return -1

```JavaScript
search([1,2,3,4,5,6],4) // 3
search([1,2,3,4,5,6],6) // 5
search([1,2,3,4,5,6],11) // -1
```

**Naive Solution**

- Linear Search
- Time Complexity - O(n)

```JavaScript
search = (arr, val) => {
    for(let i = 0; i < arr.length; i++){
        if(arr[i] === val){
            return i
        }
    }
    return -1
}
```

**Refactored Solution**

- Binary Search
- Time Complexity - O(log n)

```JavaScript
search = (arr, val) => {
    let min = 0
    let max = array.length - 1

    while (min <= max) {
        let middle = Math.floor((min + max) / 2)
        let currentElement = array[middle]

        if (array[middle] < val) {
            min = middle + 1
        }
        else if (array[middle] > val) {
            max = middle - 1
        }
        else {
            return middle
        }
    }
    return -1
}
```

## Recap
- Developing a problem solving approach is incredibly important
- Thinking about code before writing code will always make you solve problems faster
- Be mindful about problem solving patterns
- Using frequency counters, multiple pointers, sliding window and divide and conquer will help you reduce time and space complexity and help solve more challenging problems
