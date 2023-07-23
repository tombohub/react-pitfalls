# React pitfalls
Collection of react pitfalls as described in react documentation

---

### 1 - capitalize component name
React components are regular JavaScript functions, but their names must start with a capital letter or they won’t work!

*source: https://react.dev/learn/your-first-component#step-2-define-the-function*

---

### 2 - parentheses after return

Without parentheses, any code on the lines after return [will be ignored](https://stackoverflow.com/questions/2846283/what-are-the-rules-for-javascripts-automatic-semicolon-insertion-asi)!

```javascript
return (
  <div>
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  </div>
);
```

*source: https://react.dev/learn/your-first-component#step-3-add-markup*

---

### 3 - don't nest component definitions

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

---




### 4 - *aria-** and *data-** keep their dash

For historical reasons, [aria-*](https://developer.mozilla.org/docs/Web/Accessibility/ARIA) and [data-*](https://developer.mozilla.org/docs/Learn/HTML/Howto/Use_data_attributes) attributes are written as in HTML with dashes.

*source: https://react.dev/learn/writing-markup-with-jsx#3-camelcase-salls-most-of-the-things*

---


### 5 - camel case for style properties

Inline `style` properties are written in camelCase. For example, HTML `<ul style="background-color: black">` would be written as `<ul style={{ backgroundColor: 'black' }}>` in your component.

*source: https://react.dev/learn/javascript-in-jsx-with-curly-braces#using-double-curlies-css-and-other-objects-in-jsx

---


