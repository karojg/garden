---
{"dg-publish":true,"dg-permalink":"react","permalink":"/react/","dgHomeLink":true,"dgPassFrontmatter":false}
---


```toc
style: number
min_depth: 1
max_depth: 6
```

> [!INFO] To Review:
> - [ ] https://www.patterns.dev/posts/reactjs/

# React Fundamentals
Course: https://btholt.github.io/complete-intro-to-react-v7

## Synthetic Event
Synthetic event is actually not a real event from the browser. It's an object that React creates for us that looks and behaves exactly like a regular event.

## JSX
- Is fairly simple HTML-like syntax sugar on top of the raw React APIs.
- It compiles down to React Create Element calls.
- Because JSX is not JavaScript, you have to use a compiler.
- Babel is a tool that will compile that.

## Hooks
- Gets caught every time the render function gets called. 
- Because the hooks get called in the same order every single time, they'll always point to the same piece of state. 
- Because of that they can be stateful: you can keep pieces of mutable state using hooks and then modify them later using their provided updater functions.
- What makes a custom hook a custom hook isn't that the function starts with `use`, but that it uses other hooks inside of it. 

> [!WARNING] Hooks rely on this strict ordering
> As such, **do not put hooks inside if statements or loops**. 

## react-dev-tools
React already has a lot of developer conveniences built into it out of the box. What's better is that they automatically strip it out when you compile your code for production.

If you're using Parcel.js, it will compile your development server with an environment variable of `NODE_ENV=development` and then when you run `parcel build <entry point>` it will automatically change that to `NODE_ENV=production` which is how all the extra weight gets stripped out.

Why is it important that we strip the debug stuff out? The dev bundle of React is quite a bit bigger and quite a bit slower than the production build. 

> [!IMPORTANT] Bundle
> Make sure you're compiling with the correct environmental variables or your users will suffer.

## Strict Mode
React has a new strict mode. If you wrap your app in `<React.StrictMode></React.StrictMode>` it will give you additional warnings about things you shouldn't be doing.

``` jsx
// import at top 
import { StrictMode } from "react"; 
// replace App 
const App = () => { 
	return ( 
		<StrictMode> 
			<div> 
				<h1>Adopt Me!</h1> 
				<SearchParams /> 
			</div> 
		</StrictMode> 
	); 
};
```

## Error Boundaries

Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed. 

> It’s more like a strategy for handling errors in React components.

Error boundaries catch errors during rendering, in lifecycle methods, and in constructors of the whole tree below them.

> [!IMPORTANT] 
> Error Boundaries are only available in Class-based React Components. At the time of this writing, there is currently not a way to implement it using React Hooks.

A class component becomes an error boundary if it defines either (or both) of the lifecycle methods static `getDerivedStateFromError()` or `componentDidCatch()`. Updating state from these lifecycles lets you capture an unhandled JavaScript error in the below tree and display a fallback UI.

> [!NOTE] An error boundary can’t catch an error within itself.
> Only use error boundaries for recovering from unexpected exceptions; don’t try to use them for control flow.
> Error boundaries only catch errors in the components below them in the tree.

### Comparing Error Boundaries and Try...Catch

Error Boundaries actually aren’t in direct competition with try...catch statements. Error Boundaries are only designed for intercepting errors that originate from 3 places in a React component:

1. During render phase
2. In a lifecycle method
3. In the constructor

> Basically… the React-y parts of a component.

As a counterpoint, these are the places where Error Boundaries will not be able to catch an error:

1. Event handlers (e.g., onClick, onChange, etc.)
2. setTimeout or requestAnimationFramecallbacks
3. Server-side rendering (SSR)
4. And errors caused by the error boundary itself (rather than its children)

### `static getDerivedStateFromError()`
This lifecycle is invoked after an error has been thrown by a descendant component. It receives the error that was thrown as a parameter and should return a value to update state.

```js
class ErrorBoundary extends React.Component {
	constructor(props) {
		super(props);
		this.state = { hasError: false 
	};
}


static getDerivedStateFromError(error) {
	// Update state so the next render will show the fallback UI.
	return { hasError: true };
}

render() {
	if (this.state.hasError) {
		// You can render any custom fallback UI
		return <h1>Something went wrong.</h1>;
	}
	    return this.props.children;
	}
}
```

> getDerivedStateFromError() is called during the “render” phase, so side-effects are not permitted. For those use cases, use componentDidCatch() instead.

### to remount an error boundary

Anytime we want this error boundary to be unmounted from the page and then remounted or ultimately reset, we need to pass a new unique key.

