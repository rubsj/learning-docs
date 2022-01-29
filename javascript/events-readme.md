### Ways of using web events
There are threee ways to handle events
 ##### Event handler properties
  ```
const btn = document.querySelector('button');

function bgChange() {
  const rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
  document.body.style.backgroundColor = rndCol;
}

btn.onclick = bgChange;
  ```
   -- this is old standard approach for setting event 
  ##### Inline event handlers
  ```
  <button onclick="bgChange()">Press me</button>
  function bgChange() {
  const rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
  document.body.style.backgroundColor = rndCol;
}
  ```
-- this is not recommended approach use only for very simple case


  ##### addEventListener() and removeEventListener()
  ```
  const btn = document.querySelector('button');

function bgChange() {
  const rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
  document.body.style.backgroundColor = rndCol;
}

btn.addEventListener('click', bgChange);
  ```
  -- this is the newest approach and recommended
  -- using this approach we can register and deregister the event listening
  -- multiple handlers can be registered for the same listeners
  -- in  below code second handler overwrites the first handler
  ```
  myElement.onclick = functionA;
  myElement.onclick = functionB;
  ```
  -- in the below code both handlers will be executed
  ```
  myElement.addEventListener('click', functionA);
myElement.addEventListener('click', functionB); 
  ```

### Event Bubbleing and capturing
- when an event is fired on an element that has parents elements, modern browsers run two phases - the `capturing` phase and the `bubbling` phase.
- in _Capturing_ phase 
    --the browser checks to see if the elements's outermost ancestor `<html>` has onClick event handler registered on it and runs it if so.
    -- then it moves on to the next element inside `<html>` and does the same thing and then the next one and so on until it reaches the element that was actually selected.
- in _Bubbling_ phase the opposite of capturing happens which is
    -- broswer checks if the selected element has an onclick event handler registered on it and runs it if so
    -- then it moves to the next immediate ancestor and does the same thing again on the next one and so on until it reaches the `<html>` element    
- :bulb: In case both types of events are registered the capturing phase is run first followed by bubbling phase    
- event handler by default registers event to be handled in bubbling phase.
- to register event in capture phase add third arument as `true` in `addEventListener` method. By default it is false indicating that event is registered in bubble phase