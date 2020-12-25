# RXJS Concepts
### References
- https://rxjs-dev.firebaseapp.com/api   (RX JS official documentation)
- https://github.com/ReactiveX/rxjs (RXJS source code)
- 

## Concepts

#### Observer pattern
The pattern has two key players involved: a Subject and an Observer. A Subject is observed by an Observer. Typically, a Subject holds an internal list of Observers that should be notified when a change happens on the Subject. It is quite common that the Subject is a model and the Observers are some kind of UI component.
In short, Subjects should be able to:
- Hold a list of Observers
- Add an Observer
- Remove an Observer
- Notify all Observers when a change happens
The Observer, on the other hand, should only hold one property, and that is an update() method that can be called by a Subject when an update has occurred.

#### Creation Operator
 ##### Create 
- **refer** - https://www.learnrxjs.io/operators/creation/create.html
- **signature** - public static create(onSubscription: function(observer: Observer): TeardownLogic): Observable
- **description** - Creates a new Observable, that will execute the specified function when an Observer subscribes to it.
- **Notes** -
  - Because Observable extends class which already has defined static `create` function, but with different type signature, it was impossible to assign proper signature to `Observable.create`. Because of that, it has very general type `Function` and thus function passed to create will not be type checked, unless you explicitly state what signature it should have.     
  - When using TypeScript it is recommended to declare type signature of function passed to `create` as `(observer: Observer) => TeardownLogic`, where `Observer` and `TeardownLogic` are interfaces provided by the library.  
  - TeardownLogic interface is of type  Unsubscribable | Function | void;
  
##### of
- Emits the arguments you provide, then completes.
- Unlike [`from`](#from), it does not do any flattening and emits each argument in whole as a separate `next` notification.

##### from
- **signature**: from(ish: ObservableInput, mapFn: function, thisArg: any, scheduler: Scheduler): Observable
- Turn an array, promise, or iterable into an observable.
- Converts almost anything to an Observable.
- :bulb: This operator can be used to convert a promise to an observable!   
- :bulb: For arrays and iterables, all contained values will be emitted as a sequence!
- :bulb: This operator can also be used to emit a string as a sequence of characters!

#### bindCallback
- Converts a callback API to a function that returns an Observable.
- **signature**: bindCallback<T>(callbackFunc: Function, resultSelector?: Function | SchedulerLike, scheduler?: SchedulerLike): (...args: any[]) => Observable<T>
- Give it a function f of type f(x, callback) and it will return a function g that when called as g(x) will output an Observable.
- bindCallback is not an operator because its input and output are not Observables. The input is a function func with some parameters, the last parameter must be a callback function that func calls when it is done.
- The output of bindCallback is a function that takes the same parameters as func, except the last one (the callback). When the output function is called with arguments it will return an Observable. If function func calls its callback with one argument the Observable will emit that value. If on the other hand the callback is called with multiple values the resulting Observable will emit an array with said values as arguments.
- It is very important to remember that input function func is not called when the output function is, but rather when the Observable returned by the output function is subscribed. This means if func makes an AJAX request, that request will be made every time someone subscribes to the resulting Observable, but not before.

#### tap
- returns An Observable identical to the source, but runs the specified Observer or callback(s) for each item.
- Intercepts each emission on the source and runs a function, but returns an output which is identical to the source *as long as errors don't occur*.
- Returns a mirrored Observable of the source Observable, but modified so that the provided Observer is called to perform a side effect for every value, error, and completion emitted by the source. Any errors that are thrown in the aforementioned Observer or handlers are safely sent down the error path of the output Observable.
- If the Observable returned by tap is not subscribed, the side effects specified by the Observer will never happen. tap therefore simply spies on existing execution, it does not trigger an execution to happen like subscribe does.

#### mergeMap / flatMap
- :bulb: flatMap is an alias for mergeMap!
- Maps each value to an Observable, then flattens all of these inner Observables using mergeAll.
- Returns an Observable that emits items based on applying a function that you supply to each item emitted by the source Observable, where that function returns an Observable, and then merging those resulting Observables and emitting the results of this merger.
- :bulb: If only one inner subscription should be active at a time, try [`switchMap`](#switchMap)!
- :bulb: If the order of emission and subscription of inner observables is important, try [`concatMap`](#concatMap)!

#### switchMap
-

#### concatMap
- 


 

  




 




