# Problem Solving Patterns

## Frequency Counters

This pattern uses objects or sets to collect values/frequencies of values

This can often avoid the need for nested loops or O(N^2) operations with arrays / strings

## Example

Write a function called same, which accepts two arrays. The function should return true if every value in the array has it's corresponding value squared in the second array. The frequency of values must be the same.

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

## Challenge: Anagrams

Given two strings, write a function to determine if the second string is an anagram of the first. An anagram is a word, phrase, or name formed by rearranging the letters of another, such as cinema, formed from iceman.

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

Creating pointers or values that correspond to an index or position and move towards the beginning, end or middle based on a certain condition.
Very efficient for solving problems with minimal space complexity as well.

## Example

Write a function called same, which accepts two arrays. The function should return true if every value in the array has it's corresponding value squared in the second array. The frequency of values must be the same.

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

