# JavaScript : Understanding the weird Parts #2

Created: Jun 27, 2020
Tags: javascript

# Types and Operators

## Summary Points

**Types :** Javascript is a loosely typed / dynamically typed language.  Under the hood, JS does have primitive types, which are  -

- Boolean
- Number (it's floating by default)
- String
- Undefined
- Null
- Symbol

**Coercion :** Coercion is the process by which javascript converts a value from one data type to the other data type. This happens in javascript quite often as it's a dynamically typed language.  Each time an operator gets a value it didn't expect, it coerces the given value to something which it considers most appropriate. 

Example - 

`console.log("hello"  + 007);`

would result in `hello007` even though the second operand here is a number.

**Coercion and Comparison Operators :**  Generally observed, coercion works in most error prone ways when working with the comparison operators. 

Example - 

`console.log(3 < 2 < 1);`

The above statement result in `true` that's because - considering left to right associativity for the less than function, we see that `3 < 2` results in `false` after which, the function call reduced to - `false < 1` at which point javascipt coerces the value to `false` to be `0` which results in the following expression - 

`console.log(0 < 1);` which is obviously `true`.

**Equality Operator :** The weird nature of the coercion is felt through in the usage of the equality operator. 

Example - 

```jsx
console.log(3 == "3); //Results in true

var a = false;
console.log(a == 0); // Results in true
console.log(null == 0) // Results in false
console.log(null < 1) // Results in true 
```

**Strict Equality :** This new operator with 3 equal signs `===` was introduced to solve the previous problem with javascript. It doesn't allow coercion to seep in and make matters weird.

Similar to this, there's **Strict Inequality operator `!==`** which does the same thing and allows a sane method to check for inequalities in javascipt. 

### always use the strict equality and strict inequality operators, unless you want coercion to happen.