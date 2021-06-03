# Testing Components with React Testing Library

_Read the content standards for expectations for [narratives](http://curriculum-documentation.codecademy.com/Content-Standards/narrative/), [checkpoints](http://curriculum-documentation.codecademy.com/Content-Standards/checkpoint/), and [hints](http://curriculum-documentation.codecademy.com/Content-Standards/hint/)._ 

<hr>

## Exercise 1: What is React Testing Library

### Narrative:

In this lesson, you will learn how to test your React components with the **R**eact **T**esting **L**ibrary (RTL). Remember, we need to test our code so that we can be sure that the application we are building is working as expected. If we are building our UI using React, we can use RTL as a way to ensure that our React components are working correctly.

The main advantage of RTL over other testing frameworks is that it allows us to test our components in a way that mimics real user interactions. The logic behind this is that a user will not care about the implementation details of a React component e.g. the component's state, the props passed to it, etc. The user will only care about whether or not they are able to use the app.

Before we jump into the lesson, let's take a quick look at an example that shows off the power and elegance of the React Testing Library. 

> Note: this lesson assumes that you have a basic understanding of the [Jest](https://jestjs.io/docs/getting-started) testing framework. Check out [our lesson](ben-will-add-link-here-once-published) if you would like to learn more!

Take a moment to observe the UI in your browser's window. It displays a grocery list with a few items. You can click on the checkboxes to mark that you've added one of these items to your cart. You can see the code for this component in the file `GroceryList.js`. 

```js
const GroceryList = () => {
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
};

export default GroceryList;
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

### Instructions

For this lesson, we will be using the project Passing Thoughts for all our checkpoints. This is a React app that allows you to fill out a simple input form and post a "thought". Once the thought is posted, it will disappear after 15 seconds. Before jumping into the lesson, play around with the App in your browser and explore its components in your code editor.
In this lesson, we will explore how we can test the features of this application from the users perspective including
* verifying that components are present at the start of the program
* mimicking user interactions
* verifying that components have changed after the user interacts with the program
* verifying the presence of components that render asynchronously
* verifying the behavior of components that make API calls

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

const Greeting = () => {
  return (<h1>Hello World</h1>)
};

test('should prints out the contents of the DOM' () => {
    render(<Greeting />);
    screen.debug();
});

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

For this lesson, we will be using the project Passing Thoughts for all our checkpoints. This is a React app that allows you to fill out a simple input form and post a "thought". Once the thought is posted, it will disappear after 15 seconds. Before jumping into the lesson, play around with the App in your browser and explore its components in your code editor. 

### Instructions:

1. Checkpoint: Install `@testing-library/react`. To verify that you have successfully added the package to your project, navigate to package.json and check that `@testing-library/react` appears in the dependencies array.

Hint: Use npm install `package name` --save dev to add the package to your project.

2. Checkpoint: In test.js import `render()` and `screen` from @testing-library/react.

Hint: Your syntax should look something like this:
```js
import { value1, value2 } from `name-of-package`;
```

3. Checkpoint: Inside the provided `test()` in `myApp.test.js` call the `render()` function on the `Thought` component and then use `screen.debug()` to make sure the component is included in the unit test. Then, in your terminal, run the `npm test` command to run the test. What do you see?

Hint: Your syntax should look something like this:
```js
 render(<componentName>);
 screen.debug();
```

<hr>

## Exercise 3: Querying with RTL

### Narrative:

Now that we know how to set up RTL, we can finally start testing our React components. To do so, we first have to query for and extract the DOM nodes from our virtually rendered components. Once that is done, we can check and see if the extracted DOM nodes were rendered as expected. Fortunately for us, RTL has many built in query methods that greatly simplifies this process. In this exercise, we will cover the `.getByX` query methods. 

There are a number of `.getByX` query methods to choose from and they are all accessible as methods on the `screen` object. Look at the example below, the `.getByText()` method is used to extract a DOM element with text that matches a specified string.
```js
import { render, screen } from '@testing-library/react';

const Button = () => {
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
import { render } from '@testing-library/react';

const Button = () => {
    return <button type="submit" disabled>Submit</button>
};

test('extracts the button DOM node', () => {
  // Render the Button component
  render(<Button/>);
  // Extract the <button>Submit</button> node
  const button = screen.getByRole('button'); 
});
```

Though `.getByText()` and `.getByRole()` often are enough, RTL has a bunch of these `.getByX` methods. Instead of memorizing them all, it is best to look at the [docs](https://testing-library.com/docs/queries/about/) to find the one that best suits your needs..

Now that we know how to query DOM nodes, we can test them using [Jest assertions](https://jestjs.io/docs/expect). Recall that in the first exercise we saw the assertion `expect.toBeChecked()`. This isn't part of the regular set of Jest matchers, but instead is an extension provided by the `testing-library/jest-dom` library. 

You can install this library using the command `npm install --save-dev @testing-library/jest-dom`. The library can then be imported like so:

```js
import '@testing-library/jest-dom';
```
Here is an example of the `expect.toBeDisabled()` matcher being used to test a DOM node extracted with the `screen.getByRole()` method.

```js
import {render} from '@testing-library/react';
import '@testing-library/jest-dom';

const Button = () => {
    return <button type="submit" disabled>Submit</button>
};

test('should show the button as disabled', () => {
  // render Button component
  render(<Button/>);
  // Extract <button>Submit</button> Node
  const button = screen.getByRole('button');
  // Assert button is disabled
  expect(button).toBeDisabled();
});
```

Once again, there are many different jest matchers. In this lesson we'll get a chance to see a number of the most common ones, however, instead of memorizing all of them, it is best to just follow the [docs](https://github.com/testing-library/jest-dom). 

### Instructions:

1. Checkpoint: Install and import the `@testing-library/jest-dom` library.

Hint: Use npm install `package name` --save dev to add the package to your project. Then import the ENTIRE library like so:

```js
import 'package-name';
```

2. Checkpoint: In the first test of the `Thought.test.js` file use `.getByText()` to extract the header node of the `App` component. Then call the `expect().toHaveText()` assertion to confirm that the header node does indeed contain the text `"Passing Thoughts"`.

Hint: Your code should be something like this:

```js
const header = screen.getByText(some text);
expect(header).toHaveText(some Text);
```

3. Checkpoint: In the second test of the `Thought.test.js` file use `.getByRole()` to extract the button node of the `Thought` component. Then use a jest assertion to check if the button is enabled. Use the [docs](https://github.com/testing-library/jest-dom#custom-matchers) to find the appropriate jest assertion!

Hint: Your code should be something like this

```js
const button = screen.getByRole(button);
expect(button).toBeEnabled();
```

<hr>

## Exercise 4: Different Query methods

### Narrative:

Now that we know how to perform queries with `.getByX` methods, it is time for us to move on to the other query method variants. RTL has two other categories of query methods called `.queryByX` and `.findByX`.

Look at the code below. It shows the code for a simple component that changes the header text to `'Goodbye!'` after the user clicks a button. We will be using this `App` component to demonstrate the different query types.

```js
import { useState } from 'react';

const App = () => {

  const [text, setText] = useState('Hello World!');

  // Changes header text after interval of 500ms
  const handleClick = () => {
    setTimeout(() => {
        setText('Goodbye!');
    }, 500);
  };

  return (
    <div>
      <h1>{text}</h1>
      <button onClick={handleClick}>click me</button>
    </div>
  )
};

export default App;
```

Let's start with the [`.queryByX`](https://testing-library.com/docs/queries/about/) variants. The `.queryByX` methods return `null` if they don't find a DOM node unlike the `.getByX` methods which throw an error and immediately cause the test to fail. This is useful when asserting that an element is not present in the DOM. 

The example below shows a scenario when you'll need `.queryByX` methods. In this example, we want to confirm that the `header` does not (yet) contain the text `'Goodbye'`:

```js
import App from './components/App';
import { render, screen } from '@testing-library/react';

test('should show DOM content as null', () => {
  // Render App
  render(<App />);
  // Extract App 
  const header = screen.queryByText('Goodbye!');
  // Assert null as we have not clicked the button
  expect(header).toBeNull();
});
```

By using the `.queryByText()` variant, when there is no element with the text `'Goodbye!', the value `null` is returned and we can successfully validate this with `expect(header).toBeNull()`. If the `.getByText()` method were used instead, the test would fail immediately due to the error rather than continuing on to the `expect()` assertion.

Next, let's discuss the [`.findByX`](https://testing-library.com/docs/queries/about/) variants. The `.findByX` methods are used to query for asynchronous elements which will eventually appear in the DOM. For example, if the user is waiting for the result of an API call to be displayed or for the text in the DOM to be updated after a set time. The `.findByX` methods work by returning a Promise which resolves when the queried element renders in the DOM. As such, the `async`/`await` keywords can be used to enable asynchronous logic..

The example below shows a scenario when you'll need `.findByX` methods. In this example, we want to confirm that the `header`will eventually display the text `'Goodbye'`:

```js
import App from './components/App';
import { render } from '@testing-library/react';

test('should show text content as Goodbye', async () => {
  // Render App
  render(<App />);
  // Extract button node 
  const button = screen.getByRole('button');
  // click button
  userEvent.click(button);
  // Extract header with new text
  const header = await screen.findByText('Goodbye!');
  // Assert header to have text 'Goodbye!'
  expect(header).toHaveTextContent('Goodbye!');
});
```
In the example above we use `.findByText()` since the `'Goodbye!'` message does not render immediately. This is because our `handleClick()` function changes the text after an interval of 500ms. So, we have to wait a bit before the new text is rendered in the DOM

Note the `async` and `await` keywords in the example above. Remember that `findBy` methods return a Promise and thus the callback function that carries out the unit test must be identified as `async` while the `screen.findByText()` method must be preceded by `await`. . 

### Instructions:

1. Checkpoint: In the first test of the `Thought.test.js` file use the `.queryByText()` method to extract the submit button of the `AddThought()` component. Then assert that the node is present in the DOM using a method from the jest-dom library.

Hint: Your code should be something like this:

```js
const node = screen.queryByText('some text');
expect(node).toBeInTheDocument();
```
2. Checkpoint: The `.queryByText('Hey There')` method in the second test of the `Thought.test.js` file is querying for an element that doesn't exist in the document. Use an assertion from the jest-dom library to confirm that this element is not present in the DOM.

Hint: Your code should be something like this:

```js
const node = screen.queryByText(some text);
expect(node).toBeNull();
```

3. Checkpoint: The third test of the `Thought.test.js` file mimics a user posting a thought with the text content 'Oreos are delicious' (we'll cover how you can do this in the next exercise!). Use the `.findByText()` method to grab that node by text and then use a jest-dom matcher to assert that this node will appear in the DOM.

Hint: Your code should be something like this:

```js
const thought = await findByText('some text');
expect(thought).toBeInTheDocument();
```

<hr>

## Exercise 5: Mimicking User Interactions

### Narrative:

Previously we've learned how to query and extract the different DOM nodes from our React components. It is time for us to learn how to mimic user interactions e.g. clicking a checkbox, typing text, etc. Once again, this entire process has been made easier for us with the help of another library, `@testing-library/user-event`.

The library can be installed with the command `npm install @testing-library/user-event` and then imported in test.js like so:

```js
import userEvent from '@testing-library/user-event';
```

The user-event library contains many built in methods that allow us to mimic user interactions. Here is an example where we mimic a user filling in a text box.

```js

import { render } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import '@testing-library/jest-dom';

const Textbox = () => {
  return(
      <form>
          <label role = "textbox" htmlFor = "greeting">
              Greeting:
          </label>
          <input type="text" id="greeting" />
          <input type="submit" value="Submit" />
      </form>
  )
};

test('should show text content as Hey Mack!', () => {
  // Render TextArea
  render(<Textbox />);
  // Extract textbox component
  const textbox = screen.getByLabelText('Greeting:');
  // Simulate typing Hey Mack!
  userEvent.type(textbox, 'Hey Mack!');
  // Assert textbox has text content 'Hey Mack!'
  expect(textbox).toHaveValue('Hey Mack!');
});
```
In the example above, the `userEvent.type()` method is used which accepts a DOM node to interact with (`textbox`) and a string to type into that node (`'Hey Mack!'). 

The `userEvent` object has methods for simulating clicks, hovering, selection and much more. Once again, instead of memorizing all of these, it is recommended that you read the [docs](https://github.com/testing-library/user-event) to find the method best suited for your needs.

### Instructions:

1. Checkpoint: Install the `@testing-library/user-event` library and then import the `userEvent` object.

Hint: Use npm install `package name` --save dev to add the package to your project. Then import it like so:

```js
import { name } from package;
```

2. Checkpoint: In the first test of the `Thought.test.js` file a user is posting a thought with the text content `'Oreos are delicious'`. Use the `userEvent` object to mimic a user pressing the 'X' button of this `Thought` component and thereby removing it. Then use the appropriate query method and a jest-dom assertion to confirm the thought has been removed from the DOM.

Hint: Your code should be something like this:

```js
// First extract the submit button. Then
userEvent.click(submit);
// Query for the removed Thought component. Then
expect(node).toBeNull();
```

3. Checkpoint: In the second test of `Thought.test.js` file implement the `userEvent` object to mimic a user posting a thought with the text content `'Did I forget my keys'`. Then use the appropriate query method to confirm that this thought gets added to the DOM.

Hint: Your code should be something like this:

```js
// First extract the desired nodes
userEvent.type(inputNode, `some text`);
userEvent.click(submit);
const node = await findByText(`some text`);
expect(node).toBeInTheDocument();
```


<hr>

## Exercise 6: Testing asynchronous components and API calls

### Narrative:

In the previous exercise we've learned how to extract DOM nodes using the different query variants. One of the methods we learned was the `.findByX` methods that allowed us to test components that render asynchronously. In this exercise, we will cover another RTL method for asynchronous testing - the `waitFor()` async method.

In order to use this method, we need to import it from  `@testing-library/react`.

```js
import { waitFor } from '@testing-library/react';
```

As with the other async methods, the `waitFor()` method returns a Promise, so we have to preface its call with the `await` keyword. It takes a callback function as an argument where we can make asynchronous function calls (like hitting an API), perform queries, and run assertions. 

```js
await waitFor(() => expect(someAsyncMethod).toHaveBeenCalled())
```

Look at the example below. We have a component which displays a header. This header is removed after 250 ms when the button "Remove Header" is clicked. 

How would you test this? Using only `screen.queryByX()` wouldn't work as the component is removed asynchronously. Fortunately, the `waitFor()` method is perfect for testing such a scenario.

```js
const Header = () => {
   const handleClick = () => {
       setTimeout(() => {
         document.querySelector('h1').remove()
       }, 250);
   };
   return (
       <div>
           <h1>Hey Everybody</h1>
           <button onClick = {handleClick}>Remove Header</button>
       </div>
   );
};

test('should remove header display', async () => {
  // Render Header
  render(<Header/>)
  // Extract button node 
  const button = screen.getByRole('button')
  // click button
  userEvent.click(button)
  await waitFor(() => expect(screen.queryByText('Hey Everybody')).toBeNull())
});
```

Now that we've learned how to user `waitFor()` it is time for us to learn how to test React components that make API calls. Remember, in unit tests our React components should not hit an actual endpoint. This is because hitting a real endpoint can be a slow process. Also, if we're using a third-party API, they might not appreciate us overloading their servers with requests. Instead we should mimic an API call that returns a mock response.

We can do this using the `jest.fn()` method. The mock API call can then be used to test components that hit an external endpoint.

Look at the component below. It calls an API and then renders the `name` property of the response data on the screen. 

```js
import { useState, useEffect } from "react"

const App = ({ url }) => {
    const [data,setData] = useState(null);
    useEffect(() => {
        // LoadData calls fetch with the url prop 
        // and sets the 'data' state to response.json()
        const loadData = async () => {
            const res = await fetch(url);
            const data = await res.json();
            setData(data)
        }
        loadData();
    },[]);

    if(!data){
        // If data is not present return loading
        return <span data-testid = "loading" >Loading...</span>;
    }
    return(
        <div>
            {/*Render the data property of the response json*/}
            <h2 data-testid = "name"> {data.name}</h2>
        </div>
    );
};
```

Look at our test below. We are using the `jest.fn()` method to mock an API call. We are then using the response from this mock call to test if our component is rendering the data correctly.

```js
import { render, screen, waitFor } from '@testing-library/react'
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

test('should fetch and display name data', async () => {

  const url = '/user?id=1'
  // Render Api App
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
Let's break down the code above:
- We are first checking to see if our component successfully displays the "Loading..." message. 
- Since an API call is asynchronous, we are then using the `waitFor()` method to check if the call is successful and that we are successfully displaying the name property from the response JSON. 
- Observe the assertions `toHaveBeenCalledTimes()` and `toHaveBeenCalledWith()`. These are extensions of jest assertions that allow you to check whether we are calling our API with the correct parameters.

### Instructions:

1. Checkpoint: In the first test of the `myApp.test.js` file use the `userEvent` object to mimic a user posting a thought with the text content `'I have to call my mom'`. Use `waitFor()` to assert that this thought will eventually be removed from the DOM (remember, thoughts disappear after 15 seconds).

Hint: Your code should be something like this:

```js
await waitFor(()=> expect(screen.queryByText(some Text)).toBeNull())
```


2. Checkpoint: Go to `Display.js`. The Display component makes an API calls and displays the data from the response in the DOM. The format of the returned data is shown below.

```js
[
  {
    name : 'John Doe',
    occupation: 'Engineer',
    id : 1
  },
  {
    name : 'Jane Doe',
    occupation: 'Doctor',
    id : 2
  }
]
```

Look at the second test of the `myApp.test.js` file. We have code that mimics an API call and returns fake data similar to the response shown above. Use `waitFor()` to ensure that the component calls the endpoint and displays the data correctly. Then use assertions to confirm that you have called your API only once with the correct arguments.

Hint: When testing if the pointing was called correctly, your code should look like the following:

```js
// I'll put the rest of the answer over here. As this question is a bit tricky.
  expect(fetch).toHaveBeenCalledTimes(1);
  expect(fetch).toHaveBeenCalledWith(url);
```
<hr>


## Review

### Narrative:

Good job! We can now use React Testing Library (RTL) to test our React components. Let's quickly review what we've learned so far:

- React Testing Library allows us to test React components by mimicking real user interactions.
- In order to make your component available in the unit test, we have to use the `render()` function. We can check to see the available components in our rendered DOM by using the `screen.debug()` method. `screen` is a special object that can be thought of as a representation of the browser window.
- RTL has built in query methods (`.getByX`,`.findByX`,`.queryByX`) that allows us to extract the DOM nodes from your components. We can use these query methods by using the `screen` object e.g. `screen.getByText()`
- We can test the behaviour of these extracted nodes by using the jest matchers provided by the `@testing-library/jest-dom` library. E.g. `expect().toBeChecked()`.
- We can mimic user interaction by using methods provided by the `testing-library/user-event` library. An example method is `userEvent.click()`.
- Besides `.findByX`, RTL has the `waitFor()` asynchronous function that can be used to test asynchronous events such as an element being removed asynchronously or a component making an API call.



