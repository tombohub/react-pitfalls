# React pitfalls
Collection of react pitfalls as described in react documentation

---
<br>

## Describing the UI

<br>

## 1 - capitalize component name
React components are regular JavaScript functions, but their names must start with a capital letter or they won’t work!

*source: https://react.dev/learn/your-first-component#step-2-define-the-function*

<br>

## 2 - parentheses after return

Without parentheses, any code on the lines after return [will be ignored](https://stackoverflow.com/questions/2846283/what-are-the-rules-for-javascripts-automatic-semicolon-insertion-asi)!

```javascript
return (
  <div>
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  </div>
);
```

*source: https://react.dev/learn/your-first-component#step-3-add-markup*

<br>

## 3 - don't nest component definitions

Components can render other components, but **you must never nest their definitions**:

```javascript
export default function Gallery() {
  // 🔴 Never define a component inside another component!
  function Profile() {
    // ...
  }
  // ...
}
```

The snippet above is [very slow and causes bugs](https://react.dev/learn/preserving-and-resetting-state#different-components-at-the-same-position-reset-state). Instead, define every component at the top level:

```javascript
export default function Gallery() {
  // ...
}

// ✅ Declare components at the top level
function Profile() {
  // ...
}
```

When a child component needs some data from a parent, [pass it by props](https://react.dev/learn/passing-props-to-a-component) instead of nesting definitions.

*source: https://react.dev/learn/your-first-component#nesting-and-organizing-components*

<br>




## 4 - *aria-** and *data-** keep their dash

For historical reasons, [aria-*](https://developer.mozilla.org/docs/Web/Accessibility/ARIA) and [data-*](https://developer.mozilla.org/docs/Learn/HTML/Howto/Use_data_attributes) attributes are written as in HTML with dashes.

*source: https://react.dev/learn/writing-markup-with-jsx#3-camelcase-salls-most-of-the-things*

<br>


## 5 - camel case for style properties

Inline `style` properties are written in camelCase. For example, HTML `<ul style="background-color: black">` would be written as `<ul style={{ backgroundColor: 'black' }}>` in your component.

*source: https://react.dev/learn/javascript-in-jsx-with-curly-braces#using-double-curlies-css-and-other-objects-in-jsx*

<br>


## 6 - curly braces in props destructuring

**Don’t miss the pair of `{` and `}` curlies** inside of `(` and `)` when declaring props:

```javascript
function Avatar({ person, size }) {  // ...}
```

This syntax is called [“destructuring”](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Unpacking_fields_from_objects_passed_as_a_function_parameter) and is equivalent to reading properties from a function parameter:

```javascript
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...}
```

*source: https://react.dev/learn/passing-props-to-a-component#step-2-read-props-inside-the-child-component*

<br>

## 7 - no numbers after `&&`

**Don’t put numbers on the left side of `&&`.**

To test the condition, JavaScript converts the left side to a boolean automatically. However, if the left side is `0`, then the whole expression gets that value (`0`), and React will happily render `0` rather than nothing.

For example, a common mistake is to write code like `messageCount && <p>New messages</p>`. It’s easy to assume that it renders nothing when `messageCount` is `0`, but it really renders the `0` itself!

To fix it, make the left side a boolean: `messageCount > 0 && <p>New messages</p>`.

*source: https://react.dev/learn/conditional-rendering#logical-and-operator-*

<br>


## 8 - explicit return in arrow functions

Arrow functions implicitly return the expression right after `=>`, so you didn’t need a `return` statement:

```javascript
const listItems = chemists.map(person =>
  <li>...</li> // Implicit return!
);
```

However, **you must write `return` explicitly if your `=>` is followed by a `{` curly brace!**

```javascript
const listItems = chemists.map(person => { // Curly brace
  return <li>...</li>;
});
```

Arrow functions containing `=> {` are said to have a [“block body”.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#function_body) They let you write more than a single line of code, but you _have to_ write a `return` statement yourself. If you forget it, nothing gets returned!

*source: https://react.dev/learn/rendering-lists#filtering-arrays-of-items*

<br>


## 9 - don't use array's item index and random numbers as keys

You might be tempted to use an item’s index in the array as its key. In fact, that’s what React will use if you don’t specify a `key` at all. But the order in which you render items will change over time if an item is inserted, deleted, or if the array gets reordered. Index as a key often leads to subtle and confusing bugs.

Similarly, do not generate keys on the fly, e.g. with `key={Math.random()}`. This will cause keys to never match up between renders, leading to all your components and DOM being recreated every time. Not only is this slow, but it will also lose any user input inside the list items. Instead, use a stable ID based on the data.

Note that your components won’t receive `key` as a prop. It’s only used as a hint by React itself. If your component needs an ID, you have to pass it as a separate prop: `<Profile key={id} userId={id} />`.

*source: https://react.dev/learn/rendering-lists#why-does-react-need-keys*

---

<br>

## Adding Interactivity

<br>

## 10 - pass a function, not call a function

Functions passed to event handlers must be passed, not called. For example:

| passing a function (correct) | calling a function (incorrect) |
| --- | --- |
| `<button onClick={handleClick}>` | `<button onClick={handleClick()}>` |

The difference is subtle. In the first example, the `handleClick` function is passed as an `onClick` event handler. This tells React to remember it and only call your function when the user clicks the button.

In the second example, the `()` at the end of `handleClick()` fires the function _immediately_ during [rendering](https://react.dev/learn/render-and-commit), without any clicks. This is because JavaScript inside the [JSX `{` and `}`](https://react.dev/learn/javascript-in-jsx-with-curly-braces) executes right away.

When you write code inline, the same pitfall presents itself in a different way:

| passing a function (correct) | calling a function (incorrect) |
| --- | --- |
| `<button onClick={() => alert('...')}>` | `<button onClick={alert('...')}>` |

Passing inline code like this won’t fire on click—it fires every time the component renders:

```javascript
// This alert fires when the component renders, not when clicked!<button onClick={alert('You clicked me!')}>
```

If you want to define your event handler inline, wrap it in an anonymous function like so:

```javascript
<button onClick={() => alert('You clicked me!')}>
```

Rather than executing the code inside with every render, this creates a function to be called later.

In both cases, what you want to pass is a function:

- `<button onClick={handleClick}>` passes the `handleClick` function.
- `<button onClick={() => alert('...')}>` passes the `() => alert('...')` function.

[Read more about arrow functions.](https://javascript.info/arrow-functions-basics)

*source: https://react.dev/learn/responding-to-events#adding-event-handlers*

<br>

## 11 - events propagation

All events propagate in React except `onScroll`, which only works on the JSX tag you attach it to.

*source: https://react.dev/learn/responding-to-events#event-propagation*

<br>



