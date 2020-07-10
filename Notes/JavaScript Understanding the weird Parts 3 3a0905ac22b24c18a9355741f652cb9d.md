# JavaScript : Understanding the weird Parts #3

Created: Jun 27, 2020
Tags: javascript

# Objects and Functions

## Understanding Objects -

![JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled.png](JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled.png)

- Objects are name value pairs which sit in memory and have references to it's own properties and methods.
- The property can either be a primitive one or another object entirely.
- The properties and methods of an object can be accesses via the dot operator, or the computed member access operator (`employee["age"] = 34;`)
- Objects can either be created with the new operator or the **object literal**.

    Example - 

    ```jsx
    var people = new Object();
    			//OR
    var people = {};
    ```

- With the help of object literals, java script allows us to create objects on the fly, which is similar to how it treats primitive types, where in any number / string etc is created instantly.

    Example - 

    ```jsx
    function greet(person) {
    	console.log("Hello " + person.firstName);
    }

    greet({firstName : "Frank",
    			lastName : "Lampard"
    });

    //The above code would result in printing - Hello Frank
    ```

- ***Faking Namespaces :***
    - Namespaces are containers for objects and functions.
    - JS allows us to create namespaces with the help of objects and the dot operator.
    - Ex -

    ```jsx
    var greet = 'Hello!';
    var greet = 'Hola!'; 

    console.log(greet);

    var english = { 
        greetings: { 
            basic: 'Hello!' 
        } 
    };

    var spanish = {};

    spanish.greet = 'Hola!';

    console.log(spanish.greet)  //Hole!
    console.log(english.greetings.basic);  //Hello!

    ```

**JSON** vs Object literals : 

- JSON stands for Javascript Object Notation.
- It was inspired by the javascript object literals.
- JSON and Object Literals are not the same thing.
- JSON is a subset of Object Literals.
- All JSON are valid Object Literals but all Object Literals are not JSON
- That's because JSON strictly requires even the property names to be wrapped in quotes
- `JSON.parse()` returns a javascript object from JSON
- `JSON.stringify()` return JSON from a javascript object.

## FUNCTIONS ARE OBJECTS!

![JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled%201.png](JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled%201.png)

- In javasctipt, we have **first class functions**.
- First class functions mean that anything we can do with other types, we can do with function, which means that we can pass them around as arguments, we can assign them to values and create them on the fly.
- Essentially, *functions are objects*.
- This means that function have properties the same way as other objects.
- The code that we write isn't the entire function, but just a single property of the function.
- 2 special properties that a function has are *name* and *code*. The *code* property is invocable.
- Function expression returns a value which need not be saved.
- Function statement does some work.
- Anonymous functions are functions which do not have a name.

## Objects, Functions and "this"-

- Each time a function is invoked, an execution context is created.
- Every execution context gives us a keyword - ***this.***
- Depending on how the function is executed, ***this*** can point to different things.
- By default in an empty js file,  ***this*** points to the global object (which is the Window object in a browser)
- Inside all the functions on the global execution context, ***this*** also points to the global object.
- Inside methods of Objects, ***this*** points to the object of which the method is part of.
- The weird part in using the ***this*** keyword comes when you use it inside of a method of an object. At that place, ***this*** points to the global object again!!!
- To prevent the previous problem, we generally use a pattern where we create a variable called **self** and set it equal to ***this*** at the start of the method and then use **self** in subsequent calls to the current object.

## Arguements and "default parameters" and Function Overloading

![JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled%202.png](JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled%202.png)

