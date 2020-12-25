
## Concepts

       ``
-        

### General Angular
- we first must import the browser module as it “provides services that are essential to launch and run a browser app.”
- A workspace is a set of Angular applications and libraries. The angular.json file at the root level of an Angular workspace provides workspace-wide and project-specific (application or library) configuration defaults for build and development tools.
- in Angular terminology, a project can be both an application or a library. Both are stored in the projects folder of the workspace and configured in the central angular.json file.
- One thing to note is that you cannot specify the style preprocessor when generating a library. And there is no configuration related to it in the workspace configuration (angular.json file) for the libraries. The result is that you must specify the style each time you generate a component in a library, for example with the command ng generate component my-component --style=scss --project=tools.


### steps to create multiple project worksapce
- reference https://octoperf.com/blog/2019/08/22/kraken-angular-workspace-multi-application-project/


### Forms general concepts
- each input element must have the name attribute to be properly identified within the form.
- `novalidate` is used to disable the browser’s native form validation
- custom validator must implement, the `Validator` interface 
- `NgForm` Creates a top-level `FormGroup` instance and binds it to a form to track aggregate form value and validation status.
- As soon as you import the `FormsModule`, this directive becomes active by default on all `<form>` tags.  You don't need to add a special selector.
- `NgForm` is automatically attached to `<form>` tags (because of the default `NgForm` selector), which means we don't have to add an `ngForm` attribute to use `NgForm`
- any div or tag can be turned into form by adding directive `ngForm` see file `isolated-form.component.html` for sample
- In template driven forms, all `<form>` tags are automatically tagged as `NgForm`.  To import the `FormsModule` but skip its usage in some forms,
for example, to use native HTML5 validation, add the `ngNoForm` and the `<form>` tags won't create an `NgForm` directive. 
- In reactive forms, using `ngNoForm` is unnecessary because the `<form>` tags are inert. In that case, you would refrain from using the `formGroup` directive.
- when you work with forms, a FormControl is always created regardless of whether you use template driven or reactive forms

### Template Forms
- reference 
  - https://www.toptal.com/angular-js/angular-4-forms-validation
  - https://blog.ng-book.com/the-ultimate-guide-to-forms-in-angular-2/

-When the `FormsModule` is imported, Angular automatically detects a form HTML element and attaches the `NgForm` component to that element (by the selector of the NgForm component)
-by adding the `NgModel` directive, all inputs are registered to the `NgForm` component. 
- The two main functionalities offered by `NgForm` are:
  - Retrieving the values of all registered input controls
  - Retrieving the overall state of all controls
-  To expose the `NgForm`, we can add the following to the `<form>` element:
```
<form #myForm="ngForm">
     ..
   </form>
```
This is possible thanks to the `exportAs` property of the `Component` decorator.
- What if we want to have a sub-group of inputs from a specific context wrapped in a container and separate object in the values JSON ? 
  -The way to achieve this is by using the `ngModelGroup` directive.
- Template driven form does not have support for creating arrays like there is no (NgModelArray?). 
There are two approaches to handling it as a work around see `phone numbers` for approach one and `addresses` for approach 2 in `SimpleFormExample1Component`
- The `myForm.control.markAsTouched()` is used to make the form touched so we can display the errors at that moment. 
**The buttons don’t activate this property when clicked**, only the inputs.
- if we’d like to access the NgForm object in some method in the component, we can do it two ways
  - the NgForm can be passed as an argument to the function that will serve as a handler for the onSubmit event of the form. 
  ```<form #myForm="ngForm" (ngSubmit)="register(myForm)">
       …
     </form>
     register (myForm: NgForm) {
       console.log('Successful registration');
       console.log(myForm);
     }
     ```
  - The second way is to use a view query by adding the @ViewChild decorator to a property of the component
     ```@ViewChild('myForm')
        private myForm: NgForm;```   
- If you use template driven approach, the `FormControl` is created implicitly by the `NgModel` directive
        
### Reactive Forms
- With the reactive approach, you create a `control` yourself explicitly and use the `formControl` or the `formControlNam`e directive to bind it to a native control


