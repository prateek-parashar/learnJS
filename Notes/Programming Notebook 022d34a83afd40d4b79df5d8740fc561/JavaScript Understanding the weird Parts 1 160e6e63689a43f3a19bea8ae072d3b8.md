# JavaScript : Understanding the weird Parts #1

Course: https://www.udemy.com/course/understand-javascript/
Created: Jun 23, 2020
Tags: javascript

# Execution Contexts and Lexical Environments

## Glossary :

**Syntax Parsers** : The program that checks our code for validity of grammar.

**Lexical Environment** : Where something sits physically in our code and what surrounds it.              It's important in languages in which **where** you write something matters.

**Execution Context :** 

- It's a wrapper to help manage where the code is running. It so happens that there are a lot of lexical environments.
- Execution contexts help manage which one is currently being run at the moment.
- Every single javascript function has it's own execution context and it's own execution stack.
- Every execution context has a reference to it's own ***outer environment***.
- Execution contexts are created in 2 phases -
    - First when the functions and variables are [hoisted](https://www.notion.so/JavaScript-Understanding-the-weird-Parts-8a91e30452ee431b945cc01ba8a65a00#895d98ee16b24fb3aba1fb1d687e3867).
    - Second when the code is executed line by line.

**Single Threaded** : One command is executed at a time. Although a lot of things happen in the browser, javascript, for us programmers behaves as a single threaded language.

**Synchronous Execution :** Very similar to single threaded. It means that only one thing happens at a time and in the same order in which they appear.

**Asynchronous Execution :**  More than one actions at a time.

- Watch the following to understand *Event Loop* and *callbacks* →

    [https://www.youtube.com/watch?v=8aGhZQkoFbQ&pbjreload=101](https://www.youtube.com/watch?v=8aGhZQkoFbQ&pbjreload=101)

**Function Invoaction :** Function invocation is calling the function in javascript. Each function invocation creates it's own execution context and is places on the execution stack from where it's popped off upon completion.

**Variable Environment :**  Where the variables live and how they interact with each other.

**Name Value Pairs :**  A name value pair is just a map where a name maps to a value. A name can map to another name value pair. (Basically a JSON string).

Example:

```json
address : {
						flat number : 203
						area : {
											state : karnataka
											city : chickmagaluru
										}
						}
					
```

**Objects :** Objects in javascript are nothing but Name Value Pairs. So, pretty much JSON.

**Scope :** Where the variables is available in the code.and if it's truly the same variable or a copy.

## Summary  Points:

![JavaScript%20Understanding%20the%20weird%20Parts%201%20160e6e63689a43f3a19bea8ae072d3b8/Untitled.png](JavaScript%20Understanding%20the%20weird%20Parts%201%20160e6e63689a43f3a19bea8ae072d3b8/Untitled.png)

**Global Object :** 

- In the very beginning the Global execution environment is created
- The Global execution environment gives us a global object.
- This global object in a browser is the **window** object.
- Any new variable created in the global execution environment is global in nature and can be accessed from anywhere.
- Global in javascript means *outside of a function*.

***this* :**

- It is a variable handed to us by the global execution context.
- In the browser, at the global level, *this* points to the window object.

**Hoisting :** 

- The javascript code is parsed through to scan for variable declarations and function definitions.
- After this process, while creation of the execution context, memory is reserved for variables (although their value is not set) and the functions are saved up in memory.
- It is due to hoisting that a code like this -

    ```jsx
    b();
    console.log(a);

    var a = 'Hello World!';

    function b() {
        console.log('Called b!');
    }
    ```

    would result in an output like this - 

    `Called b!
    undefined`

    because due to hoisting, the function definition is loaded into memory, but the variable value is not stored, although, note that the variable is identified because the variable declaration is made. 

*u**ndefined* :** 

- *undefined* is a special keyword in javascript that variables receive during the creation phase of the global execution context.
- Not to be confused with *not defined* which is an error thrown by the compiler when the variable is never declared at all.
- We can manually set variable values to *undefined* by code or test for it.

    ```jsx
    var a = "hello";
    if (a === undefined) {
    	console.log("a is undefined");
    }
    ```

**Outer Environment :** 

- Consider the following code -

    ```jsx
    function b() {
    	console.log(myVar);
    }

    function a() {
    	var myVar = 2;
    	b();
    }

    var myVar = 1;
    a();
    ```

    Here we see the diagram representing the outer environments of both the function's execution context.

![JavaScript%20Understanding%20the%20weird%20Parts%201%20160e6e63689a43f3a19bea8ae072d3b8/Untitled%201.png](JavaScript%20Understanding%20the%20weird%20Parts%201%20160e6e63689a43f3a19bea8ae072d3b8/Untitled%201.png)

- Every execution environment has a reference to an outer environment.
- The outer environment variable pointer is decided by where the function sits in the program physically (basically the *[lexical environment](https://www.notion.so/JavaScript-Understanding-the-weird-Parts-8a91e30452ee431b945cc01ba8a65a00#c7c2d2f019cc4869b4cb896739f1e86c)* of a function decides it's outer environment.
- The outer environment for the global execution environment points to *null*.
- Function invocation has no role in deciding the outer variable of the execution context.
- Here, in the example above, the output is - `1`

    Because the outer reference for both function a and b is to the global executions.

    If the code were to be rewritten to change the lexical environment of the function `b` :

    ```jsx
    function a() {
        
        function b() {
            console.log(myVar);
        }
        
    	b();
    }

    var myVar = 1;
    a();
    ```

    The output would be changed to `2` following the rules we have just established

**Scope Chain :**

- If a variable is not defined in the current execution context, javascript moves up the chain of outer environment references to look for the variable definition.