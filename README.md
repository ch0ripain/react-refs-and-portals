# React Refs and Portals
## 🔗 Refs 🔗
First, you need to import <code>useRef from 'react'</code>. A <code>ref</code> is like a persistent storage value that isn't affected by React's rendering cycle. This allows you to store values that persist over time, enabling various functionalities.

For example, if you assign a <code>ref</code> value to a <code>setInterval</code> (as shown in this project), you can later clean up that interval using <code>clearInterval</code> like this:
```javascript
const timer = useRef()
timer.current = setInterval(() => {}, 100)
clearInterval(timer.current)
```
Ref can also be use to perform built-in methods of html elements when we need.
For example to open an input file but instead of triggering that method in the normal way we can triggered using a ref on that input like this
```javascript
const filePickerRef = React.useRef()
<input ref={filePickerRef}  type="file"  /> //ref is an atribute already integrated on all html elements whilst using react
<button onClick={() => filePickerRef.current.click()}>Pick Image</button> //For triggering that click() method which is property of input type file we need to access to the ref and call it as we do in this line and that will triggered the method
```
> [!NOTE]
> Ref is property of one component instance and the rerender will never change his value

## 🌀 Portals 🌀
Portal in react is a way to make code be injected into the place we want in our HTML DOM. And we do this because if we dont for example in this project the ResultModal will appear inside some other elements bcz the place we are using that jsx is where then that jsx is injected in the final HTML.
To take control of that we can use { createPortal } which is a function from react-dom that works like this:
```javascript
import { createPortal } from 'react-dom'
//this createPortal function receive 2 arguments (renderable code, place we want to inject) so we need to this changes to use it
export default function ResultModal(...){
return createPortal(<p>code renderable </p> , document.querySelector('placeWeWant')
)}
```
And with that we are in the end showing all that content in the DOM position we want.

## ⚙️ useImperativeHandle ⚙️ 
This react hook allow us to expose some built-in html element method, that u see before, working in pair with refs and also can making a more readable and mantainable code:

```javascript
//We need to forward a ref from another component
import { forwardRef } from 'react'
//Change how our component is exported
const ResultModal = forwardRef(function ResultModal(props, ref){}) //forwardRef receive as 2nd argument the ref coming from the other component, righ next to props
const dialog = useRef()
useImperativeHandle(forwardedRef, () => {             //first argument is a ref where we need to use this methods (this ref is forwarded from another component. 
return { someRelatedName(){ dialog.current.click() }, //second argument need to return an object of functions, functions what in the end we just name the expose method and execute the original method inside of that function (we also can add some other logic)
}
<dialog ref={dialog} ...>...</dialog>
})
// So in the component we are forwarding that ref we need another ref to connect at that component and also for use this expose methods
//in TimerChallenge
const dialog = useRef()
//passing that ref to the component
<ResultModal ref={dialog} />
//Using the expose method in the component we are passing ref
someFunction(){
dialog.current.someRelatedName()
}
```
---
<p align="center">🌟 This project is a practice exercise I learned from the <a href='https://www.udemy.com/course/react-the-complete-guide-incl-redux/?couponCode=ST7MT110524'>Academind's React Course</a> 🌟</p>
<p align="center">🐸 I hope this README helps you in some way! 🐸</p>