### View as a core concept
- Angular application is a tree of components However, under the hood angular uses a low-level abstraction called view.
- There is a direct relationship between a view and a component — one view is associated with one component and vice verse
- A view holds a reference to the associated component class instance in the component property.
- All operations like property checks and DOM updates are performed on views
- Properties of elements in a View can change, but the structure (number and order) of elements in a View cannot. 
- Changing the structure of Elements can only be done by inserting, moving or removing nested Views via a ViewContainerRef. Each View can contain many View Containers.
- there’s no separate object for change detection and View is what change detection runs on
- Each view has a link to its child views through nodes property and hence can perform actions on child views.
- Each view has a state, which plays very important role because based on its value Angular decides whether to run change detection for the view and all its children or skip it.  e.g.
  - FirstCheck
  - ChecksEnabled
  - Errored
  - Destroyed

### Change Detection
- reference : https://blog.angular-university.io/how-does-angular-2-change-detection-really-work/
- A zone is nothing more than an execution context that survives multiple Javascript VM execution turns.  It's a generic mechanism which we can use to add extra functionality to the browser.
- Angular uses Zones internally to trigger change detection, but another possible use would be to do application profiling, or keeping track of long stack traces that run across multiple VM turns.
- The following frequently used browser mechanisms are patched to support change detection:
  - all browser events (click, mouseover, keyup, etc.)
  - setTimeout() and setInterval()
  - Ajax requests
- By default, Angular Change Detection works by checking if the value of template expressions have changed. This is done for all components. 
- By default, Angular does not do deep object comparison to detect changes, it only takes into account properties used by the template
- When using OnPush detectors, then the framework will check an OnPush component when any of its input properties changes, when it fires an event, or when an Observable fires an event
- but OnPush works by comparing references of the inputs of the component
- the triggering of event handlers inside the component itself also causes the on push change detector to trigger, independently than if the inputs have changed or not.
As we can see, if we take some precautions in the way we build our components, OnPush will work transparently with all sorts of component designs - components that receive data directly as inputs, that have observable inputs, or components that receive data only via constructor services, etc.
An OnPush change detector gets triggered in a couple of other situations other than changes in component Input() references, it also gets triggered for example:
-if a component event handler gets triggered
-if an observable linked to the template via the async pipe emits a new value
So if we remember to subscribe to any observables as much as possible using the async pipe at the level of the template, we get a couple of advantages:
-we will run into much less change detection issue using OnPush
-we will make it much easier to switch from the default change detection strategy to OnPush later if we need to
- Immutable data and @Input() reference comparison is not the only way to achieve a high performing UI with OnPush: the reactive approach is also an option to use OnPush effectively
- - reference : https://blog.angularindepth.com/a-gentle-introduction-into-change-detection-in-angular-33f9ffff6f10
- There are two main building blocks of change detection in Angular
  - a component view
  - the associated bindings
- Every component in Angular has a template with HTML elements. When Angular creates the DOM nodes to render the contents of the template on the screen, it needs a place to store the references to those DOM nodes.
   For that purpose, internally there’s a data structure known as View. It’s also used to store the reference to the component instance and the previous values of binding expressions.
- As the compiler analyzes the template, it identifies properties of the DOM elements that may need to be updated during change detection. For each such property, the compiler creates a binding. 
The binding defines the property name to update and the expression that Angular uses to obtain a new value.
- Each view has a state, which plays very important role because based on its value Angular decides whether to run change detection for the view and all its children or skip it.  e.g.
  - FirstCheck
  - ChecksEnabled
  - Errored
  - Destroyed
