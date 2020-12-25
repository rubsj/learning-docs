Referece : 
- Javascript Unit Testing, by Mark Trostler 
[![Javascript Unit Testing, by Mark Trostler](http://akamaicovers.oreilly.com/images/9781771374569/cat.gif)](https://www.safaribooksonline.com/library/view/title/9781771374569//)

#### key terms
Stubs - stubs are functions that return dummy data
Spies -  these are stubs + metadata . they return stubbed data and meta data like how many times its called etc
Mock Objects - simulate behavior of complex , real objects.
Stub or Spies are typically functions and Mocks are full objects
Fixtures - not a function or an object. They are fake data your test may need. Can be DOM/ JSON data. and they usually need to be setup and torn down after each test or set of tests or suite

### Challenges of JS unit testing
- JS can have both server side code as well as client side code , requiring both kind of infrastrcture for testing as well
- JS needs HTML Glue for testing
- Side effects are very common (CSS, DOM , AJAX)
- Synchronous and Asynchronous code both exists

### Some unique Jasmine matchers 
- The 'toBe' matcher compares with ===
- The 'toEqual' matcher works for simple literals and variables
- The 'toEqual' matcher should work for objects
- The 'toContain' matcher works for finding an item in an Array"
- The 'toContain' matcher also works for finding a substring
- The 'toThrow' matcher is for testing if a function throws an exception
- The 'toThrowError' matcher is for testing a specific thrown exception
- to test type of object use jasmine.any(Object) i.e jasmine.any matches any value and tests its instance type
` expect({}).toEqual(jasmine.any(Object));`
- 'jasmine.anything' expect value being compared is not null or undefined
- Asymmetric matching is the ability to do partial match of objects/array usig isEqual matcher in compbination with jasmine.anything , jasmine.arrayContaing etc . Full list of these assymetric methods are https://github.com/jasmine/jasmine/tree/master/src/core/asymmetric_equality 
- to use asymmetricMatch property
    - An asymmetric matcher needs to be a function.
    - The asymmetric matcher needs to expose a field asymmetricMatch, which is a function that takes two arguments. The first argument is the test element (the one you wrap inside expect); the second argument is customTesters, which is another way to achieve custom comparison. For the scope of this article, we will not use the second argument.
    - You can provide optional reporting when test fails by providing a jasmineToString method.
``` 
function normalizeNumber(number) {
  return number.split('').filter(digit => '0123456789'.includes(digit)).join('');
}
function phoneNumberMatching(expectedNumber) {
  const matcher = function() {};
  
  matcher.asymmetricMatch = function(testNumber) {
    return normalizeNumber(testNumber) === normalizeNumber(expectedNumber);
  }

  return matcher;
} 
....
 expect('945-123-1234').toEqual(phoneNumberMatching('9451231234'));

```

or
```
phoneNumberMatcher = {
  asymmetricMatch :  function(actual){
    return actual.match(/abc/);
}
}
```
see asymmetric.matcher.example1.spec.js for reference
- How to create custom matchers in jasmine ? refer toBeTypeMatcher example in original book

### Spies
- spy can be set by calling spyon(obj , 'property')
- this spy can later on be referenced by calling obj.property
- This spy only spies and does not actually calls the funcion
- to actually call the function use and.callThrough
- to call fake function and not the actual function use and.fake
- to only spy on function and return a fixed value use and.raturn
- it can also be used to throw error by and.toThrowError
- note to use toThrowError matcher wrap the call in function
- to reset spy to original state use and.stub() which will essentially make it like new spy
- spies can be created on a non existent function by saying  this.totallyFake=jasmine.createSpy('functionName');
totallyFake here is a funcion on which all the spy functionality can be applied
- to toallyFake an objec with fake functions that we have dependency on we use this.fakeSpyObj = jasmine.createSpyObj('object' , ['func1' , 'func2'])

### Sharing variables across tests
- you can create global variable by decaring the variable right after describe. Any change in this variable by either beforeeach/ it functions will be shared with all tests
- if you need a variable that is gloabl for beforeeahc/beforeall types of function and not changed by it function then you should attach those variables to this object in beforeeahc/beforeall
- there is jasmine level this object which is accessed directly below describe which should not be uddled with. Update this at the beforeeach/aftereach functions only