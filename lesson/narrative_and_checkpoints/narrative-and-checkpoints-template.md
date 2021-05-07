# Testing Components with React Testing Library

_Read the content standards for expectations for [narratives](http://curriculum-documentation.codecademy.com/Content-Standards/narrative/), [checkpoints](http://curriculum-documentation.codecademy.com/Content-Standards/checkpoint/), and [hints](http://curriculum-documentation.codecademy.com/Content-Standards/hint/)._ 

<hr>

## Exercise 1: What is React Testing Library

### Narrative:

In this lesson, you will learn how to test your React components with the React Testing Library. Remember, we need to test our code so that we can be sure that any application we are building is working as expected. If we are building our UI using React, we can use RTL as a way to ensure that our react components are working correctly.

The main advantage of RTL over other testing frameworks is that it allows us to test our component in a way that mimic real user interactions. The logic behind this is that in actual production, a user will not care about the implementation details of a react component e.g. the component's state, the props passed to it, etc. The user will only care on whether or not they are able to use the app.

Take a moment to observe the UI in your browser's window. It displays a grocery list with the following items. 

- Apples
- Milk
- Cereal

You can click on the checkboxes to mark that you've added one of these items to your cart. Go to `GroceryList.js` and observe the code for this component.

```js
const GroceryList = ()=>{
    return (
    <div>
        <h1>Grocery List</h1>
        <ul>
            <li>
                <label htmlFor="item1">Apples</label>
                <input type="checkbox" id="item1"/>
            </li>
            <li>
                <label htmlFor="item2">Milk</label>
                <input type="checkbox" id="item2"/>
            </li>
            <li>
                <label htmlFor="item3">Cereal</label>
                <input type="checkbox" id="item3"/>
            </li>
        </ul>
    </div>
    )
}

export default GroceryList
```

Go to the `test.js` file. It contains a unit test using the RTL library. Observe how it mimicks a user clicking the first checkbox.

```js
import { render, screen, cleanup } from '@testing-library/react';
import GroceryList from './components/GroceryList';
import userEvent from '@testing-library/user-event';
test('should mark the first checkbox as checked', () => {
  // render the grocery list
  render(<GroceryList />);
  // grab the apple item
  const appleItem = screen.getByLabelText('Apples');
  // simulate a "click" on the apple checkbox
  userEvent.click(appleItem);
  // assert that the apple checkbox was checked
  expect(appleItem).toBeChecked();
});
```

Don't worry if you cannot understand every single line of the code snippet above. In the upcoming lessons, we will cover RTL so that you can understand everything that's going on.

Can you see how we are able to test the `GroceryList` component without knowing any of its implementation details? This is what makes RTL so powerful. We can test our React components as if we are a real user and not worry about the specific logic that went behind coding them.

<hr>

## Exercise 2: Setting up React Testing Library

### Narrative:

In our upcoming lessons, we will use the components from {appName} web app to learn about RTL. Before we do that though, we must know how to set up RTL tests in our app.

In order to use React Testing Library, we will need to include the `@testing-library/react` package in our project by using npm like so:

```js
npm install @testing-library/react
```

Once you have added `@testing-library/react` to your project, you can import the `render()` function:

```js
import { render } from '@testing-library/react
```
`render()` is a special function that takes in JSX as an argument. E.g.

```js
render(<h1>Hello World</h1>);
```

It makes our component available in the unit test. We can make sure that this is the case by using the `screen.debug()` function. It prints out all the DOM contents. E.g.

```js
  render(<h1>Hello World</h1>);
  screen.debug()
```
will print out
```HTML
<body>
  <div>
    <h1>
      Hello World
    </h1>
  </div>
</body>
```

`screen` is a special object which can be thought of as a representation of the browser window. `screen` can be imported like so: 

```js
import { screen } from '@testing-library/react'`.
```
### Instructions:

1. Checkpoint: Install @testing-library/react. To verify that you have successfully added the package to your project, navigate to package.json and check that `@testing-library/react` appears in the dependencies array.

Hint: Use npm install --save to add the package to your project.

2. Checkpoint: In test.js import `render()` and `screen` from @testing-library/react.

Hint: Your syntax should look something like this:
```js
import {componentName} from `name-of-package`
```

3. Checkpoint: Call the render function on the {componentName} component and then use screen.debug() to make sure the component is included in the unit test. What do you see?

Hint: Your syntax should look something like this:
```js
 render(componentName)
 screen.debug()
```

<hr>

## Exercise 3: Querying with RTL

### Narrative:

Now that we know how to set up RTL, it is time for us to go through how to perform queries using the RTL libraries. Querying allows us to extract the different DOM nodes from our React component and test them individually. Fortunately for us RTL has many built in query methods that greatly simplifies this process for us. In this lesson, we will cover the `getBy...` query methods.

To get a list of all the `getBy` methods go to the RTL docs. There are two ways of using these query methods. You can either extract them from the `render` function and apply them, or you can use the `screen` object `screen.getByText()`. We will focus on the `screen.getBy..` option for this lesson and the upcoming ones.

Now that we know how to query DOM nodes, we can test them using jest assertions. Recall in the first exercise we saw an assertion like `expect.toBeChecked()`. This isn't part of the regular jest mathchers, but instead are extensions provided byt `blah blah` library. You can install this library using the command `npm install `. Once installed you can include it in your test.js file like so:
```js
```
There are many different jest matchers. Instead of memorizing them all it is best to just follow the docs. Here is an example of the `expect.` matcher. Look up its documentation to see how its implemented.

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

<hr>

## Exercise 4: Querying with RTL

### Narrative:

Now that we know how to set up RTL, it is time for us to go through how to perform queries using the RTL libraries. Querying allows us to extract the different DOM nodes from our React component and test them individually. Fortunately for us RTL has many built in query methods that greatly simplifies this process for us. In this lesson, we will cover the `getBy...` query methods.

To get a list of all the `getBy` methods go to the RTL docs. There are two ways of using these query methods. You can either extract them from the `render` function and apply them, or you can use the `screen` object `screen.getByText()`. We will focus on the `screen.getBy..` option for this lesson and the upcoming ones.

Now that we know how to query DOM nodes, we can test them using jest assertions. Recall in the first exercise we saw an assertion like `expect.toBeChecked()`. This isn't part of the regular jest mathchers, but instead are extensions provided byt `blah blah` library. You can install this library using the command `npm install `. Once installed you can include it in your test.js file like so:
```js
```
There are many different jest matchers. Instead of memorizing them all it is best to just follow the docs. Here is an example of the `expect.` matcher. Look up its documentation to see how its implemented.

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

<hr>