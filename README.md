# useEffect: persistent state

## 📝 Your Notes

## Background

`React.useEffect` is a built-in hook that allows you to run some custom code
after React renders (and re-renders) your component to the DOM. It accepts a
callback function which React will call after the DOM has been updated:

```javascript
React.useEffect(() => {
  // your side-effect code here.
  // this is where you can make HTTP requests or interact with browser APIs.
});
```

## Exercise

In this exercise, we're going to enhance our `<Greeting />` component to get its
initial state value from localStorage (if available) and keep localStorage
updated as the `name` is updated.

## Extra Credit

### 1. 💯 lazy state initialization

Right now, every time our component function is run, our function reads from
localStorage. This is problematic because it could be a performance bottleneck
(reading from localStorage can be slow). And what's more we only actually need
to know the value from localStorage the first time this component is rendered!
So the additional reads are wasted effort.

To avoid this problem, React's useState hook allows you to pass a function
instead of the actual value, and then it will only call that function to get the
state value when the component is rendered the first time. So you can go from
this: `React.useState(someExpensiveComputation())` To this:
`React.useState(() => someExpensiveComputation())`

And the `someExpensiveComputation` function will only be called when it's
needed!

Make the `React.useState` call use lazy initialization to avoid a performance
bottleneck of reading into localStorage on every render.

> Learn more about
> [lazy state initialization](https://kentcdodds.com/blog/use-state-lazy-initialization-and-function-updates)

### 2. 💯 effect dependencies

The callback we're passing to `React.useEffect` is called after _every_ render
of our component (including re-renders). This is exactly what we want because we
want to make sure that the `name` is saved into localStorage whenever it
changes, but there are various reasons a component can be re-rendered (for
example, when a parent component in the application tree gets re-rendered).

Really, we _only_ want localStorage to get updated when the `name` state
actually changes. It doesn't need to re-run any other time. Luckily for us,
`React.useEffect` allows you to pass a second argument called the "dependency
array" which signals to React that your effect callback function should be
called when (and only when) those dependencies change. So we can use this to
avoid doing unnecessary work!

Add a dependencies array for `React.useEffect` to avoid the callback being
called too frequently.

### 3. 💯 custom hook

The best part of hooks is that if you find a bit of logic inside your component
function that you think would be useful elsewhere, you can put that in another
function and call it from the components that need it (just like regular
JavaScript). These functions you create are called "custom hooks".

Create a custom hook called `useLocalStorageState` for reusability of all this
logic.

### 4. 💯 flexible localStorage hook

Take your custom `useLocalStorageState` hook and make it generic enough to
support any data type (remember, you have to serialize objects to strings... use
`JSON.stringify` and `JSON.parse`). Go wild with this!
