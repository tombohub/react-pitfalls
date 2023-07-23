# React pitfalls
Collection of react pitfalls as described in react documentation


### 1 - capitalize component name
React components are regular JavaScript functions, but their names must start with a capital letter or they won’t work!

*url: https://react.dev/learn/your-first-component*

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

*url: https://react.dev/learn/your-first-component*

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

---

### 