```jsx
<ErrorBoundary key={pokemonName} FallbackComponent={ErrorFallback}>
    <PokemonInfo pokemonName={pokemonName} />
</ErrorBoundary>
```

### react-error-boundary
https://github.com/bvaughn/react-error-boundary

> The simplest way to use `<ErrorBoundary>` is to wrap it around any component that may throw an error. This will handle errors thrown by that component and its descendants too.


```jsx
import {ErrorBoundary} from 'react-error-boundary'

function ErrorFallback({error, resetErrorBoundary}) {
  return (
    <div role="alert">
      <p>Something went wrong:</p>
      <pre>{error.message}</pre>
      <button onClick={resetErrorBoundary}>Try again</button>
    </div>
  )
}

const ui = (
  <ErrorBoundary
    FallbackComponent={ErrorFallback}
    onReset={() => {
      // reset the state of your app so the error doesn't happen again
    }}
  >
    <ComponentThatMayError />
  </ErrorBoundary>
)
```

## Portals & Refs
### Portals
You can think of the portal as a separate mount point (the actual DOM node which your app is put into) for your React app. 

React Portal comes in handy when we need to render child components outside the normal DOM hierarchy without breaking the event propagation's default behavior through the React component tree hierarchy. This is useful when rendering components such as modals, tooltips, popup messages, and so much more.

### Refs `useRef` 
- Refs are like instance variables for function components. 
- Whereas on a class you'd say `this.myVar` to refer to an instance variable, with function components you can use refs. 
- They're containers of state that live outside a function's closure state which means anytime I refer to `elRef.current`, it's **always referring to the same element**. 
- This is different from a `useState` call because the variable returned from that `useState` call will **always refer to the state of the variable when that function was called.**

# Intermediate React
## Hooks in Depth

### `useReducer`
- `useReducer` allows us to do Redux-style reducers but inside a hook.
- Instead of having a bunch of functions to update our various properties, we have one reducer that handles all the updates based on an action type. 
- This is a preferable approach if you have complex state updates or if you have a situation like this: all of the state updates are very similar so it makes sense to contain all of them in one function.

```js
const reducer = (state, action) => {
	switch (action.type) {
		case "INCREMENT_R":
			return Object.assign({}, state, { r: limitRGB(state.r + step) });
		case "DECREMENT_R":
			return Object.assign({}, state, { r: limitRGB(state.r - step) });
		case "INCREMENT_G":
			return Object.assign({}, state, { g: limitRGB(state.g + step) });
		case "DECREMENT_G":
			return Object.assign({}, state, { g: limitRGB(state.g - step) });
		case "INCREMENT_B":
			return Object.assign({}, state, { b: limitRGB(state.b + step) });
		case "DECREMENT_B":
			return Object.assign({}, state, { b: limitRGB(state.b - step) });
		default:
			return state;
	}
};
```

### `useMemo` & `useCallback`
- `useMemo` and `useCallback` are performance optimizations. Use them only when you already have a performance problem instead of pre-emptively.
- `useMemo`: memoizes expensive function calls so they only are re-evaluated when needed.
- `useCallback`: It’s useful when a component is passing a callback to its child component in order to prevent rendering. It only changes the callback when one of its dependencies is changed.

## Code Splitting
The latest version of React uses a combination of two things to accomplish this: `Suspense` and `React.lazy`

```jsx
// Splitting on Route
import { useState, StrictMode, lazy, Suspense } from "react"; // above const App = 
const Details = lazy(() => import("./Details")); const SearchParams = lazy(() => import("./SearchParams")); 
// surround BrowserRouter 
<Suspense fallback={<h1>loading route …</h1>}>[…]</Suspense>;
```

**Async Load**
```jsx
// import lazy 
import { Component, lazy } from "react"; 
const Modal = lazy(() => import("./Modal"));
```


## Server Side Rendering

> [!ERROR] Review
> Need to review current implementation
> https://btholt.github.io/complete-intro-to-react-v7/lessons/server-side-rendering/server-side-rendering

This is a technique where you run React on your Node.js server _before_ you serve the request to the user and send down the first rendering of your website already done. This saves precious milliseconds+ on your site because otherwise the user has to download the HTML, then download the JavaScript, then execute the JS to get the app. In this case, they'll just download the HTML and see the first rendered page while React is loading in the background.

While the total time to when the page is actually interactive is comparable, if a bit slower, the time to when the user _sees_ something for the first time should be much faster, hence why this is a popular technique.