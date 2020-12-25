## Readme for language part

#### VALUES, TYPES, AND OPERATORS
- NUMBERS
  - JavaScript uses a fixed number of bits, 64 of them, to store a single number value.
  - Those bits also store negative numbers, so one bit indicates the sign of the number. A bigger issue is that nonwhole numbers must also be represented. To do this, some of the bits are used to store the position of the decimal point.
- Special Numbers
  - There are three special values in JavaScript that are considered numbers but don’t behave like normal numbers.
  - The first two are Infinity and -Infinity, which represent the positive and negative infinities. 
  - next special number: NaN.  
  - NaN stands for “not a number,” even though it is a value of the number type. 
  - There is only one value in JavaScript that is not equal to itself, and that is NaN (“not a number”).
- STRINGS
  - whenever a backslash (\) is found inside quoted text, it indicates that the character after it has a special meaning.
  - Strings, too, have to be modeled as a series of bits to be able to exist inside the computer. The way JavaScript does this is based on the Unicode standard.
  - JavaScript’s representation uses 16 bits per string element, which can describe up to 2^16 different characters. 
  - But Unicode defines more characters than that—about twice as many, at this point. So some characters, such as many emoji, take up two “character positions” in JavaScript strings.
  - Strings cannot be divided, multiplied, or subtracted, but the + operator can be used on them. It does not add, but it concatenates—it glues two strings together.
  - When comparing strings, JavaScript goes over the characters from left to right, comparing the Unicode codes one by one. 
- UNARY OPERATORS
  - Not all operators are symbols. Some are written as words. One example is the typeof operator, which produces a string value naming the type of the value you give it.
  - The minus operator can be used both as a binary operator and as a unary operator.
- BOOLEAN VALUES
  - JavaScript has a Boolean type, which has just two values, true and false, which are written as those words.
- EMPTY VALUES
  - There are two special values, written null and undefined, that are used to denote the absence of a meaningful value. They are themselves values, but they carry no information.
- AUTOMATIC TYPE CONVERSION
  - When an operator is applied to the “wrong” type of value, JavaScript will quietly convert that value to the type it needs, using a set of rules that often aren’t what you want or expect. This is called type coercion. 
  - When comparing values of the same type using ==, the outcome is easy to predict: you should get true when both values are the same, except in the case of NaN. But when the types differ, JavaScript uses a complicated and confusing set of rules to determine what to do. 
  - In most cases, it just tries to convert one of the values to the other value’s type.
  - However, when null or undefined occurs on either side of the operator, it produces true only if both sides are one of null or undefined.
  - I recommend using the three-character comparison operators defensively to prevent unexpected type conversions from tripping you up. But when you’re certain the types on both sides will be the same, there is no problem with using the shorter operators.
- Short-Circuiting of Logical Operators
  - The logical operators && and || handle values of different types in a peculiar way. They will convert the value on their left side to Boolean type in order to decide what to do, but depending on the operator and the result of that conversion, they will return either the original left-hand value or the right-hand value.
  - The || operator, for example, will return the value to its left when that can be converted to true and will return the value on its right otherwise. This has the expected effect when the values are Boolean and does something analogous for values of other types.
  - The && operator works similarly but the other way around. When the value to its left is something that converts to false, it returns that value, and otherwise it returns the value on its right.
  - Another important property of these two operators is that the part to their right is evaluated only when necessary.In the case of true || X, no matter what X is—even if it’s a piece of program that does something terrible—the result will be true, and X is never evaluated. The same goes for false && X, which is false and will ignore X. This is called short-circuit evaluation.
  - The conditional operator works in a similar way. Of the second and third values, only the one that is selected is evaluated.      
