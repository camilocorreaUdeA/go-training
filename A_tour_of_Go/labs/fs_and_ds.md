# Time to practice...

## 1. Slice remove and insert

* Implement a function to remove an element from a slice. If the element is not found an error should be returned.
* Implement a function to insert elements anywhere in a slice. Make sure to check boundaries and return an error when convenient.
* Implement the corresponding unit tests to your functions, use `testify` package ([testify](https://github.com/stretchr/testify))

## 2. CamelCase

There is a sequence of words in CamelCase as a string of letters, <b><i>s</b></i>, having the following properties:

* It is a concatenation of one or more words consisting of English letters.
* All letters in the first word are lowercase.
* For each of the subsequent words, the first letter is uppercase and rest of the letters are lowercase.

Given <b><i>s</b></i>, determine the number of words in <b><i>s</b></i>.

Example

<b><i>s = oneTwoThree</b></i> 

There are 3 words in the string: 'one', 'Two', 'Three'.

* Code a function that receives a string as input and returns the number of words in the string.
* The function should check any errors in the input string (empty string, spaces, etc)
* Add unit test to you function with `testify`

## 3. Unique names

Implement the uniqueNames function. When passed to slices of names, it will return a slice containing the names that appear in either or both slices. The returned slice should have no duplicates.

For example, calling <i>uniqueNames([]string{"Juan", "Diana", "Andrea", "Carlos"}, []string{"Jorge", "Carlos", "Sofia"}) should return a slice containing Juan, Diana, Andrea, Carlos, Jorge, Sofia.

Don't forget to unit test your function.

