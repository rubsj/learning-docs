# React Funcdamentals

### excercise 1 `src/exercise/01.md` Basic JavaScript-rendered Hello World

1. javascript modules can have .mjs extension as well and infact thats the recommended extention
2. unbundled files can be deployed as well , they have longer first time load but can be faster for subsequent visit,
   however this could be beneficial for small applications only
3. strict type javascript code throws errors for previously silent errors
   1. like setting value to undefined
   2. not declaring variable with type
   3. performing operation on NAN
   4. strict type can enhance performance by making the variable types deterministi
4. refreshed memory for creating element using document.createElement
5. refreshed memory for adding attribute , adding class Names
6. learnt script type can be module for loading a module type script and that its defer by default
7. defer type will execute script only after html parsing is done
8. script can be async type as well , for async type script is loaded in parallel , howevr can be executed before html parsing is complete
9. module import can only be done for relative path full file name needs to be provided including extentiopn .js
10. modules are strict mode by default
11. modules are imported into the scope of scipt

### excercise 2 `src/exercise/02.md` Intro to raw React APIs

1. React.createElement returns Element which is DOM API's Element
2. React.createElement accesspts three params , type : a string of element type , props , an Object for props and rootContainerElement which can be element type, document or documentFragment type
3. if the props is empty you can set it to null or {} . This object can also contain children which if has only one value can be onject or needs to be array if more than on children esists
4. The children prop can be passed in the second param or as the third param
5. samples
   - single element`React.createElement('h1', {}, 'Hello World')`
   - with config `React.createElement('li', { className: 'container' }, 'Chocolate'),`
   - multiple ones

```
 React.createElement('ul', {},
      [
        React.createElement('li', {}, 'Chocolate'),
        React.createElement('li', {}, 'Vanilla'),
        React.createElement('li', {}, 'Banana')
      ]
    )

```

### excercise 3 `src/exercise/03.md` Using JSX

- JSX is syntactic sugar for `React.createElement(component, props, ...children)`
- JSX represents object as it boils down to createElement call which returns object
- JSX needs babel compiler to compile the code to JS
- We can use dot notation for jsx type. e.g `<MyComp.xyz />` is valid if `MyComp.xyz` is a component
- User defined component must be captitalized
- \_Choosing type at run time_To prvide the type at run time assign the type to capitalized variable sample
  ```
  const componentNames = {a: Apple ,  t: Twitter};
  const Mycomponent = ({componentNames}) =>{
    const DynamicComponentType = componentNames.a;
    return <DynamicComponentType/>
  }
  // this will create <Apple /> component
  ```
- Any JS expression can be passed as props , by surrounding it with {}
- String literals can be passed as prop directly without curly braces
- when prop is not provided with any value it defaults to true `<MyComp abc>` here it means `abc=true`
- Children in JSX
  1. string literals can be children
  2. JSX elements can be children
  3. JS expression can be used as children
  4. _Functions_ can be children too
  5. JSX ignores booleans , null and undefined and hence are valid children. Thy simply don't render
- React component can return an array of elements

### excercise 4 `src/exercise/04.md` Creating custom components

- learnt about propTypes that can be set on component name to provide type safety in react JS without needing typescript
- React creates dom element for a custom component vs when the same component is called via function i.e if we have custom component called message id its called like {Message} no additional DOM element will be created but if its called like `<Message>` or `using React.createElement(Message)` dom element will be created

### excercise 5 `src/exercise/05.md` Styling

- In react the `{{` and `}}` is actually a combination of a JSX expression and an object expression
- property names are camelCased rather than kebab-cased. This matches the style property of DOM nodes e.g. `style={{marginTop: 20, backgroundColor: 'blue'}}`
- className and style represent DOM object rather than HTMl attribute

### excercise 6 `src/exercise/06.md` Forms

  - In html to enable focus on input element on click of label you need to provide `for` attribute which binds the label with input. in react this for becomes `htmlFor` to avoid name collision with JS
  - `console.log` for html `event.target` prints dom element , to get the properties of this dome element use `console.dir`
  - `event.taget.form[0]` or `event.target[0]` can be used to retreive value of a property however to not depend on the order of element , provide name or id of element and then it can be retrieved using `event.target.elements.xyz.value`
  - form's onSubmit event causes browser refresh and appends input elements value to the url , this is browser's default property, for SPA we need to state event.preventDefault to stop the event from causing the page refresh
  - on click event react's synthetic event is triggered , to get the native event from the synthetic event you can use `event.nativeEvent`
  - Controlled component - being able to pass in value from outside to a component turns the component to controlled component . e.g `<input value={abc}>` iscontrolled input becuase value is being set from outside however `<input />` is uncontrolled

# React Hooks

### excercise 2 `src/exercise/02.md` - useEffects , useState, useReducer

- useState can be made to initialize value only once by using its lazy load feature which is to pass function to initialize the value

- if the setting state requires previous value that the state had in that case setState should be done via function to prevent stale data issue
- when using useReducer the issue of stale data does not come because state is the first argument of the reducer function. The only time stale data can be issue when setting staate depends on external values like props and in that case we should use useRef to get the current value of these external values
- useReducer's first method is the one which acts as the setter for state, the second param is for initializing the value and the third param is the function that can be used to initialize the value when it should be initialized only once , i.e. lazyloaded

- when doing the object destructuring we should initialize the object to empty object as shown below

```
function useLocalStorageState(
  key,
  defaultValue = '',
  {
    serialize = JSON.stringify,
    deserialize = JSON.parse
  } = {},
)
```

-react fragment can be assigned key when used in long form i.e. `<></>` replace with `<Fragment key={'abc}>{children}</Fragment>`

### excercise 4 `src/exercise/04.md` - useState tic tac toe
- 