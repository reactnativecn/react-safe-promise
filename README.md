Wrap a promise to avoid memory leak and warning after component is unmounted.

Fix a common warning that says `Can only update a mounted or mounting component. This usually means you called setState() on an unmounted component. This is a no-op. Please check the code for the undefined component.`;

## Install:

```bash
npm install react-safe-promsie -save
```

## Wrap your component:

For babel 5 (React native <= 0.15), you can use decorator:

```javascript
import safePromise from 'react-safe-promise';

@safePromise
export default class MyComponent extends React.Component
```

For babel 6 (React native >= 0.16), you must use as a function

```javascript
import safePromise from 'react-safe-promise';

class MyComponent extends React.Component{
}

export default safePromise(MyComponent);
```

## Usage:

```javascript
this.safePromise(fetch(url))
  .then(resp=>this.safePromise(resp.json()))
  .then(data=>this.setState({data:data}));
```

If the promise returned by `fetch(url)` is resolved after the component was unmounted, no following callback will be called. And, the reference to component will be released quickly after the component was unmounted.

## API

### safePromise(Clazz)

Wrap a component class to support `this.safePromise()` method.

### this.safePromise(promise)

Wrap a promise, return a new promise, which will not keep reference to this after this was unmounted.
