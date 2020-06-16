# Algorithms and Problem Solving Patterns

## Step 1: Understand the Problem

- Can I restate the problem in my own words?
- What are the inputs that go into the problem?
- What are the outputs that should come from the solution to the problem?
- Can the outputs be determined from the inputs? In other words, do I have enough information to solve the problem? (You may not be able to answer this question until you set about solving the problem. That's okay; it's still worth considering the question at this early stage.)
- How should I label the important pieces of data that are a part of the problem?

## Step 2: Explore Concrete Examples

- Start with Simple Examples
- Progress to More Complex Examples
- Explore Examples with Empty Inputs
- Explore Examples with Invalid Inputs

**Example**:
**Write a function which takes in a string and returns counts of each character in the string**

```JavaScript
charCount("aaaa") // {a:4}
charCount("hello") // {h:1, e:1, l:2, o:1}

charCount("my phone number is 1987432") // Do we account for spaces?
charCount("Hello hi") // Do we consider identical characters with different casing as the same key value?
charCount("") // Do we want to return an empty object? or rather null instead? or maybe an error?

```

## Step 3: Break it Down

**Explicitly write out the steps you need to take.**

```JavaScript
function charCount(str){
    // do something
    // return and object with keys that are lowercase alphanumeric characters in the string
    // values should be the counts for those characters
}
```

```JavaScript
function charCount(str){
    // make object to return at end
    // loop over string, for each character...
        // if the char is a number/letter AND is a key in object, add one to count
        // if the char is a number/letter AND NOT in object, add it object and set value to 1
        // if character is something else (space, period, etc.), don't do anything
    // return object at end
}
```

## Step 4: Solve or Simplify

- Find the core difficulty in what you're trying to do
- Temporarily ignore that difficulty
- Write a simplified solution
- Then incorporate that difficulty back in

```JavaScript
function charCount(str){
    // make object to return at end
    let result = {}
    // loop over string, for each character...
    for(let i=0; i < str.length; i++){
        let char = str[i].toLowerCase()
        // if the char is a number/letter
        if(/[a-z0-9]/.test(char)) {
            // if the key exists in the object already, add one to count
            if(result[char] > 0) {
                result[char]++
            }
            // if the char is not in the object, add it to the object and set value to 1
            else {
                result[char] = 1
            }
        }
    }
    // return object at end
    return result
}
```

## Step 5: Look Back & Refactor

- Can you check the result?
- Can you derive the result differently?
- Can you understand it at a glance?
- Can you use the result or method for some other problem?
- Can you improve the performance of your solution?
- Can you think of other ways to refactor?
- How have other people solved this problem?

### Slower Solution

```JavaScript
function charCount(str){
    // make object to return at end
    let result = {}
    // loop over string, for each character...
    for(let char of str){
        char = char.toLowerCase()
        // if the char is a number/letter
        if(/[a-z0-9]/.test(char)) {
            // add one to the char count OR add the char to the result objejct and set it's value to 1
            result[char] = ++result[char] || 1
        }
    }
    // return the result
    return result
}
```

### Faster Solution

```JavaScript
function charCount(str){
    // make object to return at end
    let result = {}
    // loop over string, for each character...
    for(let char of str){
        char = char.toLowerCase()
        // if the char is a number/letter
        if(isAplhaNumeric(char)) {
            // add one to the char count OR add the char to the result objejct and set it's value to 1
            result[char] = ++result[char] || 1
        }
    }
    // return the result
    return result
}

function isAplhaNumeric(char){
    let code = char.charCodeAt(0)
    if( !(code > 47 && code < 58) &&
        !(code > 64 && code < 91) &&
        !(code > 96 && code < 123)) {
        return false
    }
    return true
}
```