# proposal-ternary-placeholder

Enhanced ternary operator proposal for EcmaScript / JavaScript


## Motivation

The ternary operator is a very useful thing which sometimes helps make code a lot easier to understand. However, sometimes it doesn't cover all needs, and instead, we have to write a bit more verbose code. Often that happens when we need to modify the value that we check in the condition part.

For example,

```js
function X() {
  return x?.y?.z ? doSomethingWithValue(x?.y?.z) : null;
}
```

That looks okay, but when `x?.y?.z` require computations, we've got a problem:

```js
function X() {
  return x?.y?.z
    ? doSomethingWithValue(x?.y?.z)
    : null;
}
```

Obviously, we don't want to recompute z again before passing it in `doSomethingWithValue`.

So instead of the ternary operator, we have to write a bit more verbose code:

```js
function X() {
  const result = x?.y?.z;
  
  return result
    ? doSomethingWithValue(x?.y?.z)
    : null;
}
```

That still looks okay, but if instead of `z`, there is a function that can return a nullable result, it gets a lot more verbose:
```js
const Configuration = {
  Component: ({ value }) => value ? <SomeAnotherComponent value={value} /> : null);
};


const OneMoreComponent = () => {
  const Component = Configuration?.Component;
  const component = Component ? <Component /> : null;
  
  return component 
    ? <div className="example-wrapper">{component}</div>
    : null;
};
```

What I propose is to make a special marker variable $if available in ternary operator branches, which evaluates to the condition expression result. In that case, the code from the example could be rewritten in the following (a lot more simple) way:

```js
const Configuration = {
  Component: ({ value }) => value ? <SomeAnotherComponent value={value} /> : null);
};

const OneMoreComponent = () => Configuration?.Component 
  ? <div className="example-wrapper"><$it /></div>
  : null;
```

And that ends up having a lot more compact, neat and readable code.
