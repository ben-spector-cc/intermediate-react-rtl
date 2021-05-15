# Testing Components with React Testing Library

_Read the content standards for expectations for [narratives](http://curriculum-documentation.codecademy.com/Content-Standards/narrative/), [checkpoints](http://curriculum-documentation.codecademy.com/Content-Standards/checkpoint/), and [hints](http://curriculum-documentation.codecademy.com/Content-Standards/hint/)._ 

<hr>

## Exercise 1: What is React Testing Library

### Narrative:

In this lesson, you will learn how to test your React components with the **R**eact **T**esting **L**ibrary (RTL). Remember, we need to test our code so that we can be sure that the application we are building is working as expected. If we are building our UI using React, we can use RTL as a way to ensure that our React components are working correctly.

The main advantage of RTL over other testing frameworks is that it allows us to test our components in a way that mimics real user interactions. The logic behind this is that a user will not care about the implementation details of a React component e.g. the component's state, the props passed to it, etc. The user will only care about whether or not they are able to use the app.

Before we jump into the lesson, let's take a quick look at an example that shows off the power and elegance of the React Testing Library. 

Take a moment to observe the UI in your browser's window. It displays a grocery list with a few items. 
You can click on the checkboxes to mark that you've added one of these items to your cart. You can see the code for this component in the file `GroceryList.js`. 

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

Go to the `test.js` file. It contains a unit test using RTL. Observe how it mimics a user clicking the first checkbox.

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
Can you see how we are able to test the `GroceryList` component without knowing any of its implementation details? This is what makes RTL so powerful. We can test our React components as if we are a real user and not worry about the specific logic that went behind coding them.

Don't worry if you cannot understand every single line of the code snippet above. In the upcoming exercises, we will cover RTL so that you can understand everything that's going on.

<hr>

## Exercise 2: Setting up React Testing Library

### Narrative:

In our upcoming exercises, we will use the components from {appName} web app to learn about RTL. Before we do that though, we must know how to set up RTL tests in our app.

In order to use React Testing Library, we will need to include the `@testing-library/react` package in our project by using npm like so:

```js
npm install @testing-library/react
```

Once we have added `@testing-library/react` to our project, we can import the two essential values, `render` and `screen`, into our tests. `render()` is a function that we can use to virtually render components and make them available in our unit tests. Similar to `ReactDOM.render()`, RTL's `render()` function takes in JSX as an argument. 

`screen` is a special object which can be thought of as a representation of the browser window. We can make sure that our virtually rendered components are available in the test by using the `screen.debug()` method which prints out all the DOM contents.  `screen` has a few other useful methods that we'll cover in the upcoming exercises but for now, let's look at an example. 


Look at the code snippet below, it shows the output of a unit test that prints out the DOM contents of the `Greeting` component.

```js
import { render, screen } from '@testing-library/react'

function Greeting() {
  return (<h1>Hello World</h1>)
}

test('should prints out the contents of the DOM' ()=>{
    render(<Greeting />);
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

After importing the `render` and `screen` values from `'@testing-library/react'`, a test is created using the `test()` function from the [Jest testing framework](https://jestjs.io/docs/getting-started). Inside, the `<Greeting>` component is virtually rendered and then the resulting virtual DOM is printed via the `screen.debug()` method.

Notice how the output shows the rendered contents of `<Greeting>` (an `<h1>` element) and not the component itself. As was mentioned in the first exercise, React Testing Library strives to produce a testing environment that is as close to the user's experience as possible.

### Instructions:

1. Checkpoint: Install @testing-library/react. To verify that you have successfully added the package to your project, navigate to package.json and check that `@testing-library/react` appears in the dependencies array.

Hint: Use npm install --save to add the package to your project.

2. Checkpoint: In test.js import `render()` and `screen` from @testing-library/react.

Hint: Your syntax should look something like this:
```js
import { value1, value2} from `name-of-package`
```

3. Checkpoint: Inside the provided `test()`, call the `render()` function on the {componentName} component and then use `screen.debug()` to make sure the component is included in the unit test. Then, in your terminal, run the `npm test` command to run the test. What do you see?

Hint: Your syntax should look something like this:
```js
 render(componentName)
 screen.debug()
```

<hr>

## Exercise 3: Querying with RTL

### Narrative:

Now that we know how to set up RTL, we can finally start testing our React components. To do so, we first have to query and extract the DOM nodes from our components. Once that is done, we can check and see if the extracted DOM nodes have the correct content or are working as expected. Fortunately for us, RTL has many built in query methods that greatly simplifies this process. In this exercise, we will cover the `getBy` query methods. 

There are two ways of accessing the `getByX` functions:

- We can access them by applying destructuring on the object returned by the `render()` function.

```js
const {getByText, getByLabelText} = render(<App/>)
const node1 = getByText('Login Page')
const node2 = getByLabelText('Username')
```

- Or We can use the `screen` object to use them directly

```js
render(<App/>)
const node1 = screen.getByText('Login Page')
const node2 = screen.getByLabelText('Username')
```

We will focus on the `screen.getByX` option for this exercise and the upcoming ones.

Look at the example below, the `.getByText()` method is used to extract a DOM element with text that matches a specified string.
```js
import { render, screen } from '@testing-library/react';

const Button = ()=>{
    return <button type="submit" disabled>Submit</button>
};

test('A "Submit" button is rendered', () => {
  // Render the Button component
  render(<Button/>);
  // Extract the <button>Submit</button> node
  const button = screen.getByText('Submit'); 
});
```

Similarly, another method is `.getByRole()` that allows us to extract a DOM node by its role type. Look at the example below, it shows us another way we can query for the `<button>` element using `.getByRole()`.

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

Though `.getByText()` and `.getByRole()` often are enough, RTL has a bunch of these `.getByX` methods. Instead of memorizing them all, it is best to look at the [docs](https://testing-library.com/docs/queries/about/) to find the one that best suits your needs..

Now that we know how to query DOM nodes, we can test them using [Jest assertions](https://jestjs.io/docs/expect). Recall that in the first exercise we saw the assertion `expect.toBeChecked()`. This isn't part of the regular set of Jest matchers, but instead are extensions provided by the `testing-library/jest-dom` library. 

You can install this library using the command `npm install --save-dev @testing-library/jest-dom`. The library can then be imported like so:

```js
import '@testing-library/jest-dom'
```
Here is an example of the `expect.toBeDisabled()` matcher being used to test a DOM node extracted with the `screen.getByRole()` method.

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

Once again, there are many different jest matchers. In this lesson we'll get a chance to see a number of the most common ones, however, instead of memorizing all of them, it is best to just follow the [docs](https://github.com/testing-library/jest-dom). 

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

<hr>

## Exercise 4: Different Query methods

### Narrative:

Now that we know how to perform queries with `.getByX` methods, it is time for us to move on to the other query method variants. RTL has two other categories of query methods called `.queryByX` and `.findByX`.

Look at the code below. It shows the code for a simple component that changes the header text to 'Goodbye!' after the user clicks a button. We will be using this `App` component to demonstrate the different query types.

```js
import { useState } from 'react';

const App = () => {

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

Let's start with the [`.queryByX`](https://testing-library.com/docs/queries/about/) variants. The `.queryByX` methods return `null` if they don't find a DOM node unlike the `.getByX` methods which throw an error. This is useful when asserting that an element is not present in the DOM. 

The example below shows a scenario when you'll need `.queryByX` methods. In this example, we want to confirm that the `header` does not yet contain the text `'Goodbye'`:

```js
import App from './components/App'
import { render, screen } from '@testing-library/react'

test('should show DOM content as null', () => {
  // Render App
  render(<App />)
  // Extract App 
  const header = screen.queryByText('Goodbye!')
  // Assert null as we have not clicked the button
  expect(header).toBeNull()
})
```

By using the `.queryByText()` variant, when there is no element with the text `'Goodbye!', the value `null` is returned and we can successfully validate this with `expect(header).toBeNull()`. If the `.getByText()` method were used instead, the test would fail immediately due to the error rather than continuing on to the `expect()` assertion.

Next, let's discuss the [`.findByX`](https://testing-library.com/docs/queries/about/) variants. The `.findByX` methods are used to query for asynchronous elements which will eventually appear in the DOM. For example, if the user is waiting for the result of an API call to be displayed, or the text in the DOM to be updated after a set time. The `.findByX` methods work by returning a Promise which resolves when the queried element renders in the DOM.

The example below shows a scenario when you'll need `.findByX` methods. In this example, we want to confirm that the `header`will eventually display the text `'Goodbye'`

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
In the example above we use `.findByText()` as the `'Goodbye!'` message does not render immediately. This is because our `handleClick()` function changes the text after an interval of 500ms. So, we have to wait a bit before the new text is rendered in the DOM

Note the `async` and `await` keywords in the example above. Remember that `findBy` methods return a Promise and thus the callback function that carries out the unit test must be identified as `async` while the `screen.findByText()` method must be preceded by `await`. . 

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
In the example above, the `userEvent.type()` method is used which accepts a DOM node to interact with (`textbox`) and a string to type into that node (`'Hey Mack!'). 

Once again, instead of memorizing all these, it is best if you just read the [docs](https://github.com/testing-library/user-event) and figure out how the different methods work.

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

<hr>

## Exercise 6: Testing asynchronous components

### Narrative:

In the previous exercise we've learned how to extract DOM nodes using the different query variants. One of the methods we learned was the `.findByX` methods that allowed us to test components that render asynchronously. In this exercise, we will cover another RTL method for asynchronous testing - the `waitFor()` async method.

In order to use this method, we need to import it from  `@testing-library/react`.

```js
import { waitFor} from '@testing-library/react'
```

As with the other async methods, the `waitFor()` method returns a Promise, so we have to preface its call with the `await` keyword. It takes a callback function as an argument where we can perform queries and run assertions. E.g.

```js

await waitFor(() => expect(someAsyncMethod).toHaveBeenCalled())
```

Look at the example below, it shows a component that displays a counter which updates asynchronously when the "Increase Count" button is clicked. We can use the `waitFor()` method to check if the component is working as expected.

```js
import {useState} from 'react'
import {render, screen, waitFor} from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import '@testing-library/jest-dom'

const Counter = ()=>{
    const [count,setCount] = useState(0);
    const handleClick =()=>{
      // Counter updates after a time of 250ms
        setTimeout(()=>{
            setCount(count + 1)
        },250)
    };
    return(
        <div>
            <h1>{count}</h1>
            <button onClick = {handleClick}>Increase Count</button>
        </div>
    );
}

test('should update counter', async ()=>{
  // Render Counter
  render(<Counter/>)
  // Extract button node 
  const button = screen.getByRole('button')
  // click button
  userEvent.click(button)
  // Extract initial button display
  const count = screen.getByText('0')
  // Make sure initial count value is 0
  expect(count).toHaveTextContent('0')
  // Use waitFor to check if count is incremented
  await waitFor(()=> expect(screen.queryByText('1')).toBeInTheDocument())
})

```
In the code snippet above, the callback inside `waitFor()` asserted that the text in the counter would be updated to `"1"`. We first extracted our node with the `.queryByText()` method and then ran our assertion `expected().toBeInTheDocument()`.

What if an element is removed asynchronously? `waitFor()` can be used to check for this as well. Look at the example below. We have a component which displays a header. This header is removed asynchronously when the button "Remove Header" is clicked. This component can be tested with the `waitFor()` method like so:

```js
const Header = ()=>{
   const handleClick = ()=>{
       setTimeout(()=>{
         document.querySelector('h1').remove()
       },250)
   };
   return(
       <div>
           <h1>Hey Everybody</h1>
           <button onClick = {handleClick}>Remove Header</button>
       </div>
   )
}

test('should remove header display', async ()=>{
  // Render Header
  render(<Header/>)
  // Extract button node 
  const button = screen.getByRole('button')
  // click button
  userEvent.click(button)
  await waitFor(()=> expect(screen.queryByText('Hey Everybody')).toBeNull())
})
```

In the code snippet above, since the header display is removed 250ms after the button is clicked, we can use the `waitForElementToBeRemoved()` method to check if the header is removed successfully from the DOM.

Note: we could've also used `waitFor()` to run this test:

```js
test('should remove header display', async ()=>{
  // Render Header
  render(<Header/>)
  // Extract button node 
  const button = screen.getByRole('button')
  // click button
  userEvent.click(button)
  await waitFor(()=> expect(screen.queryByText('Hey Everybody')).toBeNull())
})
```

This is, however, a bit more verbose than using `waitForElementToBeRemoved()`. How you decide to run your tests is up to you.

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

<hr>

## Exercise 7: Testing Components That Make API Calls

### Narrative:

We've learned how to use async methods to run our test, it is now time for us to learn how to test React components that make API calls. Remember, in unit tests our React components should not hit an actual endpoint. This is because hitting a real endpoint can be a slow process. Also, if we're using a third-party API, they might not appreciate us overloading their servers with requests. Instead we should mimic an API call that returns a mock response.

Fortunately for us, we can mock an API call using the `jest.fn()` method to see if our component that calls the API is working as expected.

Look at the component below. It calls an API and then renders the `name` property of the response data on the screen. 

```js
import { useState, useEffect } from "react"

const App = ({url})=>{
    const [data,setData] = useState(null)
    useEffect(()=>{
        // LoadData calls fetch with the url prop 
        // and sets the 'data' state to respone.json()
        const loadData = async ()=>{
            const res = await fetch(url);
            const data = await res.json();
            setData(data)
        }

        loadData();

    },[])

    if(!data){
        // If data is not present return loading
        return <span data-testid = "loading" >Loading...</span>
    }
    return(
        <div>
            {/*Render the data property of the response json*/}
            <h2 data-testid = "name"> {data.name}</h2>
        </div>
    )

}
```

Look at our test below. Observe how we are using the `jest.fn()` method to mock an API response. We are then using this mock fetch to fake an API call and test our component.

```js
import {render, screen, waitFor} from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import '@testing-library/jest-dom'

// Mocking fetch call
global.fetch = jest.fn(() =>
  Promise.resolve({
    json: () => Promise.resolve({ name: 'John Doe', age: 27}),
  })
);

beforeEach(() => {
  fetch.mockClear();
});

test('should fetch and display name data', async ()=>{

  const url = '/user?id=1'
  // Render ApiApp
  render(<App url ={url}/>)

  // Assert loading text appears
  expect(screen.getByTestId('loading')).toHaveTextContent('Loading...')
  // Assert data retrieved from fetch has name property 'John'
  await waitFor(()=> expect(screen.getByTestId('name').toHaveTextContent('John Doe')))
  // Assert fetch to have been called once
  expect(fetch).toHaveBeenCalledTimes(1);
  // Assert fetch to have been called with url '/user?id=1'
  expect(fetch).toHaveBeenCalledWith('/user?id=1');

})
```
In the code snippet above, we are first checking to see if our component successfully displays the "Loading..." message. Since an API call is asynchronous, we are then using the `waitFor()` method to check if the call is successful and that we are successfully displaying the name property from the response JSON. Observe the assertions `toHaveBeenCalledTimes()` and `toHaveBeenCalledWith()`. These are extensions of jest assertions that allow you to check whether we are calling our API with the correct parameters.

### Instructions:

1. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

2. Checkpoint: _Insert checkpoint text here._

Hint: _Insert optional but recommended hint text here._

<hr>


## Review

### Narrative:

Good job! We can now use React Testing Library (RTL) to test our React components. Let's quickly review what we've learned so far:

- React Testing Library allows us to test React components by mimicking real user interactions.
- In order to make your component available in the unit test, we have to use the `render()` function. We can check to see the available components in our rendered DOM by using the `screen.debug()` method. `screen` is a special object that can be thought of as a representation of the browser window.
- RTL has built in query methods (`.getByX`,`.findByX`,`.queryByX`) that allows us to extract the DOM nodes from your components. We can use these query methods by using the `screen` object e.g. `screen.getByText()`
- We can test the behaviour of these extracted nodes by using the jest matchers provided by the `@testing-library/jest-dom` library. E.g. `expect().toBeChecked()`.
- We can mimic user interaction by using methods provided by the `testing-library/user-event` library. An example method is `userEvent.click()`.
- Besides `.findByX`, RTL has the async methods `waitFor()` and `waitForElementToBeRemoved()` to test asynchronous components.
- Components that make API calls can be tested by using jest to mock an API call.