#### PROGRAM STRUCTURE
- a program is built out of statements, which themselves sometimes contain more statements. Statements tend to contain expressions, which themselves can be built out of smaller expressions.
- Putting statements after one another gives you a program that is executed from top to bottom. You can introduce disturbances in the flow of control by using conditional (if, else, and switch) and looping (while, do, and for) statements.
- Bindings can be used to file pieces of data under a name, and they are useful for tracking state in your program. The environment is the set of bindings that are defined. JavaScript systems always put a number of useful standard bindings into your environment.
- Functions are special values that encapsulate a piece of program. You can invoke them by writing functionName(argument1, argument2). Such a function call is an expression and may produce a value.
- A fragment of code that produces a value is called an expression. 
- A statement stands on its own, so it amounts to something only if it affects the world. It could display something on the screen—that counts as changing the world—orit could change the internal state of the machine in a way that will affect the statements that come after it. These changes are called side effects. 
- In some cases, JavaScript allows you to omit the semicolon at the end of a statement. In other cases, it has to be there, or the next line will be treated as part of the same statement. The rules for when it can be safely omitted are somewhat complex and error-prone.
- To catch and hold values, JavaScript provides a thing called a binding, or variable : You should imagine bindings as tentacles, rather than boxes. They do not contain values; they grasp them—two bindings can refer to the same value. 
- A single let statement may define multiple bindings. The definitions must be separated by commas.
- The words var and const can also be used to create bindings, in a way similar to let.
- Binding name : the name must not start with a digit. A binding name may include dollar signs ($) or underscores (_) but no other punctuation or special characters.
- A function is a piece of program wrapped in a value. Such values can be applied in order to run the wrapped program.
- Executing a function is called invoking, calling, or applying it.
- When a function produces a value, it is said to return that value. Anything that produces a value is an expression in JavaScript, which means function calls can be used within larger expressions. 
- The function Number converts a value to a number.
- here are similar functions called String and Boolean that convert values to those types.
- Applying the ! operator will convert a value to Boolean type before negating it.  
- There is a special statement called break that has the effect of immediately jumping out of the enclosing loop.
- When continue is encountered in a loop body, control jumps out of the body and continues with the loop’s next iteration.
- The program will start executing at the label that corresponds to the value that switch was given, or at default if no matching value is found. It will continue executing, even across other labels, until it reaches a break statement. 
#### FUNCTIONS
- There are three ways to write your own functions. 
  - The function keyword, when used as an expression, can create a function value.
  ```
  // Declare g to be a function
  function g(a, b) {
    return a * b * 3.5;
  }
  ``` 
  - When used as a statement, it can be used to declare a binding and give it a function as its value. 
  ```// Define f to hold a function value
  const f = function(a) {
     console.log(a + 2);
  };
  ```
  - Arrow functions are yet another way to create functions.
  ```
  // A less verbose function value
  let h = a => a % 3;
  ```
- A return keyword without an expression after it will cause the function to return undefined. 
- Functions that don’t have a return statement at all, such as makeNoise, similarly return undefined.
- A pure function is a specific kind of value-producing function that not only has no side effects but also doesn’t rely on side effects from other code—for example, it doesn’t read global bindings whose value might change. A pure function has the pleasant property that, when called with the same arguments, it always produces the same value (and doesn’t do anything else). 
- Scopes
  - Each block creates a new scope. Parameters and bindings declared in a given scope are local and not visible from the outside.
  - Bindings declared with var behave differently—they end up in the nearest function scope or the global scope.
  - when multiple bindings have the same name—in that case, code can see only the innermost one. 
  - JavaScript distinguishes not just global and local bindings. Blocks and functions can be created inside other blocks and functions, producing multiple degrees of locality.
  - Each local scope can also see all the local scopes that contain it, and all scopes can see the global scope.This approach to binding visibility is called lexical scoping.
- JavaScript is extremely broad-minded about the number of arguments you pass to a function. If you pass too many, the extra ones are ignored. If you pass too few, the missing parameters get assigned the value undefined. 
- JavaScript does NOT natively support method overloading. So, if it sees/parses two or more functions with a same names it’ll just consider the last defined function and overwrite the previous ones.
- CLOSURE
  - The ability to treat functions as values, combined with the fact that local bindings are re-created every time a function is called, brings up an interesting question. What happens to local bindings when the function call that created them is no longer active?
  - This feature—being able to reference a specific instance of a local binding in an enclosing scope—is called closure. A function that references bindings from local scopes around it is called a closure.
  - A good mental model is to think of function values as containing both the code in their body and the environment in which they are created. When called, the function body sees the environment in which it was created, not the environment in which it is called.
  

#### DATA STRUCTURES: OBJECTS AND ARRAYS
- Array
  - Think of the index as the amount of items to skip, counting from the start of the array.
  - The elements in an array are stored as the array’s properties, using numbers as property names
  - Because you can’t use the dot notation with numbers and usually want to use a binding that holds the index anyway, you have to use the bracket notation to get at them.
  - The length property of an array tells us how many elements it has. This property name is a valid binding name, and we know its name in advance, so to find the length of an array, you typically write array.length because that’s easier to write than array["length"].
  - The push method adds values to the end of an array, 
  - the pop method does the opposite, removing the last value in the array and returning it.
  - The corresponding methods for adding and removing things at the start of an array are called unshift and shift.
  - Arrays, then, are just a kind of object specialized for storing sequences of things. If you evaluate typeof [], it produces "object"
  - Arrays have an includes method that checks whether a given value exists in the array.
  - Another fundamental array method is slice, which takes start and end indices and returns an array that has only the elements between them. The start index is inclusive, the end index exclusive.
  
