# Anonymous Functions & IIFE

A function without a name. The typical function in javascript is defined with the 'function' keyword and not invoked until called upon at a later point in time. 

Looks something like this:

``` javascript
function exampleFunction(x,y){
return x + y;
}
exampleFunction(1,2)
// 3
```

Functions can also be stored in a variable, called a function expression :

```javascript
let example = function(x,y){
return x + y;
}
let z = example(1,2);
// 3
```

Function expressions are a type of anonymous function as they have no name, but rather assigned to a variable.

## IIFEs

1. Self-Invoking Functions aka **Immediately Invoked Function Expressions** (IIFE)

   1. These functions are invoked (automatically) without being called.

   2. To do so, wrap the function in parentheses and follow it with (): 

      ```javascript
      (function(){
      	alert("Hello!")
      })();
      // "Hello"
      ```

   3. Or you can move the () within the parentheses wrapping the function like so: 

      ```javascript
      (function(){
      	alert("Hello!")
      }());
      // "Hello"
      ```

      

   4. This will encapsulate the code to the local scope

2. As a callback that only needs to run once and nowhere else. If this is the case, it makes readability easier as the reader doesn't need to search elsewhere for the function.

   ```javascript
   setTimeout(function(){
   	console.log("Hello");
   }, 1000);
   //returns "Hello" after 1 second
   ```

3. Arrow functions can also be written as IIFE:

   ```javascript
   x => x + 1;
   x => {return x + 1}
   (x,y) => x + y
   (x,y) => {return x + y}
   let add = (a, b)  => a + b;
   ```

## Use Cases

1. Closuress and Private data

   1. by wrapping a local variable in an IIFE, we can make it inaccessible from the outside. This creates a private state

      ```javascript
      const uniqueID = (function(){
      	let count = 0;
      	return function(){
      		++count;
      		return `id_${count}`;
      	};
      })();
      console.log(uniqueID()) //"id_1"
      console.log(uniqueID()) //"id_2"
      console.log(uniqueID()) //"id_3"
      ```

      

### Resources

- https://github.com/yangshun/front-end-interview-handbook/blob/master/contents/en/javascript-questions.md#can-you-describe-the-main-difference-between-a-foreach-loop-and-a-map-loop-and-why-you-would-pick-one-versus-the-other
- https://www.w3schools.com/js/js_function_definition.asp
- https://en.wikibooks.org/wiki/JavaScript/Anonymous_functions
- https://mariusschulz.com/blog/use-cases-for-javascripts-iifes

