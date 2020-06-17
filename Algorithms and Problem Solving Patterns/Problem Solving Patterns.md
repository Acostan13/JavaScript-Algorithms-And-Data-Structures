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