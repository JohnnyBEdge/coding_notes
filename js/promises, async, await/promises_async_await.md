# Promises, Async/Await

## Need to know prior...

### Callbacks

Asynchronous actions- actions that intitate now, finish later

 - ex: setTimeout, scripts and modules

Here is a function that creates and loads a script: 

```javascript
function loadScript(src){
  //creates the script element
  let script = document.createElement('script');
  //adds the source
  script.src = src;
  //appends it to the head of the document
  document.head.append(script)
}
```

Now to run it we simply call the function and supply it with a source: 

```	
loadScript('/my/script.js')
//the code loads nows, but runs later after it the function has completed
//and any code below this, doesn't wait for it to complete
```

But what if loadscript also included other functions we wanted to run? The browser hasn't had time to load them, so if they are called elsewhere in the code, it will fail.

This is where a **callback function** can be useful:

```javascript
function loadScript(src, callback){
	let script = document.createElement(script);
  script.src = src;
  
  script.onload = () => callback(script);
  document.head.append(script)
}

//now if we want to call a new function within the script, we can write it in the callback

loadScript('/my/script.js', function(){
  //this runs after the script it loaded:
  newFunction();
})
```

This is called  **callback-based** style of asynchronous coding and is fine until you want to do more, like load another script, and another and another. Don't forget we need to add error handling in case the script fails to load. In **error first callbacks**, convention is to reserve the first arg of the callback for the error, second arg for the function that runs if successful. All of this can get messy:

```	javascript
loadScript('1.js', function(error, script) {

  if (error) {
    handleError(error);
  } else {
    // ...
    loadScript('2.js', function(error, script) {
      if (error) {
        handleError(error);
      } else {
        // ...
        loadScript('3.js', function(error, script) {
          if (error) {
            handleError(error);
          } else {
            // ...continue after all scripts are loaded (*)
          }
        });

      }
    });
  }
});
```

and is called **callback hell.** Difficult to read and difficult to manage. 

The solution...

## Promises

Lets start with an example:

```javascript
let promise = new Promise(function(resolve, reject){
//function tasks
})
```

Here, using a constructor, we create a new promise, which takes a function, called the **executer** that takes two arguments, resolve and reject, then has the producing code. 

The executer runs automatically when created, if successful, calls resolve and if not, calls reject.

The object returned by new Promise has two properties:

1. state

   - initially **pending**, then either **fullfilled** or **rejected**

2. result

   - initially **undefined** then either the **value** if resolved or **error** if rejected.

     ![image-20201201105043950](/Users/john_home/Library/Application Support/typora-user-images/image-20201201105043950.png)

     The executer should perform jobs that take time (like API calls), then call resolve/reject depending on the results and therefore changing the state of the promise object.

     Also, the executer should only call either resolve or reject. Once one is called, any additional calls will be ignored. Resolve and reject will only take one arg 

     ```javascript
     let promise = new Promise(function(resolve, reject) {
       resolve("done");
     
       reject(new Error("…")); // ignored
       setTimeout(() => resolve("…")); // ignored
     });
     ```



### Consumers

The promise object returned is the link between the executer function and the consuming functions that receive the results (err or result). They consuming these via methods such as .then, .catch or .finally.

**then**

​	syntax :

```javascript
promise.then(
	function(result){handle result},
	function(err){ handle err}
)
```

- the first arg is the function that runs when the promised is solved and receives the result

- the second arg if a fx that funs when the promise is rejected and receives the error

  Example:

  ```javascript
  let promise = new Promise(function(resolve, reject){
  	setTimeout(()=> resolve('done!'), 1000)
  })
  
  // resolve runs first, then:  .then
  promise.then(
  	result => alert(result); //shows after 1 second 
    error => alert(error) //and this wont run
  )
  ```

**Attach handlers to settled promises**

If a promise is pending, .then/catch/finally will wait for it. 

Promises give us better code flow and more flexibility.



## Async/Await

Putting "async" before a function means the function will always return a promise and wraps non-promises in it.

"Await" makes JS wait for the promise to settle, then returns the result.

```javascript
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000)
  });

  let result = await promise; // wait until the promise resolves (*)

  alert(result); // "done!"
}

f();
```

The fx pauses on the await, and resumes when settled. 

Await suspends the function execution until the promise settles then resumes with the promise result. This doesnt cost and CPU resources bc JS can do other jobs in the meantime.















































