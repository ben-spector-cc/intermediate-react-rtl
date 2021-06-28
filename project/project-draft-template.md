# React Testing Library

_Read the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/project/) for expectations for outline and check out [this example](https://docs.google.com/document/d/1SfHo68LS_w38ur7FJCxyKhBX5aOk_odY1GBYD_Y2kmE/edit)._

## Project Information

### Resource(s)

Take a look at the react app in the browser. It displays a textbox with a picture of a cat beneath it. 

When users type into the textbox, a copy of the text will appear below the cat image. This is the `'copycat'` state of the app. Clicking on the image will make tape appear over the cat's mouth. This is the `'quietcat'` state of the app. In this state, when you type into the textbox the text will no longer appear below the cat image. Any existing text below the image will also disappear. You can toggle between the two states by clicking on the cat image.

For this project, you will use the *React Testing Library* to write component tests for this application. Take a moment to familiarize yourself with the app before you get started on the project. 

1. Resource

### Description

### Objective(s)

### Thumbnail Image URL: 

_Select a stock photo from a website like [pexels.com](https://www.pexels.com/) or [unsplash.com](https://unsplash.com/). Resize the image to a width of 500px or less, using your own software or an online service like [resizeimage.net](https://resizeimage.net/)._

<hr>

## Task Group #1: _Insert Title_

### Task #1

- Before we can start writing our tests, we first have to install the necessary modules. In your terminal, use `npm` to install the following packages.
1. `@testing-libary/react`
2. `@testing-library/jest-dom`
3. `@testing-library/user-event`

Remember to save the packages as a dev dependency by using the `--save-dev` flag. Once this is done, head over to the **package.json** file and look at the devDependencies section to confirm that you have installed all the necessary packages.

### Hint

You can install multiple packages like so:

```js
npm install --save-dev package1 package2 ...
```

<hr>

### Task #2

- Now that we have all the neccessary packages, we can finally start testing our application. Go to **CopyCat.test.js**.  We will test our `CopyCat` component here. Make all the neccessary imports listed below.

1. `React` from `react`
2. `render` and `screen` from `@testing-libary/react`
3. The `userEvent` object from `@testing-library/user-event`
4. The entire `@testing-library/jest-dom` module


### Hint

Here are all the imports:

```js
import React from 'react';
import {render,screen} from @testing-library/react;
import userEvent from @testing-library/user-event;
import @testing-library/jest-dom;
```

<hr>

### Task #3

- 
  Now take a look at the **CopyCat.js** file. It is a presentational component which accepts a bunch of props from the `CopyCatContainer` component. 
  
  Observe in line 13 that it takes a `name` prop which, if provided, is displayed in the header as 'Copy Cat [name]'. Our first unit test will verify that this component is correctly accepting this `name` prop and rendering it in the DOM. Before we can do that, though, we first have to include this component in our unit test.

  Use RTL's `render()` method to render the `CopyCat` component in the unit test. Pass it the following props:

  1. `name` : `'Mack'`
  2. `value` : an empty string.
  3. `handleChange`: an empty arrow function.
  4. `toggleTape`: an empty arrow function.
  5. `isCopying`: `true`

### Hint

Your code should look like the following:

```js
test('test description' ()=>{
    render(
        <Component
            prop1 = {value1}
            prop2 = {value2}
            prop3 = {value3}
        />
    )
})
```

<hr>

## Task Group #2: _Insert Title_

### Task #4

- Now that the component is included in the unit test, verify that the header displays the text content `'Copy Cat Mack'`. Use an appropriate query method from the `screen` object and an apprpriate matcher from the `jest-dom` library to test this. Here are the links to the [query methods](https://testing-library.com/docs/queries/about/) object and to the [jest-dom](https://github.com/testing-library/jest-dom) library

Use `npm test` to check whether your test passes.

### Hint

Your code should look something like this:

```js
const node = screen.getByText('some text')
expect(node).toBeInTheDocument()
```

<hr>

### Task #5

- Go back to **CopyCat.js** and take a look at the `isCopying` prop. When `isCopying` is set to `true`, the text from the input form will be rendered in the paragraph below the cat image.

  The second test in **CopyCat.test.js** will verify this functionality. Your tasks are to do the following.

  1. Render the `CopyCat` component in the unit test. The prop values are the same as the first text, except for the `value` prop which will be changed to `'Here is an input'`.
  2. Extract the input node using the appropriate query method.
  3. Use the `.toHaveDisplayValue()` jest matcher to verify that the input value displayed in the textbox is the same as the `value` prop passed to the `CopyCat` component.
  3. Extract the paragraph node using the appropriate query method.
  5. Use an appropriate jest matcher to verify that the paragraph node has the same text as the input value. 

  Use `npm test` to check whether your test passes.
  
> Note: `.byText()` methods cannot grab input text values. Just text from nodes with a text property. E.g. paragraphs. So you don't have to worry about your query methods accidentally grabbing the text from the input node instead of the paragraph.


### Hint

Your solution should have the following steps:

First render the `CopyCat` component in your test.
```js
test('Should display input text in paragraph when isCopying is set to true',()=>{
  render(
    <CopyCat
    value = {"Here is an input"}
    handleChange = {()=>{}}
    toggleTape = {()=>{}}
    isCopying = {true}
    />);
})
```

Once this is done use the `.byRole('textbox')` method to extract the input node and the `.toHaveDisplayValue('input text')` assertion to confirm the input value.

Finally, extract the paragraph node by the `.getByText('some text')` method and confirm that it is present in the DOM with the `.toBeInTheDocument()` assertion.

<hr>

### Task #6

- When `isCopying` is set to `false`, the text from the input form will no longer be rendered in the paragraph below the cat image.

  The third test in **CopyCat.test.js** will see that this is functioning properly. Your tasks are to do the following.

  1. Render the `CopyCat` component in the unit test. The prop values are the same as the second text, but set `isCopying` to `false`.
  2. Extract the input node using the appropriate query method.
  3. Use the `.toHaveDisplayValue()` jest matcher to verify that the input value displayed in the textbox is the same as the `value` prop passed to the `CopyCat` component.
  4. Extract the paragraph node using the appropriate query method. Hint: What is the query variant that returns `null` if a node is not present?
  5. Use an appropriate jest matcher to verify that the paragraph node is `null`.

  Use `npm test` to check whether your test passes.

### Hint
Your solution should have the following steps:

First render the `CopyCat` component in your test.
```js
test('Should not display input text in paragraph when isCopying is set to false',()=>{
  render(
    <CopyCat
    value = {"Here is an input"}
    handleChange = {()=>{}}
    toggleTape = {()=>{}}
    isCopying = {false}
    />);
})
```

Once this is done use the `.byRole('textbox')` method to extract the input node and the `.toHaveDisplayValue('input text')` assertion to confirm the input value.

Finally, extract the paragraph node by the `.queryByText('some text')` method and confirm that it is not present in the DOM with the `.toBeNull()` assertion.

<hr>

### Task #7

- 
  Now take a look at the **CopyCatContainer.js** file. It is the container component which contains all the logic for the `CopyCat` component and is responsible for rendering `CopyCat`. More specifically it contains the following which are passed to the `CopyCat` component as props.
  1. The `input` state which stores the text data of the input field.
  2. The `handleChange()` function for the input. This sets the `input` value to the value in the textbox.
  3. The `isCopying` state which is the boolean that determines the state of the app. It is `true` when the app is in the `'copycat'` state and `false` when it's in the `'quietcat'` state.
  2. The `toggleTape()` function which is passed as the `onClick` handler for the cat image. It flips the boolean value of `isCopying`. This is what swithces the apps state from `'quietcat'` to `'copycat'`.
  
  Go to the **CopyCatContainer.test.js** file. We will test the `CopyCatContainer` component here. Before we do that though we have to make the necessary imports. Import all the required modules needed to test the `CopyCatContainer` component.

### Hint

Here are all the imports:

```js
import React from 'react';
import {render,screen} from @testing-library/react;
import userEvent from @testing-library/user-event;
import @testing-library/jest-dom;
```

<hr>

### Task #8

- The first test in **CopyCatContainer.test.js** should check whether the app is correctly rendering the user input below the cat image whenever the user types.

  Write a test that mimics a user typing the text `'Hello World!'` into the textbox. Then check to see if the text is mimicked below the cat image.

  Remember to include the `CopyCatContainer` component in your test with the `render()` method. Note that it does not take in any props. You can use the `userEvent` object from the `testing-library/user-event` to mimic user interactions.
  
  Use `npm test` to check whether your test passes.

### Hint
To simulate the user typing into a textbox, you might write something like this:

```js
const input = screen.getByRole('textbox');
userEvent.type(input, 'some text');
```

Then, use the `.getByText()` query method and the `.toBeInTheDocument()` matcher to confirm that the text appears in the document.

<hr>

### Task #9

  Look at the CopyCat app in the browser. Type something in the textbox and then click on the cat image such that tape appear over the cat's mouth. Observe that the copied text dissapears. Also notice that there is a slight lag until the tape appears, implying that this is an asynchronous event.
  
  The second test checks this functionality and whether the user input disappears when the tape appears.

  However, before we can write out our test, we have to make the necessary imports required for asynchronouse testing.

  On top of the `CopyCatContainer.test.js` file import the `waitFor` method from `@testing-library/react`


### Hint

Your code should look like the following:

```js
import {screen,render,waitFor} from '@testing-library/react';
```

<hr>

### Task #10

- 
  Now that we've made the necessary imports, write a test that checks whether the user input is first displayed and then disappears when the user clicks the cat image and sets it to the `'quietcat'` state.

  You have to do the following:

  1. Render the `CopyCatContainer` component in the unit test.
  2. Mimic a user typing the text `'My mouth is shut'` into the textbox.
  3.Extract the paragraph node and verify that the text is present (since we haven't clicked on the cat yet).
  4. Extract the image node and mimic a user clicking on it. You can use the [.byAltText()]() query method to to extract image nodes by their `alt` text attributes. The `alt` is `'copycat'` when there is no tape over the cat's mouth.
  5. Use the `waitFor()` method to verify that the text eventually dissapears when the user switches state from `'copycat'` to `'quietcat'`.

  This is an asynchronouse test, so remember to include the `async/await` keywords. Use `npm test` to check whether your test passes.

### Hint

Look at the solution below:

```js
test('Should remove copied text after removing tape',async ()=>{
  render(<CopyCatContainer/>);
  // Simulate user typing in textbox
  const input = screen.getByRole('textbox');
  userEvent.type(input,'My mouth is shut');
  // Assert that input text appears in paragraph
  const par = screen.getByText('My mouth is shut')
  expect(par).toBeInTheDocument()
  // Simulate clicking on copycat image (This will switch to quietcat image).
  const copyCatImage = screen.getByAltText('copycat')
  userEvent.click(copyCatImage)
  // Assert that paragraph text will eventually disappear when copycat image is switched over to quietcat
  await waitFor(()=> expect(screen.queryByText('My mouth is shut')).toBeNull());
})
```

<hr>

### Task #11

-
  Look at the app again. Click on the cat image and set it to the `'quietcat'` state. Type something in the textbox and then click on the cat image again such that it reverts back to `'copycat'`. Observe that the textbox input reappears below the cat image.
  
  The third and final test checks this functionality i.e. whether the user input reappears when the tape is removed from the cat's mouth. This test is a bit longer than the rest, so we will write it in two steps. 
  
  The first steps involves the user typing into the text box, clicking on the cat image and setting it to 'quietcat'. Your task is to replicate this user interaction by doing the following.

  1. Render the `CopyCatContainer` component in the unit test.
  2. Extract the image node and mimic a user clicking on it.
  3. Verify that that the image switched over to the `'quietcat'` state. The `alt` attribute is `'quietcat'` when there is tape over the cat's mouth.
  > Note: you have to use the `.find` query variant of `.byAltText()` to extract the `'quietcat'` image, since this is asynchronous.
  4. Mimic a user typing the text `'Eventually this will appear'` into the textbox.
  5. Verify that the text **isn't** copied. Once again, what is the query method variant that returns `null`?

  Remember to include the `async/await` keywords. Use `npm test` to check whether your test passes up to this point.


### Hint

Look at the solution below:

```js
test('Should display copied text after removing tape',async ()=>{
  render(<CopyCatContainer/>);
  // Simulate user clicking on copycat image
  let copyCatImage = screen.getByAltText('copycat')
  userEvent.click(copyCatImage)
  // Make sure image changes to quietcat (Cat with tape over mouth) 
  const quietCatImage = await screen.findByAltText('quietcat')
  // Simulate user typing in textbox
  const input = screen.getByRole('textbox');
  userEvent.type(input,'Eventually this will appear');
  // Assert that text is not copied when image is of quietcat
  const emptyPar = screen.queryByText('Eventually this will appear');
  expect(emptyPar).toBeNull();

})

```


<hr>

### Task #12

-
  For the remainder of the test, your task is to do the following. 

  1. Mimic the user clicking on the `'quietcat'` image.
  2. Verify that that the image once again switches over to the `'copycat'` state.
  3. Verify that that the text `'Eventually this will appear'` is rendered in the DOM when the app is set to the `'copycat'` state.

  Remember to include the `async/await` keywords. Use `npm test` to check whether your test passes.

### Hint

Look at the solution below:

```js
test('Should display copied text after removing tape',async ()=>{
  render(<CopyCatContainer/>);
  // Simulate user clicking on copycat image
  let copyCatImage = screen.getByAltText('copycat')
  userEvent.click(copyCatImage)
  // Make sure image changes to quietcat (Cat with tape over mouth) 
  const quietCatImage = await screen.findByAltText('quietcat')
  // Simulate user typing in textbox
  const input = screen.getByRole('textbox');
  userEvent.type(input,'Eventually this will appear');
  // Assert that text is not copied when image is of quietcat
  const emptyPar = screen.queryByText('Eventually this will appear');
  expect(emptyPar).toBeNull();
  // Simulate clicking on quietcat image
  userEvent.click(quietCatImage);
  // Make sure image switches over to copycat
  copyCatImage = await screen.findByAltText('copycat')
  //Assert that input text appears in paragraph
  const par = screen.getByText('Eventually this will appear');
  expect(par).toBeInTheDocument()
})
```

<hr>