- Change detection is skipped for the view and its child views if ChecksEnabled is false or view is in the Errored or Destroyed state.  
- By default, all views are initialized with ChecksEnabled unless ChangeDetectionStrategy.OnPush is used.
- The states can be combined, for example, a view can have both FirstCheck and ChecksEnabled flags set.
- When an asynchronous event takes place, Angular triggers change detection on its top-most ViewRef, which after running change detection for itself runs change detection for its child views.
- This viewRef is what you can inject into a component constructor using ChangeDetectorRef token
``export class AppComponent {
       constructor(cd: ChangeDetectorRef) { ... } 
- The main logic responsible for running change detection for a view resides in checkAndUpdateView function. Most of its functionality performs operations on child component views.
  This function is called recursively for each component starting from the host component. It means that a child component becomes parent component on the next call as a recursive tree unfolds.
- When this function triggered for a particular view it does the following operations in the specified order:
  - sets ViewState.firstCheck to true if a view is checked for the first time and to false if it was already checked before
  - checks and updates input properties on a child component/directive instance  
  - updates child view change detection state (part of change detection strategy implementation)
  - runs change detection for the embedded views (repeats the steps in the list)
  - calls OnChanges lifecycle hook on a child component if bindings changed
  - calls OnInit and ngDoCheck on a child component (OnInit is called only during first check)     
  - updates ContentChildren query list on a child view component instance
  - calls AfterContentInit and AfterContentChecked lifecycle hooks on child component instance (AfterContentInit is called only during first check)
  - updates DOM interpolations for the current view if properties on current view component instance changed
  - runs change detection for a child view (repeats the steps in this list)
  - updates ViewChildren query list on the current view component instance
  - calls AfterViewInit and AfterViewChecked lifecycle hooks on child component instance (AfterViewInit is called only during first check)
  - disables checks for the current view (part of change detection strategy implementation
- The first thing is that onChanges lifecycle hook is triggered on a child component before the child view is checked and it will be triggered even if changed detection for the child view will be skipped. 
- The second thing is that DOM for a view is updated as part of a change detection mechanism while the view being checked. This means that if a component is not checked, the DOM is not updated even if component properties used in a template change. The templates are rendered before the first check. What I refer to as DOM update is actually interpolation update. 
  So if you have <span>some {{name}}</span>, the DOM element span will be rendered before the first check. During the check only {{name}} part will be rendered. 
- Another interesting observation is that state of a child component view can be changed during change detection. I mentioned earlier that all component views are initialized with ChecksEnabled by default, but for all components that use OnPush strategy change detection is disabled after the first check 
- So if you have the following components hierarchy: A -> B -> C, here is the order of hooks calls and bindings updates:
  A: AfterContentInit
  A: AfterContentChecked
  A: Update bindings
      B: AfterContentInit
      B: AfterContentChecked
      B: Update bindings
          C: AfterContentInit
          C: AfterContentChecked
          C: Update bindings
          C: AfterViewInit
          C: AfterViewChecked
      B: AfterViewInit
      B: AfterViewChecked
  A: AfterViewInit
  A: AfterViewChecked
-  ``class ChangeDetectorRef {
       markForCheck() : void
       detach() : void
       reattach() : void
       
       detectChanges() : void
       checkNoChanges() : void
     }`` 
     Component hierarchy
       AppComponent (aProp , bProp) -> AComponent -> AA Component
                                     |              |
                                     |             -> AC Component
                                      ->BComponent -> BB Component
                                                   -> BCComponent
     - detach
       - The first method that allows us manipulating the state is detach which simply disables checks for the current view   
       - if you detach a component all its children will get detached too i.e.  all its child components will not be checked as well.
       - since no change detection will be performed for the  branch components, DOM in their templates will not be updated as well 
     - reattach
       - As shown in the first part of the article OnChanges lifecycle hook will still be triggered for A`Component` if input binding `aProp` changes on the `AppComponent`.
       - The DOM won't be updated for component because of ngONChanges trigger unless the component is reattached
       - note that OnChanges hook is only triggered for the top-most component in the disabled branch, not for every component in the disabled branch.
       - The reattach method enables checks for the current component only, but if changed detection is not enabled for its parent component, it will have no effect. 
         It means that reattach method is only useful for top-most component in the disabled branch. 
     - markForCheck
       - We need a way to enable check for all parent components up to root component. And there is a method for it markForCheck:   
       - it simply iterates upwards and enables checks for every parent component up to the root. 
     - detectChanges
       - There is a way to run change detection once for the current component and all its children. This is done using detectChanges method.
       - This method runs change detection for the current component view regardless of its state  
     - checkNoChanges
       - This last method available on the change detector ensures that there will be no changes done on the current run of change detection.   

- operations Angular performs during change detection and their order. it can be found in `checkAndUpdateView` method inside the @angular/core module. 
 ```function checkAndUpdateView(view, ...) {
       ...       
       // update input bindings on child views (components) & directives,
       // call NgOnInit, NgDoCheck and ngOnChanges hooks if needed
       Services.updateDirectives(view, CheckType.CheckAndUpdate);
       
       // DOM updates, perform rendering for the current view (component)
       Services.updateRenderer(view, CheckType.CheckAndUpdate);
       
       // run change detection on child views (components)
       execComponentViewsAction(view, ViewAction.CheckAndUpdate);
       
       // call AfterViewChecked and AfterViewInit hooks
       callLifecycleHooksChildrenFirst(…, NodeFlags.AfterViewChecked…);
       ...
   }
   ```
   - First, it updates the input bindings for the child component.
   - Then it calls the OnInit, DoCheck, and OnChanges hooks, again, on the child component.
   - Then Angular performs rendering for the current component. 
   - And after that, it runs change detection for the child component.This means that it’ll basically repeat these operations on the child view. 
   - And finally, it calls theAfterViewChecked and AfterViewInit hooks on the child component to let it know that it’s been checked.
   - What we can notice here is that Angular calls the AfterViewChecked lifecycle hook for the child component after it’s processed the bindings of the parent component.
     :note you can update parent's binding's directly in life cycle hooks but you can not throw event which then changes binding in parent. It is still to be fully fathomed as to why ..probably by updating binding directly you are altering object stealthily and angular is not able to detect it...
 -  DOM updates is part of Angular’s change detection mechanism which mostly consists of three major operations:
    - DOM updates
    - child component `input` bindings updatees
    - query list updates
 -  Change detection mechanism is implemented as depth-first internally, but involves calling `ngDoCheck` lifecycle hooks on sibling components first.    
 -  When is `ngDoCheck` triggered   
    - Detect and act upon changes that Angular can’t or won’t detect on its own. Called during every change detection run, immediately after ngOnChanges() and ngOnInit().
    - You can see that ngDoCheck is called on the child component when the parent component is being checked. 
    - ``ComponentA
            ComponentB
                ComponentC``
      So when Angular runs change detection the order of operations is the following:
      ``Checking A component:
          - update B input bindings
          - call NgDoCheck on the B component
          - update DOM interpolations for component A
         
         Checking B component:
            - update C input bindings
            - call NgDoCheck on the C component
            - update DOM interpolations for component B
         
           Checking C component:
              - update DOM interpolations for component C``
        Now suppose we implement onPush strategy for the B component.The order of operations is the following:
        ``Checking A component:
            - update B input bindings
            - call NgDoCheck on the B component
            - update DOM interpolations for component A
           if (bindings changed) -> checking B component:
              - update C input bindings
              - call NgDoCheck on the C component
              - update DOM interpolations for component B
           
             Checking C component:
                - update DOM interpolations for component C``       
         - So with the introduction of the OnPush strategy we see the small condition if (bindings changed) -> checking B component is added before B component is checked. 
           If this condition doesn’t hold, you can see that Angular won’t execute the operations under checking B component.      
           However, the NgDoCheck on the B component is still triggered even though the B component will not be checked. 
           It’s important to understand that the hook is triggered only for the top level B component with OnPush strategy, and not triggered for its children — C component in our case.
  - Why do we need `ngDoCheck`?
    - You probably know that Angular tracks binding inputs by object reference. 
      It means that if an object reference hasn’t changed the binding change is not detected and change detection is not executed for a component that uses OnPush strategy.              
    - If we want to track an object or an array mutations we need to manually do that. 
      And if discover the change we need to let Angular know so that it will run change detection for a component even though the object reference hasn't changed.
    - Luckily, we can use the ngDoCheck lifecycle hook to check for object mutation and notify Angular using markForCheck method.
- Angular 2 separates updating the application model and reflecting the state of the model in the view into two distinct phases.
  The developer is responsible for updating the application model. 
  Angular, by means of change detection, is responsible for reflecting the state of the model in the view.
- the parent property update using output binding mechanism: is not performed as part of change detection.
  i.e. `<a-comp (updateObj)="value = $event"></a-comp>`
  where value is being updated for parent through child's event emitter will not be done by change detection
  It’s executed during the first phase of updating the application model before the change detection starts.
- unidirectional data flow defines the architecture of bindings updates that are processed during change detection.  
- there is no code in change detection mechanism in Angular that propagates a child property updates to its parent.
- The output bindings processing is performed outside of change detection and hence doesn’t transform unidirectional data flow into two-way data binding.
- During change detection Angular performs checks for each component which consists of the following operations performed in the specified order:
  - update bound properties for all child components/directives
  - call ngOnInit, OnChanges and ngDoCheck lifecycle hooks on all child components/directives
  - update DOM for the current component
  - run change detection for a child component
  - call ngAfterViewInit lifecycle hook for all child components/directives
     
### approaches for nesting forms
-  `NgModelGroup`, `FormGroupName`, and the `FormArrayName` directives can be used as containers and used for nesting withing or across components `NestedFormExample1Component` demostrates this
- CVA
- `NestableFormDirective` created in nestable-form.directive.ts

### Control Value Accessor
- A ControlValueAccessor acts as a bridge between the Angular forms API and a native element in the DOM.
- Any component or directive can be turned into `ControlValueAccessor` by implementing the `ControlValueAccessor` interface and registering itself as an `NG_VALUE_ACCESSOR` provider.
- ``interface ControlValueAccessor {
      writeValue(obj: any): void
      registerOnChange(fn: any): void
      registerOnTouched(fn: any): void
      setDisabledState?(isDisabled: boolean): void;
    }``
    - The writeValue method is used by formControl to set the value to the native form control.
    - The registerOnChange method is used by formControl to register a callback that is expected to be triggered every time the native form control is updated.
    - It is your responsibility to pass the updated value to this callback so that the value of respective Angular form control is updated.
    - The registerOnTouched method is used to indicate that a user interacted with a control.
    - `setdisabledState` is by the forms API when the control status changes to or from 'DISABLED'. Depending on the status, it enables or disables the appropriate DOM element.
- Angular implements default value accessors for all standard native form elements:    
  +------------------------------------+----------------------+
  |              Accessor              |     Form Element     |
  +------------------------------------+----------------------+
  | DefaultValueAccessor               | input, textarea      |
  | CheckboxControlValueAccessor       | input[type=checkbox] |
  | NumberValueAccessor                | input[type=number]   |
  | RadioControlValueAccessor          | input[type=radio]    |
  | RangeValueAccessor                 | input[type=range]    |
  | SelectControlValueAccessor         | select               |
  | SelectMultipleControlValueAccessor | select[multiple]     |
- All form directives inject value accessors using the token NG_VALUE_ACCESSOR and then select a suitable accessor. 
  If there is an accessor which is not built-in or DefaultValueAccessor it is selected. 
  Otherwise Angular picks the default accessor if it’s provided. And there can be no more than one custom accessor defined for an element.

### Unit Testing
- The following list provides a walkthrough of the import statements you’ll need for your tests:
  - `import { DebugElement } from '@angular/core'`;—You can use `DebugElement` to inspect an element during testing. 
  You can think of it as the native `HTMLElement` with additional methods and properties that can be useful for debugging elements.
  - import { ComponentFixture, fakeAsync, TestBed, tick } from '@angular/core/testing';
  - `ComponentFixture`—You can find this class in the `@angular/core` module. You can use it to create a fixture that you then can use for debugging.
  - TestBed—You use this class to set up and configure your tests. 
    Because you use TestBed anytime you want to write a unit test for components, directives, and services, it’s one of the most important utilities that Angular provides for testing.
  - fakeAsync—Using fakeAsync ensure that all asynchronous tasks are completed before executing the assertions.  
  - `import { By } from '@angular/platform-browser'`;—By is a class included in the `@angular/platform-browser` module that you can use to select DOM elements.
  - import { NoopAnimationsModule } from '@angular/platform-browser/animations';—You use the NoopAnimationsModule class to mock animations, which allows tests to run quickly without waiting for the animations to finish.
  - import { BrowserDynamicTestingModule } from '@angular/platform-browser-dynamic/testing';—BrowserDynamicTestingModule is a module that helps bootstrap the browser to be used for testing.
  - The TestBed class has a method called configureTestingModule. You can probably guess its purpose, which is to configure the testing module. 
    It’s much like the NgModule class that’s included in the app.module.ts file, which you can find at src/app. The only difference is that you only use configureTestingModule in tests.
  -  It takes an object that’s in the format of a TestModuleMetadata type alias
  - overrideModule ?? look into it
  - The fixture variable stores the component-like object from the TestBed.createComponent method that you can use for debugging and testing,
  - The component variable holds a component that you get from your fixture using the componentInstance property.
  
### Angular ng-template, ng-container , ng-content and ngTemplateOutlet 
- reference 
  - https://blog.angular-university.io/angular-ng-template-ng-container-ngtemplateoutlet/
  - https://www.freecodecamp.org/news/everything-you-need-to-know-about-ng-template-ng-content-ng-container-and-ngtemplateoutlet-4b7b51223691/
  - https://medium.com/claritydesignsystem/ng-content-the-hidden-docs-96a29d70d11b
-  the `ng-template` directive represents an Angular template: this means that the content of this tag will contain part of a template, 
   that can be then be composed together with other templates in order to form the final component template.
-  its not possible to apply two structural directives to the same element
   ```<div class="lesson" *ngIf="lessons" 
            *ngFor="let lesson of lessons">
         <div class="lesson-detail">
             {{lesson | json}}
         </div>
     </div>
      ```
      
     above code won't work to make it work it will need to be turned to
     ```<div *ngIf="lessons">
           <div class="lesson" *ngFor="let lesson of lessons">
               <div class="lesson-detail">
                   {{lesson | json}}
               </div>
           </div>
       </div>
      ```
    - In this example, we have moved the ngIf directive to an outer wrapping div, but in order for this to work we have to create that extra div element
    - is there a way to apply a structural directive to a section of the page without having to create an extra element?
    - Yes and that is exactly what the ng-container structural directive allows us to do!
    ```  <ng-container *ngIf="lessons">
      <div class="lesson" *ngFor="let lesson of lessons">
          <div class="lesson-detail">
              {{lesson | json}}
          </div>
      </div>
      </ng-container>
     ```
- the `ng-container` directive provides us with an element that we can attach a structural directive to a section of the page, without having to create an extra element just for that.
- `ng-container` directive can also provide a placeholder for injecting a template dynamically into the page.      
- We can also take the template itself and instantiate it anywhere on the page, using the `ngTemplateOutlet` directive
- We could add as many `ngTemplateOutlet` tags to the page as we would like, and instantiate a number of different templates.
  The value passed to this directive can be any expression that evaluates into a template reference
- all `ng-template` instances have access also to the same context on which they are embedded.  
  But each template can also define its own set of input variables. Actually, each template has associated a context object containing all the template-specific input variables.
- The core directives `ng-container`, `ng-template` and `ngTemplateOutlet` all combine together to allow us to create highly dynamical and customizable components.
- We can even change completely the look and feel of a component based on input templates, and we can define a template and instantiate on multiple places of the application.
- `<ng-content>` : They are used to create configurable components. This means the components can be configured depending on the needs of its user. This is well known as Content Projection. 
- Instead of every content projected inside a single `<ng-content>`, you can also control how the contents will get projected with the `select` attribute of `<ng-content>`. 
  It takes an element selector to decide which content to project inside a particular `<ng-content>`.  
- Sometimes you want different children of your wrapper to be projected in different parts of your template. 
  To handle this, `<ng-content>` supports a `select` attribute that lets you project specific content in specific places. 
  This attribute takes a CSS selector (my-element, .my-class, [my-attribute], …) to match the children you want. 
- If you include an `ng-content` without a `select` attribute, it will serve as a catch-all and will receive all children that did not match any of the other `ng-content` elements.
- `<ng-content>` does not “produce” content, it simply projects existing content, Because of this the lifecycle of the projected content is bound to where it is declared, not where it is displayed.
-  To let the wrapper control the instantiation of its children, we need to give it a template for the content, rather than the content itself.
- This can be done in two ways: using the `<ng-template>` element around our content, or using a structural directive with the “star” syntax, like *myContent.  
     
## Concepts to look into and create samples
- implement async validator
- implement registerOnValidatorChange for custom validation- 
- https://stackblitz.com/edit/depth-or-breadth-first for cdr
- dynamic component https://hackernoon.com/here-is-what-you-need-to-know-about-dynamic-components-in-angular-ac1e96167f9e
- Here is how to get ViewContainerRef before @ViewChild query is evaluated https://blog.angularindepth.com/here-is-how-to-get-viewcontainerref-before-viewchild-query-is-evaluated-f649e51315fb
- https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#8ce5
- Angular Forms Several Ways: Reactive, Nested, Across Routes https://dev.to/bitovi/angular-forms-several-ways-reactive-nested-across-routes-42g3
