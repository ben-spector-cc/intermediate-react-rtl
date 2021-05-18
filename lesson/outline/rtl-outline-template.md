# Lesson Name

Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/outline/) for guidance on writing lesson outline.

## Lesson Information

### Resource(s)

_Insert any resources you used while researching to help plan your lesson._

1. [Medium article 1](https://medium.com/dont-leave-me-out-in-the-code/react-testing-library-cd98e8ebf2cb)
2. [Medium article 2](https://medium.com/swlh/beginners-guide-to-react-testing-with-testing-library-97f23b4af40f)
3. [Medium article 3](https://avinashadluri.medium.com/a-guide-to-react-testing-with-react-testing-library-623cbbe57a22)
4. [RTL walkthrough](https://www.youtube.com/watch?v=ZmVBCpefQe8)
5. [Mocking an API request](https://www.youtube.com/watch?v=Ngj2f1n9pUw&t=459s)

### Learning Standards
Check out the [content standards](http://curriculum-documentation.codecademy.com/Resources/outcomes-standards-objectives/#learning-standards) for guidance on writing learning standards.

1. What is testing and why use React Testing Library?
2. Installing and importing `React Testing Library`.
3. Querying DOM nodes with RTL methods such as `queryByText`, etc.
4. The difference between the different types of query methods.
3. Installing and importing `user-event`.
4. Testing components by mimicking real user interactions with `fireEvent` and `user-event`.
6. Testing components with asynchronous methods.
7. Testing components that make API calls.

<hr>

## Exercise # 1: What is React Testing Library

### Which course outcomes will be covered by this exercise?

1. Learners will be able to understand the importance of unit tests in app development, and the guiding philosophy behind React Testing Library.

### Narrative Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/narrative/) for guidance on writing narratives for exercises._

1. Learners will be introduced to unit tests and their importance. Emphasize that unit tests serve as a way to verify that the code in an application is working as intended.
2. Learners will be introduced to React Testing Library which is a tool that allows one to test React components. Emphasize that RTL tests the React components in a way that simulate real user interactions.
3. Explain that the philosophy of testing components this way is because in actual production, the users of an app will not be concerned with the implementation details of the component. Just whether or not it works as intended.
4. Go through a simple RTL test with a react app to demonstrate this philosophy.

### What would you like to have in the workspace for this exercise? Share your plan below.

- Perhaps a simple react app that renders a list when an input form is filled out. This can be used to test the component.

<hr>

## Exercise # 2: Setting up React Testing Library

### Which course outcomes will be covered by this exercise?

1. Learners will be able to install and import React Testing Library.
2. Learners will be able to make a component available in a test using RTL's `render` function and check for its availability using `screen.debug()`.

### Narrative Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/narrative/) for guidance on writing narratives for exercises._


1. In order to use React Testing Library learners need to install it using the command `npm install --save-dev @testing-library/react`.
3. React Testing Library has a `render()` function that makes the component available in the unit test. `render()` can be imported like so: `import { render } from '@testing-library/react'`. Emphasize that `render()` takes in JSX as the argument.

```javascript
render(<h1>Hello</h1>);
```
4. The availability of this component can be verified using the method `screen.debug()` which prints out all the DOM contents.
5. `screen` is a special object which can be thought of as a representation of the browser window. `screen` can be imported like so: `import { screen } from '@testing-library/react'`.


### Checkpoints Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/checkpoint/) for guidance on writing narratives for exercises._

1. Import the `screen` and `render` values from `'react-testing-library'`.
2. Call the `render` function to make the react component available in a unit test.
3. Check the availability of this component using `screen.debug()`
4. Run the `npm test` command to see the results.

#### What is the purpose of these checkpoints?

1. So that learners know how to import the `render` and `screen` values from the `'react-testing-library'` package.
2. So that learners can implement the `render` function and make the intended component available in the unit test.
3. So that learners can view the rendered component with `screen.debug()`
4. Run a test


### What would you like to have in the workspace for this exercise? Share your plan below.

- Perhaps a simple React component with paragraph text. This is just so that learners can see how the `render` function works.

<hr>

## Exercise # 3: Querying with RTL

### Which course outcomes will be covered by this exercise?

1. Learners will be able to perform queries with RTL helper methods.
2. Learners will be able to implement the different jest matchers.

### Narrative Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/narrative/) for guidance on writing narratives for exercises._

1. Explain to learners that RTL has a bunch of useful helper methods that can be used to query for DOM elements.
2. There are two ways to perform these queries.
- Either by the helper methods returned by the `render` function. **Not to be emphasized**
```javascript
const { getByText } = render(<div>Baz</div>)
expect(getByText('Baz')).toBeInTheDocument()
```
- Or by using the `screen` object E.g. **Emphasize this method in the lesson**

```javascript
render(<div>Baz</div>);
expect(screen.getByText('Baz')).toBeInTheDocument();
```
3. Explain that for this lesson, we'll be focusing on the `getBy...` (The descriptions are taken from the RTL documentation) // Put in link of RTL library
    * `getBy...` :  Returns the matching node for a query, and throw a descriptive error if no elements match or if more than one match is found

    For multiple elements these are `getAllby...`

4. Showcase the difference between the different `getBy` query methods. E.g. the difference between `getbyText` and `getbyLabelText`. Emphasize that instead of memorizing all these, it is best to just look them up on the [docs](https://testing-library.com/docs/queries/about/).
5. Once learners know how to query DOM nodes with RTL, go through a few different jest matchers provided by `@testing-library/jest-dom`. Explain that these matchers are extremely helpful when looking at the state of DOM nodes in our unit tests. E.g. whether or not a button is disabled, or a checkbox is checked, etc.

    Once again, it is best to look these up on the [React Testing Library docs](https://github.com/testing-library/jest-dom) instead of memorizing them.
```javascript
expect(getByRole('button')).toBeDisabled();
expect(getByText('Option A')).toBeChecked();
```
6. Demonstrate how RTL gives a readable error message when a DOM component is not present or when a test has failed. The debug message can be printed with `screen.debug()`.

### Checkpoints Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/checkpoint/) for guidance on writing narratives for exercises._

1. Import `render` and `screen` 
2. Perform the different `getBy` query types on component using `screen.getByX` query functions.
- getByRole

3. Use the different query methods and jest matchers from `expect` to determine if a component is working as expected.
4. Run the `npm test` command to see the result

#### What is the purpose of these checkpoints?
1. So that learners understand how to perform queries on DOM nodes.
2. So that learners understand the difference between the different `getBy...` query types and know when to apply which.
3. So that learners can apply the different jest matchers to verify the state of the DOM.

### What would you like to have in the workspace for this exercise? Share your plan below.
- A simple react component with multiple div to demonstrate the different query types.

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/workspaces/) for guidance on writing narratives for exercises._

## Exercise # 4: Different Query methods

### Which course outcomes will be covered by this exercise?

1. Learners will understand the difference between different types of query methods `getBy..`, `findBy...`, `queryBy...`.

### Narrative Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/narrative/) for guidance on writing narratives for exercises._

1. Explain to learners that RTL has different categories of query methods besides `getBy...`. These methods are used for different scenarios.

3. Explain that the other two categories are (The descriptions are taken from the RTL documentation) // Put in link of RTL library
    * `queryBy...`: Returns the matching node for a query. Returns `null` if no elements match. This is useful for asserting an element that is not present.
    * `findBy...`: Returns a Promise which resolves when an element is found which matches the given query. The promise is rejected if no element is found or if more than one element is found after a default timeout of 1000ms. Emphasize that `findBy` is useful for querying for DOM nodes that'll eventually appear. e.g. nodes that display data from an API, modals, etc.

    For multiple elements these are `queryAllBy..`, `findAllBy...`

4. Demonstrate a few different examples of `findby...` and `queryby...` methods to show their difference. Emphasize that instead of memorizing all these, it is best to just look them up on the [docs](https://testing-library.com/docs/queries/about/).

### Checkpoints Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/checkpoint/) for guidance on writing narratives for exercises._

1. Use the `queryBy...` method to search for an exisiting DOM node and for a DOM node that is not present.
2. Use the `findBy...` method to search for nodes that'll eventually appear in the browser window. A modal or a timer function would probably be suitable.
3. Run the `npm test` command to see the result

#### What is the purpose of these checkpoints?
1. So that learners can implement all the different types of query methods.

### What would you like to have in the workspace for this exercise? Share your plan below.
- A react component with that displays API data, or a timer function that displays a DOM component after a fixed interval.

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/workspaces/) for guidance on writing narratives for exercises._

## Exercise # 5: Mimicking User Interactions

### Which course outcomes will be covered by this exercise?

1. Learners will be able mimic user interactions E.g. a mouse click, keystrokes, etc.

### Narrative Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/narrative/) for guidance on writing narratives for exercises._

1. RTL has different methods that can be used to mimic user interactions. E.g. `user-event` or `fireEvent`.
```javascript
test('type', () => {
  render(<textarea />);

  userEvent.type(screen.getByRole('textbox'), 'Hey There');
  expect(screen.getByRole('textbox')).toHaveValue('Hey There');
});
```

2. Emphasize that `user-event` is often preferred over `fireEvent` as it has greater capabilities.
3. The `user-event` library can be installed using `npm install --save-dev @testing-library/user-event`. Here are the [docs](https://github.com/testing-library/user-event).
3. Go through a few different user interactions to demonstrate these.

### Checkpoints Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/checkpoint/) for guidance on writing narratives for exercises._

1. Test a component that renders a checkbox.
2. Test a component that takes in a user form.
3. Test a component that takes in a button click.


#### What is the purpose of these checkpoints?

1. So that learners can use the `user-event` library method to mimic user interactions.

### What would you like to have in the workspace for this exercise? Share your plan below.

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/workspaces/) for guidance on writing narratives for exercises._

- A React application with multiple interactive elements e.g. a checkbox, a user form with a button.

## Exercise # 6: Asynchronous Testing with RTL

### Which course outcomes will be covered by this exercise?

1. Learners will be able to test components that render asynchronously. E.g. components where modals pop up if a button is clicked, etc.

### Narrative Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/narrative/) for guidance on writing narratives for exercises._

1. Some components require some time for all their content to render. E.g. components with modals, etc. In such cases the unit test has to be asynchronous. 

2. Jest has async methods e.g. `findBy...` or `waitFor...`that allows one to test for such interactions.
```javascript
//Taken from RTL docs
const button = screen.getByRole('button', { name: 'Click Me' })
fireEvent.click(button)
await screen.findByText('Clicked once')
fireEvent.click(button)
await screen.findByText('Clicked twice')
```

```javascript
//Taken from https://react-testing-examples.com/
it('renders personalized greeting', async () => {
  // Render new instance in every test to prevent leaking state
  const { getByText } = render(<HelloMessage name="Satoshi" />);

  await waitForElement(() => getByText(/hello Satoshi/i));
});
```
3. These tests must use the `async` keyword for them to work. Emphasize that `await` is only allowed inside an async function.

```javascript
// Taken from https://avinashadluri.medium.com/a-guide-to-react-testing-with-react-testing-library-623cbbe57a22
describe('App', ()=>{ test('renders App Component', async() => { 
        render(<App/>)
        fireEvent.click(screen.getByText('Load Greeting'))
        await waitFor(() => screen.getByRole('alert'))
        expect(screen.getByRole('alert')).toHaveTextContent('Oops, failed to fetch')
        expect(screen.getByRole('button')).not.toHaveAttribute('disabled')
    })
})
```

### Checkpoints Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/checkpoint/) for guidance on writing narratives for exercises._

1. Test a component that renders a modal.
2. Test a component that renders an alert or renders an element after a certain time has passed.

#### What is the purpose of these checkpoints?
1. So that learners can implement async methods to test components. E.g. Component that display a pop up window up after the user clicks a button.

### What would you like to have in the workspace for this exercise? Share your plan below.

- Perhaps a component that displays a modal or an alert message.

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/workspaces/) for guidance on writing narratives for exercises._

## Exercise # 7: Testing Components That Make API Calls

### Which course outcomes will be covered by this exercise?

1. Learners will be able to test React components that make calls to an external API.
### Narrative Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/narrative/) for guidance on writing narratives for exercises._

1. Remind learners that oftentimes components will make an API call and render a div based on whether or not the API call was successful.
2. Explain that in real life testing, components should not hit an actual endpoint. Instead, their functionality should be mocked with Jest.
3. Jest can be used to mimic an API call that returns a mock value. These can be done with the `jn.fn()` method. Use a mock api call to mimic a fetch request to demonstrate this. Assume that learners already know how to do this or link to the codecademy Jest module.
4. Emphasize that since API calls are asynchronous, these unit tests should be asynchronous too.
5. React Testing library also provides additional methods to ensure we correctly called upon our mock API call.

```javascript
// Taken from https://polvara.me/posts/how-to-test-asynchronous-methods
import React from ‘react’;
import { render, screen, waitFor } from "@testing-library/react"; // highlight-line
import Index from "./Index";
import "jest-dom/extend-expect";
import { fetchPosts } from "./api/posts";

jest.mock("./api/posts");

// highlight-next-line
test("We show a list of posts", async () => {
  // highlight-start
  const posts = [{ id: 1, title: "My post", url: "/1" }];
  fetchPosts.mockResolvedValueOnce(posts);
  // highlight-end
  render(<Index />);
  expect(screen.getByText("Loading...")).toBeInTheDocument();
  expect(fetchPosts).toHaveBeenCalledTimes(1);
  expect(fetchPosts).toHaveBeenCalledWith();
  // highlight-start
  await wait(() => expect(screen.getByText("My Posts")).toBeInTheDocument());
  posts.forEach((post) =>
    expect(screen.getByText(post.title)).toBeInTheDocument()
  );
  // highlight-end
});
```

### Checkpoints Summary

_Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/checkpoint/) for guidance on writing narratives for exercises._

1. Mock an API call with Jest
2. Use RTL methods to see if the data retrieved from the API call was successfully rendered inside the component.
3. Check if the mock API calls took the appropriate arguments and was called the desired number of times.

#### What is the purpose of these checkpoints?
1. So that learners can test components that call an external API.

### What would you like to have in the workspace for this exercise? Share your plan below.

- A react component that makes a fetch request. Code that fakes a fetch request in the jest `mock` folder.


##  Lesson Review

### Narrative Summary

Congrats on completing this lesson! You can now use the React Testing Library to validate the behavior of your React components. 

Let's review what we learned and practiced in this lesson:

- React testing library allows us to test react components by mimicking real user interactions.
- In order to make your component available in the unit test, you have to use the `render()` function.
- RTL has built in query methods (`getBy`,`findBy`,`queryBy`) that allows you to extract the DOM nodes from your components.
- You can test the behaviour of these extracted nodes by using the jest matchers provided by the `@testing-library/jest-dom` library.
- You can mimic user interaction by using methods provided by the `testing-library/user-event` library.
- Components that make API calls can be tested by using jest to make a fake API call.

### Instructions


