# Classes

In OOP, a class is a template for creating objects with initial values and functions. This is useful because we often need to create many objects of the same kind, like users, items/goods or tasks like in a to-do list.

### Syntax

The basic syntax starts by using the **class** keyword, naming it, starting with a capital letter, then create your constructor with the containing methods: 

**note there are no commas between methods**

```javascript
class NewUser{
	constructor(){
		method1(){}
		method2(){}
		method3(){}
	}
}
```

By calling NewUser with the "new" keyword, the constructor will automatically intialize the object: 

```javascript
class NewUser{
	constructor(name, age){
		this.name = name;
		this.age = age;
}
	sayHi(){
		alert(this.name)
	}
}

//Usage:
let user = new NewUser("John")
user.sayHi();  // "John"
```

## What Makes Classes Different?

1. a function created by class is labeled witha special internal property: **[[FunctionKind]]:"classConstructor"**  which it would not get if created manually.
2. class methods are non-enumerable, setting the enumerable flag to false for all methods in the "prototype"
3. Classes always use "use-strict"

### Class Expressions

Like functions, we can define a class in another expression, pass it around, return it, assign it etc..

```javascript
let User = class {
  sayHi() {
    alert("Hello");
  }
};
```

They can be made dynamically: 

```javascript
function makeClass(phrase) {
  // declare a class and return it
  return class {
    sayHi() {
      alert(phrase);
    }
  };
}

// Create a new class
let User = makeClass("Hello");

new User().sayHi(); // Hello
```



### Getters/Setters

As with literal objects, classes can include getters/setters: 

```javascript
class User{
	constructor(name){
		this.name = name
	}
  get name(){
    return this._name
  }
  set name(){
    if(value.length < 4){
      alert("Name is too short");
      return;
    }
    this._name = value;
  }
}

let user = new User("John");
alert(user.name); // John

user = new User(""); // Name is too short.
```

**So, the name is stored in `_name` property, and the access is done via getter and setter.**

**Technically, external code is able to access the name directly by using `user._name`. But there is a widely known convention that properties starting with an underscore `"_"` are internal and should not be touched from outside the object.**



### Class Fields

**Old browsers may need polyfill as class fields are a recent addition to the language**

Class fields allow us to set properties (until now, we've only seen methods) which are set only on the individual object, not the prototype.

```javascript
class User {
	//the field
  name = "John";
}

let user = new User();
alert(user.name); // John
alert(User.prototype.name); // undefined
```



## Summary

The basic syntax looks like: 

```javascript
class MyClass {
  prop = value; // property

  constructor(...) { // constructor
    // ...
  }

  method(...) {} // method

  get something(...) {} // getter method
  set something(...) {} // setter method

}
```

















