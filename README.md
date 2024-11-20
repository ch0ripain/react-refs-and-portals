# React Refs and Portals
## 🔗 Refs 🔗
First, you need to import <code>useRef from 'react'</code>. A <code>ref</code> is like a persistent storage value that isn't affected by React's rendering cycle. This allows you to store values that persist over time, enabling various functionalities.

For example, if you assign a <code>ref</code> value to a <code>setInterval</code> (as shown in this project), you can later clean up that interval using <code>clearInterval()</code> like this:
```javascript
const timer = useRef()
timer.current = setInterval(() => {}, 100)
clearInterval(timer.current)
```
A ref can also be used to call built-in methods of HTML elements when needed. For example, to open a file input, instead of triggering the method in the usual way, we can trigger it using a <code>ref</code> on the input element, like this:
```javascript
const filePickerRef = React.useRef();
<input ref={filePickerRef} type="file" />  {/* The ref attribute is built-in for all HTML elements in React */}
<button onClick={() => filePickerRef.current.click()}>Pick Image</button>  {/* Triggering the click() method of the file input using the ref */}
```
In this example, the <code>ref</code> attribute is used to reference the input element. By calling <code>filePickerRef.current.click()</code>, we can programmatically trigger the file picker dialog instead of relying on the usual user interaction.
> [!NOTE]
> A <code>ref</code> is a property of a component instance, and its value will not change on re-renders.

## 🌀 Portals 🌀
A portal in React allows you to render content in a different part of the DOM than where the component is defined. This is useful because, without it, for example, in this project, the <code>ResultModal</code> might appear inside other elements due to the position of the JSX in the component tree.

To control where this content is rendered, we can use <code>{ createPortal }</code>, a function from <code>react-dom</code>, which works like this:
```javascript
import { createPortal } from 'react-dom' //2 arguments => (renderable code, target DOM element where you want to inject)
export default function ResultModal(...){
return createPortal(
    <p>Renderable content</p>, 
    document.querySelector('placeWeWant') // Target element where the content will be injected
  );}
```

## ⚙️ useImperativeHandle ⚙️ 
The <code>useImperativeHandle</code> hook allows us to expose certain built-in HTML element methods (like those we’ve seen before) when working with refs. This hook helps make the code more readable and maintainable by controlling what functions or properties are accessible outside of the component.

```javascript
//We need to forward a ref from another component
import { forwardRef } from 'react'

//Change how our component is exported
const ResultModal = forwardRef(function ResultModal(props, ref){}) //forwardRef receive as 2nd argument the ref coming from the other component, right next to props
const dialog = useRef()
useImperativeHandle(forwardedRef, () => {                          //first argument is the ref where then we can call this exposed methods
return { someRelatedName(){ dialog.current.click() },             //The second argument of useImperativeHandle should return an object of functions. In this object, each function acts as an "exposed method." These methods are named by you, and within each one, you can execute the original method (e.g., dialog.current.click()) or add additional logic as needed.
}
<dialog ref={dialog} ...>...</dialog>
})
// In the component where we are forwarding the ref (which gives access to the exposed methods), we need a separate ref inside the parent component to actually call those methods.
//TimerChallenge.jsx
const dialog = useRef()
//pass the ref to the component
<ResultModal ref={dialog} />
//Use the expose method
someFunction(){
dialog.current.someRelatedName()
}
```
---
<p align="center">🌟 This project is a practice exercise I learned from the <a href='https://www.udemy.com/course/react-the-complete-guide-incl-redux/?couponCode=ST7MT110524'>Academind's React Course</a> 🌟</p>
<p align="center">🐸 I hope this README helps you in some way! 🐸</p>
