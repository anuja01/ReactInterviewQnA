# ReactInterviewQnA
Questions and answers to React Interview Questions

### React Documentation Guide
## Main Concepts Section
### JSX
1. What is JSX and the use of it in React JS   
   JSX - Javascript XML   
   JSX is used to describe what UI should look like    
   (Use HTML within Javascript)   
   JSX prevents Injection attacks - XSS (cross-site-scripting)   
      > HOW ?   
      XSS = executable code injection. react injects only non-code (eg HTML-attributes/classes and text) unless you do hacky things. if you try to inject code, it will simply be rendered as text, but not executed.
      https://stackoverflow.com/questions/57746377/react-documentation-jsx-prevents-injection-attacks

   Babel compiles JSX down to ```React.createElement()``` calls.

### Rendering Elements
- ### Notes:
- Unlike browser elements, React elements are plain objects, and cheaper to create.
- React elements are ```immutable```. Once created can't change children or attributes.
- To update the UI, create a new element, and pass it to ```ReactDOM.render()```.
- React only updates what's necessary. (Compare what's changed)   

### Components and Props
2. What are components   
   Components let you split the UI into Independant, reusable pices   

3. What are the types of components
   >1. Functional Components   
  A Javascript function
   >2. Class Components   
   uses ES6 class to define a component
4. What are ```props```    
   Props are properties getting passed to component from parent   
   Props must <b>never</b> be modified   
5. Why functional components are Pure functions   
   Because they never attempt to change their input(props), and always return the same output for same input (no side effects)   
   <b>All React components must act like pure functions with respect to their props.</b>   
   NOTE: State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.

### State and Lifecycle
[Lifecycle](https://miro.medium.com/max/2000/1*lINPzI9FsJnay2_fm4vmzA.png "Lifecycle")

Notes:   
Always use `setState()` to modify the local state   
As State updates may be Asynchronous, add a callback to `setState()` for immideate state updates   
eg:
```javascript
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```
### Handling Events

6. What are the syntax difference when handling events?   
   React events are named using camelCase, rather than lowercase.   
   With JSX you pass a function as the event handler, rather than a string.
   eg:
   ```html
    <button onClick={activateLasers}>
        Activate Lasers
    </button>
   ```
7. How do you prevent the default behaviour of triggered event in React?   
   Need to call `preventDefault` explicitly.   
   ```javascript
    function handleClick(e) {
        e.preventDefault();
        console.log('The link was clicked.');
   }
   ```
8. Why <b>binding</b> is necessary JSX callbacks and what are the available options?   
   In Javascript clas methods are not bound by default (`this.` means lexical scope)
   >available options   
   i. explicit binding using `bind` in constructor
   ```javascript
    class Toggle extends React.Component {
    constructor(props) {
        super(props);
        this.state = {isToggleOn: true};

        // This binding is necessary to make `this` work in the callback
        this.handleClick = this.handleClick.bind(this);
    }

    render() {
        return (
        <button onClick={this.handleClick}>
            {this.state.isToggleOn ? 'ON' : 'OFF'}
        </button>
        );
        }
    }
   ```
   ii. Using public class fields syntax   
   ```javascript
    class LoggingButton extends React.Component {
        // This syntax ensures `this` is bound within handleClick.
        // Warning: this is *experimental* syntax.
        handleClick = () => {
            console.log('this is:', this);
        }

        render() {
            return (
            <button onClick={this.handleClick}>
                Click me
            </button>
            );
        }
    }
   ```
   iii. use arrow functions in the callback (arrow functions by default bind to lexical scope)  
   NOTE: using arrow functions will create a different callback each time button renders, and it can be a issue if callback passes props to child components as they'll re-render too.

   MORE: with arrow functions, you need to explicitly pass (e) to pass the event, but with `.bind` event is automatically forwarded.   
   ```html
    <!--arrow function -->
    <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
    <!--usual function with .bind-->
    <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
   ```

### Lists and Keys   
9. Why do you need to specify a key attribute when rendering list of elements?   
   Keys help React identify which items have changed, are added, or are removed which optimize the re-rendering

10. Why do we call `super(props)` in constructor, and can we avoid it?  
    `super(props)` will call the constructor of parent class.(`React.Component`)   
    Otherwise can't use `this.` within the construcotr.   
    We can use a class field to set the initial state, so there's no need to call the constructor.   
    `state = { isOn: true}`
11. What does it mean by Render Props?
    A component with a render prop takes a function that returns a React element and calls it instead of implementing its own render logic.

    Avantages:    
    Efficiently reuse code.

12. Name types of static type checking
    1. Flow - a static type checker for js
    2. use Typescript

13. Explain **Strict Mode** in React   
    StrictMode is a tool for highlighting potential problems in an application. It activates additional checks and warnings for its descendants.   
    eg:
    1. identify unsafe lifecycle methods
    2. warning about legacy APIs
    3. Detecting unexpected side effects
    4. Warning about depricated findDomNode usage
    5. Detecting legacy context API