- Object
  - Almost all JavaScript values have properties. The exceptions are null and undefined.
  - The two main ways to access properties in JavaScript are with a dot and with square brackets. Both value.x and value[x] access a property on value—but not necessarily the same property.
  - The difference is in how x is interpreted. When using a dot, the word after the dot is the literal name of the property.
  - When using square brackets, the expression between the brackets is evaluated to get the property name.
  - Whereas value.x fetches the property of value named “x,” value[x] tries to evaluate the expression x and uses the result, converted to a string, as the property name.
  - Property names are strings. They can be any string, but the dot notation works only with names that look like valid binding names.
  - So if you want to access a property named 2 or John Doe, you must use square brackets: value[2] or value["John Doe"].
  - Properties that contain functions are generally called methods of the value they belong to, as in “toUpperCase is a method of a string.”
  - Values of the type object are arbitrary collections of properties
  - Inside the braces, there is a list of properties separated by commas. 
  - Each property has a name followed by a colon and a value
  - Properties whose names are n’t valid binding names or valid numbers have to be quoted.
  - This means that braces have two meanings in JavaScript.
    - At the start of a statement, they start a block of statements. 
    - In any other position, they describe an object. 
  - Reading a property that don’t exist will give you the value undefined.
  - It is possible to assign a value to a property expression with the = operator. This will replace the property’s value if it already existed or create a new property on the object if it didn’t.
  - The delete operator cuts off a tentacle from such an octopus. It is a unary operator that, when applied to an object property, will remove the named property from the object. 
    -  example `delete anObject.left;`
  - The binary in operator, when applied to a string and an object, tells you whether that object has a property with that name. 
    - example `"console.log(left" in anObject)`  -> false  
  - The difference between setting a property to undefined and actually deleting it is that, in the first case, the object still has the property (it just doesn’t have a very interesting value), whereas in the second case the property is no longer present and in will return false.
  - To find out what properties an object has, you can use the Object.keys function.
  - There’s an Object.assign function that copies all properties from one object into another.
  - The object1 and object2 bindings grasp the same object, which is why changing object1 also changes the value of object2. They are said to have the same identity.
  - When you compare objects with JavaScript’s == operator, it compares by identity: it will produce true only if both objects are precisely the same value.
  - Comparing different objects will return false, even if they have identical properties.
  - There is no “deep” comparison operation built into JavaScript, which compares objects by contents
  - Values of type string, number, and Boolean are not objects, and though the language doesn’t complain if you try to set new properties on them, it doesn’t actually store those properties. As mentioned earlier, such values are immutable and cannot be changed.
  - because of a historical accident, typeof null also produces "object".
  - Use for…of to iterate over the values in an iterable and You can also use for…in to iterate over the index values of an iterable
  - you can use for in to get key of object without need of Object.keys
    - ```for (let key in oldCar) {
           console.log(`${key} --> ${oldCar[key]}`);
       }```and
    - ```for (let key of Object.keys(oldCar)) {
           console.log(`${key} --> ${oldCar[key]}`);
       }```   will give same result
  
    
  
- Destructuring
  -Note that if you try to destructure null or undefined, you get an error, much as you would if you directly try to access a property of those values.
- JSON
  - JSON looks similar to JavaScript’s way of writing arrays and objects, with a few restrictions. 
    - All property names have to be surrounded by double quotes, and
    - only simple data expressions are allowed—no function calls, bindings, or anything that involves actual computation.
    - Comments are not allowed in JSON.
  -JavaScript gives us the functions JSON.stringify and JSON.parse to convert data to and from this format.  
    
#### HIGHER-ORDER FUNCTIONS
- Functions that operate on other functions, either by taking them as arguments or by returning them, are called higher-order functions.
- Since we have already seen that functions are regular values, there is nothing particularly remarkable about the fact that such functions exist. The term comes from mathematics, where the distinction between functions and other values is taken more seriously.
- Higher-order functions allow us to abstract over actions, not just values.
- JavaScript’s charCodeAt method gives you a code unit, not a full character code. The codePointAt method, added later, does give a full Unicode character. So we could use that to get characters from a string. But the argument passed to codePointAt is still an index into the sequence of code units. So to run over all characters in a string, we’d still need to deal with the question of whether a character takes up one or two code units.
- a for/of loop can also be used on strings. Like codePointAt, this type of loop was introduced at a time where people were acutely aware of the problems with UTF-16. When you use it to loop over a string, it gives you real characters, not code units.
- If you have a character (which will be a string of one or two code units), you can use codePointAt(0) to get its code.

#### THE SECRET LIFE OF OBJECTS
-
-
-

#### keyword `this` deep dive
- The value of `this` is usually determined by a functions execution context. Execution context simply means how a function is called.
- It’s important to know that `this` may be different (refer to something different) each time the function is called.

#### Events
- `onMouseDown` will trigger when either the left or right (or middle) is pressed.
- `onMouseUp` will trigger when any button is released.
- `onMouseDown` will trigger even when the mouse is clicked on the object then moved off of it,
- while `onMouseUp` will trigger if you click and hold the button elsewhere, then release it above the object.
- `onClick` will only trigger when the left mouse button is pressed and released on the same object.
- In case you care about order, if the same object has all 3 events set, it's onMouseDown, onMouseUp, then  onClick. Each even should only trigger once though.
- onclick vs addEventListener
  - `onclick` is just a property, and like all object properties, if you write on more than once, it will be overwritten. 
  - With addEventListener() instead, we can simply bind an event handler to the element, and we can call it each time we need it without being worried of any overwritten properties.
- keyboard events are applicable on the elements that are in focus. To give any element a focus set `tabIndex` property on the element
  
  
  