- Javascript engine gives us another special parameter whenever an execution context is created called as ***arguments.***
- Arguments is the [array like](https://stackoverflow.com/questions/29707568/javascript-difference-between-array-and-array-like-object) data structure of all the parameters passed to a function when it is invoked.
- In the ECMA5, there wan't any provision for default parameters, hence this particular pattern was used to mimic that behaviour.

```jsx
function greet(firstname, lastname, language) {
	language = language || "english"; //defualts to english if language is null/empty
	
	if (arguements.length === 0) {    //using the keyword arguments
		console.log("missing paramters");
	}
}
```

- **Function Overloading :**

    Function overloading doesn't exist in javascript as a consequence of it being a language which considers functions as first class. 

    There are design patterns that allow us to mimic the behaviour of function overloading.

 

## Dangerous Aside -

ALWAYS put your own semicolons in javascript to avoid errors introduced by the guess work of the syntax parser. 

```jsx
function getPerson() {
 
    return
    {
        firstname: 'Tony'
    }
    
}

console.log(getPerson());
```

The above function would result in returning nothing back, as after the return, if you use a carriage return (Enter key) the js engine automatically adds a semicolon and the statement below becomes redundant.

## Immediately Invoked Function Expressions (IIFE)

- Functions can be invoked right after they are defined in a function expression.
    - Example :

    ```jsx
    var greeting = function() {
    		return "Hello there" + " " + name;
    }("Tony");

    ```

    - Here, the variable greeting ultimately becomes a string as the function is immediately invoked resulting in the string - `Hello there Tony`.
- Javascript also allows us to write standalone IIFEs rather than them being part of a function expression.
- This feature isn't available by default as function expressions as a standalone line are not allowed, as they are confused with function statements and their expected syntax.
- So, we have to trick the syntax parser and not allow it to see the word `function` the first word in the line.
- One of the most elegant ways of achieving this is by wrapping the code into parenthesis (remember that parenthesis are used for expressions)
- Example of a standalone IIFE :

```jsx
var firstname = "Kenobi";
( function (name) {
	console.log("Hello there " + name);	
}(firstname));  //note that the invocation can happend either inside or outside the parenthesis
```

- One big advantage of IIFE's is the fact that any variable declared inside it sits on the execution contexts of the IIFE and thus does not interefere or get interfered by any other variables on the global execution context.
- Thus, in many frameworks, the code sits in an IIFE so as not to interfere with the global object.

    ![JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled%203.png](JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled%203.png)

    - As can be seen here, the variable *greeting* sits on the global execution context.

## Closures :

- Consider the code below -

    ```jsx
    function greet(whatToSay) {
        return function (name) {
            console.log(whatToSay + " " + name);
        }
    }

    var sayHi = greet("How are you");
    sayHi("Tony");
    ```

- The code above results in the output -

    `How are you Tony`

- When the line - `var sayHi = greet("How are you");` is executed, the function greet is invoked and the execution context is created, and after execution is popped off the stack.
- Now, when we execute the returned function, we still receive an output to a variable that should have been removed once the execution stack of the greet function was popped off.
- This happens because of **closures**.
- It's a feature of javascript which makes sure that functions always have a reference to their all it's variables via the scope chain.

![JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled%204.png](JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled%204.png)

- As you can see in the diagram above, the value of the variable `whattosay` still lives on even though the execution context of the function greet is popped off the stack.
- So, js functions always maintain a reference to all the variables that they are using.
- Another Example -

    ```jsx
    function buildFunctions() {

        var arr = [];
        for (i = 0; i < 3; i++) {
            arr.push(
                function() {
                    console.log(i);
                }
            )
        }

        return arr;
    }

    var functionArr = buildFunctions();
    functionArr[0]();
    functionArr[1]();
    functionArr[2]();
    ```

    - The output of the above function is `3`, `3`, `3`
    - That cause the functions do not have an `i` variable in their own scope and hence look up to the `closed` in variable and  it's value is 3.

    ![JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled%205.png](JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled%205.png)

    - Closures are useful in creating **function factories**
        - Example -

        ![JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled%206.png](JavaScript%20Understanding%20the%20weird%20Parts%203%203a0905ac22b24c18a9355741f652cb9d/Untitled%206.png)

## `call()`, `apply()` and `bind()`

- All functions have access to a `call()`, `apply()` and `bind()` function. When creating an expression function, we can change what `this` is referring to using bind:

```jsx
var person = {  firstname: "Tom"}var test = function(salutation){  console.log(salutation + ' ' + this.firstname)}test()// => Will return undefinedvar testBinded = test.bind(person)testBinded()// => Will return "tom"
```

- We can also keep the same function using call:

```jsx
test.call(person, "hi")// => Will return "hi tom"
```

- Apply can be used, the difference is that the arguments have to be wrapped into an array:

```jsx
test.apply(person, ["hi"])// => Will return "hi tom"
```

- Cool patterns using these functions:

```jsx
// Borowing a methodvar post = {  title: "How to be the best js person in the world",  author: "Tom",  getPostName: function(){    return this.title + " by: " + this.author  }}var post2 = {  title: "How to be a bad js person",  author: "Jack"}post.getPostName.bind(post2)()// => Will return "How to be a bad js person by: Jack"
```

```jsx
// function curryingfunction multiply(a,b){  return a*b}// we set the function to be a copy of multiply with 2 as the first argumentvar multiplyByTwo = multiply.bind(this, 2)multiplyByTwo(4)// => will return "8"
```