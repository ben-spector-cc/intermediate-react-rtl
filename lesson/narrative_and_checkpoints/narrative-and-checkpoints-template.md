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

Once you have added `@testing-library/react` to your project, you can use the `render()` function to render the element and make it available in your unit test. `render()` is a special function that takes in JSX as an argument, similar to `ReactDOM.render()`. 

You can use make sure that your component is available in the test by using the `screen.debug()` method which prints out all the DOM contents. `screen` is a special object which can be thought of as a representation of the browser window. 

`render()` and `screen` can be imported like so:

```js
import { render, screen } from '@testing-library/react
```
Look at the code snippet below, it shows the output of a unit test that prints out the DOM contents.

```js
test('shoult prints out the contents of the DOM' ()=>{
    render(<h1>Hello World</h1>);
    screen.debug()
})

// Output:
<body>
  <div>
    <h1>
      Hello World
    </h1>
  </div>
</body>
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

Now that we know how to set up RTL, we can finally start testing our React components. To do so, we first have to query and extract the DOM nodes from our components. Once that is done, we can check and see if the extracted DOM nodes have the correct content or are working as expected. Fortunately for us, RTL has many built in query methods that greatly simplifies this process. In this lesson, we will cover the `getBy` query methods. 

There are two ways of accessing the `getBy` functions:

- You can access them by applying destructuring on the `render()` function.

```js
const {getByText, getByLabelText} = render(<App/>)
const node1 = getByText('Login Page')
const node2 = getByLabelText('Username')
```

- Or you can use the `screen` object to use them directly

```js
render(<App/>)
const node1 = screen.getByText()
const node2 = screen.getByLabelText()
```

We will focus on the `screen.getBy` option for this lesson and the upcoming ones.

Look at the example below, the `getByText()` method is used to extract a DOM element with a specified string.
```js
import {render} from '@testing-library/react'

const Button = ()=>{
    return <button type="submit" disabled>Submit</button>
}

test('extracts the button DOM node', () => {
  // Render the Button component
  render(<Button/>);
  // Extract the <button>Submit</button> node
  const button = screen.getByText('Submit') 
});
```

Similarly, another method is `getByRole()` that allows us to extract a DOM node by its functionality. E.g.

```js
import {render} from '@testing-library/react'

const Button = ()=>{
    return <button type="submit" disabled>Submit</button>
}

test('extracts the button DOM node', () => {
  // Render the Button component
  render(<Button/>);
  // Extract the <button>Submit</button> node
  const button = screen.getByRole('button') 
});
```

RTL has a bunch of these `getBy` methods. Instead of memorizing them all, it is best to look at the [docs](https://testing-library.com/docs/queries/about/).

Now that we know how to query DOM nodes, we can test them using jest assertions. Recall that in the first exercise we saw the assertion `expect.toBeChecked()`. This isn't part of regular jest matchers, but instead are extensions provided by the `testing-library/jest-dom` library. 

You can install this library using the command `npm install --save-dev @testing-library/jest-dom`. The library can then be imported like so:

```js
import '@testing-library/jest-dom'
```
Here is an example of the `expect.toBeDisabled()` matcher.

```js
import {render} from '@testing-library/react'
import '@testing-library/jest-dom'

const Button = ()=>{
    return <button type="submit" disabled>Submit</button>
}

test('should show the button as disabled', () => {
  // render Button component
  render(<Button/>);
  // Extract <button>Submit</button> Node
  const button = screen.getByRole('button')
  // Assert button is disabled
  expect(button).toBeDisabled()
});
```

Once again, there are many different jest matchers. Instead of memorizing all of them, it is best to just follow the [docs](https://github.com/testing-library/jest-dom). 

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

<hr>

## Exercise 4: Different Query methods

### Narrative:

Now that we know how to perform queries with `getBy`, it is time for us to move on to the other query method variants. RTL has two other categories of query methods called `queryBy` and `findBy`.

Look at the code below. It shows the code for a simple component that changes the header text to 'Goodbye!' after the user clicks a button. We will be using this `App` component to demonstrate the different query types.

```js
import {useState} from 'react';

const App = ()=>{

  const [text, setText] = useState('Hello World!');

  // Changes header text after interval of 500ms
  const handleClick = () => {
    setTimeout(()=>{
        setText('Goodbye!')
    },500)
  };

  return (
    <div>
      <h1>{text}</h1>
      <button onClick={handleClick}>click me</button>
    </div>
  )
}

export default App
```

`queryBy` : The `queryBy` method returns `null` if it doesn't find a DOM node. This is useful when asserting that an element is not present in the DOM. The example below shows a scenario when you'll need `queryBy`. Go to the [docs](https://testing-library.com/docs/queries/about/) to read more about the `queryBy` methods.

```js
import App from './components/App'
import {render} from '@testing-library/react'

test('should show DOM content as null', () => {
  // Render App
  render(<App />)
  // Extract App 
  const header = screen.queryByText('Goodbye!')
  // Assert null as we have not clicked the button
  expect(header).toBeNull()
})
```

`findBy`: The `findBy` method is used for asynchronous elements which will eventually appear in the DOM. E.g. if the user is waiting for the result of an API call to be displayed, or the text in the DOM to be updated after a set time. The findBy method works by returning a Promise which resolves when the queried element renders in the DOM. Go to the [docs](https://testing-library.com/docs/queries/about/) to read more about the `findBy` method.

```js
import App from './components/App'
import {render} from '@testing-library/react'

test('should show text content as Goodbye', async () => {
  // Render App
  render(<App />)
  // Extract button node 
  const button = screen.getByRole('button')
  // click button
  userEvent.click(button)
  // Extract header with new text
  const header = await screen.findByText('Goodbye!')
  // Assert header to have text 'Goodbye!'
  expect(header).toHaveTextContent('Goodbye!')
})
```
In the example above we use `findByText()` as the 'Goodbye!' message does not render immediately. This is because our `handleClick()` function chnages the text after an interval of 500ms. So we have to wait a bit before the new text is rendered in the DOM

Note the `async` and `await` keywords in the example above. Remember that `findBy` methods return a promise and thus the callback function that carries out the unit test must be asynchronous. 

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

<hr>

## Exercise 5: Mimicking User Interactions

### Narrative:

Previously we've learned how to query and extract the different DOM nodes from our React components. It is time for us to learn how to mimic user interactions e.g. clicking a checkbox, typing text, etc. Once again, this entire process has been made easier for us with the help of the library `testing-library/user-event`.

The library can be installed with the command `npm install @testing-library/user-event` and then imported in test.js like so:

```js
import userEvent from '@testing-library/user-event'
```

The user-event library contains many built in methods that allow us to mimic user interactions. Here is an example where we mimic a user filling in a text box.

```js

import {render} from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import '@testing-library/jest-dom'

const Textbox = ()=>{
  return(
      <form>
          <label role = "textbox" htmlFor = "greeting">
              Greeting:
          </label>
          <input type="text" id="greeting" />
          <input type="submit" value="Submit" />
      </form>
  )
}

test('should show text content as Hey Mack!', () => {
  // Render TextArea
  render(<Textbox />)
  // Extract textbox component
  const textbox = screen.getByLabelText('Greeting:')
  // Simulate typing Hey Mack!
  userEvent.type(textbox, 'Hey Mack!')
  // Assert textbox has text conent 'Hey Mack!'
  expect(textbox).toHaveValue('Hey Mack!')
})
```
Once again, instead of memorizing all these, it is best if you just read the [docs](https://github.com/testing-library/user-event) and figure how the different methods work.

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

<hr>