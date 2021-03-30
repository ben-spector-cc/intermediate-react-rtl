## Template: Learning Standard X

### Learning Standard Text

...

### Tags

...

### Additional Notes / Resources Used

...

<hr>

## Example: Installing Redux

### Learning Standard Text

The `redux` package is added to a project by first installing it with `npm`...
```
npm install redux
```

...and then importing it into the desired file:
```js
import { createStore } from 'redux'; 
```

### Tags

JavaScript, Redux

### Additional Notes / Resources Used

The Redux API surface is tiny. Redux defines a set of contracts for you to implement (such as reducers) and provides a few helper functions to tie these contracts together. The following top-level exports are available:
- createStore(reducer, [preloadedState], [enhancer])
- combineReducers(reducers)
- applyMiddleware(...middlewares)
- bindActionCreators(actionCreators, dispatch)
- compose(...functions)

This learning standard:
* does not cover reducer functions or store methods
* does not explain how to use `createStore`

Sources:
* https://redux.js.org/api/api-reference