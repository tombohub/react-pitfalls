# React pitfalls
Collection of react pitfalls as described in react documentation


## 1 - capitalize component name
React components are regular JavaScript functions, but their names must start with a capital letter or they won’t work!

*url: https://react.dev/learn/your-first-component*

## 2 - parentheses after return

Without parentheses, any code on the lines after return will be ignored!

```javascript
return (
  <div>
    <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />
  </div>
);
```

*url: https://react.dev/learn/your-first-component*
