---
{"dg-publish":true,"dg-permalink":"react","permalink":"/react/","dgHomeLink":true,"dgPassFrontmatter":false}
---


To Review:

- https://www.patterns.dev/posts/reactjs/

# React Fundamentals

## Using JSX

- Is fairly simple HTML-like syntax sugar on top of the raw React APIs.
- It compiles down to React Create Element calls.
- Because JSX is not JavaScript, you have to use a compiler.
- Babel is a tool that will compile that.

Synthetic Event

- Synthetic event is actually not a real event from the browser. It's an object that React creates for us that looks and behaves exactly like a regular event.

Custom Hooks
React can treat this function as a function component, and React can treat this function as a custom hook so it can apply some of its ESLint niceties to the code for this hook.

4:50 The key thing that I want you to pull away from this is what makes a custom hook a custom hook isn't that the function starts with use, but that it uses other hooks inside of it. All a custom hook is is a function that uses hooks. Those hooks can be built-in hooks like what we're doing here, or they can be other custom hooks, so you can compose these things together.

## Lifting State

What we did was we lifted state. You take that state that we have, and we find the component that is the sibling component that needs to share that state. We find the least common parent between these two components.

## Error Boundaries in React

Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed. Error boundaries catch errors during rendering, in lifecycle methods, and in constructors of the whole tree below them.

A class component becomes an error boundary if it defines either (or both) of the lifecycle methods static getDerivedStateFromError() or componentDidCatch(). Updating state from these lifecycles lets you capture an unhandled JavaScript error in the below tree and display a fallback UI.

> Only use error boundaries for recovering from unexpected exceptions; don’t try to use them for control flow.
> Error boundaries only catch errors in the components below them in the tree. An error boundary can’t catch an error within itself.

What are Error Boundaries exactly? Contrary to what you may think, it’s not a new component or JavaScript library. It’s more like a strategy for handling errors in React components.

Any React Component is considered an Error Boundary when it employs at least one of these lifecycle methods.

Good practices suggest that you will want to create a component that is purpose-built as an Error Boundary instead of mixing error-handling logic into your generic components.

### Comparing Error Boundaries and Try...Catch

Error Boundaries actually aren’t in direct competition with try...catch statements. Error Boundaries are only designed for intercepting errors that originate from 3 places in a React component:

During render phase
In a lifecycle method
In the constructor
Basically… the React-y parts of a component.

As a counterpoint, these are the places where Error Boundaries will not be able to catch an error:

Event handlers (e.g., onClick, onChange, etc.)
setTimeout or requestAnimationFramecallbacks
Server-side rendering (SSR)
And errors caused by the error boundary itself (rather than its children)
So Error Boundaries don’t really impact how you use try...catch. They’re both needed as a robust strategy for handling errors in React.

> Note: Error Boundaries are only available in Class-based React Components. At the time of this writing, there is currently not a way to implement it using React Hooks.

## `static getDerivedStateFromError()`

`static getDerivedStateFromError(error)`
This lifecycle is invoked after an error has been thrown by a descendant component. It receives the error that was thrown as a parameter and should return a value to update state.

```
class ErrorBoundary extends React.Component {
constructor(props) {
super(props);
this.state = { hasError: false };
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

## to remount an error boundary

Anytime we want this error boundary to be unmounted from the page and then remounted or ultimately reset, we need to pass a new unique key.

```
<ErrorBoundary key={pokemonName} FallbackComponent={ErrorFallback}>
    <PokemonInfo pokemonName={pokemonName} />
</ErrorBoundary>
```

### react-error-boundary

https://github.com/bvaughn/react-error-boundary

- `onReset`
- `resetKeys`

# Advanced React Hooks

## `useReducer`

## Compound Components

One liner: The Compound Components Pattern enables you to provide a set of components that implicitly share state for a simple yet powerful declarative API for reusable components.

Compound components are components that work together to form a complete UI. The classic example of this is <select> and <option> in HTML:

```
<select>
  <option value="1">Option 1</option>
  <option value="2">Option 2</option>
</select>

```


React