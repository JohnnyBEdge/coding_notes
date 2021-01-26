

# Testing

"Automated testing means that tests are written separately, in addition to the code. They run our functions in various ways and compare the results with the expected."



## Behavior Driven Development (BDD)

It is three in one things: tests, documentation and examples

Before creating the code, imagine what the function should do and describe it. This is called **specification**, or spec, which contains descriptions of the use cases togther with tests for them

Ex:

```javascript
describe("pow", function(){
it("raises to n-th power", function(){
	assert.equal(pow(2,3),8);
});
});
```

- it describes the functionality
  - in this case, describing the function of 'pow'
- groups "workers", the "it" block
  - describes the particular use case, the second arg is the function that tests it
- Assert.equals compares the args and returns an error if not equal.

### Development flow

1. Initial spec is written, with tests for basic functionality
2. initial implementation created
3. run the testing framework (Mocha, Chai, Jest etc...)
   1. while func not complete, errors are displayed and we make corrections
4. add more use cases to spec
   1. go back to step 3
5. repeat until functionality is ready.

### Improving the Spec

Sometimes the tests pass, but the function can still be wrong. In this case, we need to add more use cases to it.

Here we cheat the code to make it work: 

```javascript
function pow(x, n) {
  return 8; // :) we cheat!
}
```

But now we improve it and it becomes hard to "cheat"

```javascript
describe("pow", function() {

  it("raises to n-th power", function() {
    assert.equal(pow(2, 3), 8);
    assert.equal(pow(3, 4), 81);
  });

});

```

However, this way of improving cause 'it' block to terminate, never running the second test. A better way to do this would be: 

```javascript
describe("pow", function() {

  it("2 raised to power 3 is 8", function() {
    assert.equal(pow(2, 3), 8);
  });

  it("3 raised to power 4 is 81", function() {
    assert.equal(pow(3, 4), 81);
  });

});
```

Now both tests will run, we will have two results, which gives us more informationt to work with. 

But this second example follows a more important rule:

**One test should check one thing**

Again, continouing with improving our tests, we can make sure the function works well by testing more values: us a loop to generate our "it" blocks for us:

```javascript
function pow(x, n) {
  let result = 1;

  for (let i = 0; i < n; i++) {
    result *= x;
  }

  return result;
}

describe("pow", function(){
	function makeTest(x){
		let expected = x * x * x;
		it(`${x} in the power 3 is ${expected}`, function(){
			assert.equal(pow(x,3), expected);
		});
	}
	for(let x = 1; x < 5; x++){
	makeTest(x);
	}
})
```



This is much better but what if we want to test other cases? Like the input type? Our helper tests: makeTest and for, don't need to be used in every case. So we can make a group with them using **describe**. Describe creates a subgroup of tests

```javascript
describe("pow", function() {

  describe("raises x to power 3", function() {

    function makeTest(x) {
      let expected = x * x * x;
      it(`${x} in the power 3 is ${expected}`, function() {
        assert.equal(pow(x, 3), expected);
      });
    }

    for (let x = 1; x <= 5; x++) {
      makeTest(x);
    }

  });

  // ... more tests to follow here, both describe and it can be added
});
```



## Summary

The spec can be used in three ways:

1. As **Tests** – they guarantee that the code works correctly.
2. As **Docs** – the titles of `describe` and `it` tell what the function does.
3. As **Examples** – the tests are actually working examples showing how a function can be used.



































