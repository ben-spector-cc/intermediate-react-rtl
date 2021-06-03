# Quiz

Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/quiz/) for guidance on writing quiz assessments.

<hr>

## Assessment 1 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.

What is React Testing Library (RTL)?

Response: React Testing Library is a library that allows one to test React components by mimicking how an actual user would interact with a particular component. It does not concern itself with the implementation details of the component.

- That's right!

Response: React Testing Library is a tool that allows one to check the current state and props passed to a React component.

- Not quite. RTL is a testing framework that allows one to check if a React component is working as intended from the end users perspective. it does not give any information on a component's state. 

Response: React Testing Library is a tool that allows one to test React components by checking whether the event handlers associated with a component are updating state as intended.

- Not quite. While React Testing Library is a tool for testing React components, it does not consider the actual implementation details of the component such as the event handlers associated with it.

Response: React testing library is a tool that manages routing in your react applications.

- Not quite. React Testing library does not give an app any routing capabilities. It is used to test whether a React component is working as intended from the user's perspective.

<hr>


## Assessment 2 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.

What is the correct command to install React Testing Library and add it as a developer dependency to your application?

Response: `npm install --save-dev @testing-library/react`

- That's right! The --save-dev command adds an installed package to the `"devDependency"` array in the **package.json** file. 

Response: `npm install @testing-library/react`

- Not quite. While that command will add React Testing Library to an application, to save it as a dependency in **package.json** the `--save-dev` flag must be included.

Response: `npm install react-testing-library`

- Not quite. React Testing Library is part of the `@testing-library` package. Also, the `--save-dev` flag is missing!

Response: `npm install @testing-library/react-testing-library --save-dev`

- Not quite. While the `--save-dev` flag will properly save this as a developer dependency, the name of the correct package is `@testing-library/react`.

<hr>


## Assessment 3 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.

Consider the component below:

```js
const Header = () => {
  return <h1>Hello friends</h1>
};
```

What is the correct code to include this component in a RTL unit test and then print out its contents?

Response: 

```js

import { render, screen } from '@testing-library/react';
import Header from './Header';

test('Should display content of Header component', () => {
  render(<Header/>);
  screen.debug();
});

```

- That's right! We first need to import `render` and `screen`. Then inside the `test()` we can call `render()` on the `Header` component followed by `screen.debug()` to print out the rendered element.

Response:

```js
import { screen } from '@testing-library/react';
import ReactDOM from 'react-dom';
import Header from './Header';

test('Should display content of Header component', () => {
    ReactDOM.render(<Header/>);
    screen.debug();
});

```

- Not quite. To include a component in a unit test, the `render()` method from React Testing Library should be used, not `ReactDOM.`

Response:

```js
import { render, screen } from '@testing-library/react';
import Header from './Header';

test('Should display content of Header component', () => {
  render();
  screen.debug(Header);
});

```

- Not quite. `screen.debug()` does not take in any arguments.

Response:

```js
import Header from './Header';

test('Should display content of Header component', () => {
  render(<Header/>);
  screen.debug();
});

```

- Not quite. `render()` and `screen` have not been imported from `@testing-library/react`.


<hr>


## Assessment 4 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.

Consider the component below:

```js
const App = () => {
    return (
        <div>
            <h1> What is your name? </p>)
            <label for="name">Enter Name:</label>
            <input id="name">
            <button type = "submit">Submit </button>
        </div>
   );
};

export default App;
```

Fill in the code to assert that the header is present in the document and that the button is enabled asynchronously:

- The header node is extracted and assigned to the `header` variable
- The button node is extracted and assigned to the `button` variable.
- An assertion is used to confirm the text of the header.
- An assertion is used to confirm the button is present in the document.

```js

import { render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import App from './App';

test('Should display content of Header component',()=>{
    render(<App/>)
    const header = __~screen.getByText('What is your name?')~__
    const button = __~screen.getByRole('button')~__
    // Confirm text of header
    __~expect(header).ToBeInTheDocument()~__
    // Confirm button node present in Document
    __~expect(button).ToBeEnabled()~__
})

```

- Hint : You can use the `screen.getByX` methods to extract DOM nodes.
- Hint : You can extract nodes by their roles as well, and not just text content.
- Hint : Remember jest-dom gives you access to additional jest assertions. You can figure out what they are doing by looking at the method name after the `expect.` prefix  E.g. `expect(checkbox).ToBeChecked()`, asserts that the checkbox is checked.
- Hint: What is the assertion that allows you to see if a DOM node is active?

Extra Responses: 
- expect(header).toBeEnabled();
- expect(button).toHaveTextContent('What is your name?');
- screen.query('button');
- screen.query('What is your name?');


<hr>


## Assessment 5 (FITC)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.

Consider the component below. It’s a simple form that asks users to enter some text:

```js
const Form = () => {
    return (
        <div>
            <label for="name">Enter Text:</label>
            <input id="name">
            <button type = "submit">Submit </button>
        </div>
    );
};
```

Fill in the code such that the unit test mimics a user typing `“Hello”` and clicking the submit button.

Response: 

```js

import { render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import __~userEvent~__ '@testing-library/user-event';

import Form from './Form';

test('Should display content of Header component', () => {
    render(<Form/>);
    const input = screen.getByRole('textbox');
    ___~userEvent.type(input, 'Hello')~___;
    const button = screen.getByRole('button');
    ___~userEvent.click(button)~___;
});

```

Hints :
- Before mimicking any interactions you have to import a special object from `@testing-library/user-event`.
- The userEvent object has numerous methods that mimic user interactions. E.g. `userEvent(link).hover()` mimics a user hovering over a link.
- What would be the correct method that mimics a ‘click’ event?

Extra Options:
- interact
- type(input, ‘Hello’)
- click(button)

<hr>

## Assessment 6 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.


Which query method variant returns `null`, if it can't find a DOM element and is useful for determining if an element is NOT present in the DOM?

Response: `.queryByX`

- That's right!

Response: `.findByX`

- Not quite. The `findByX` query methods are used to query for DOM elements that appear asynchronously.

Response: `.getByX`

- Not quite. The `.getByX` method variant throws an error if it can't find a DOM node.

<hr>

## Assessment 7 (FITC)

Prompt: 

Consider the component below:

```js
import { useState } from 'react';
 
const App = () => {
  const [text, setText] = useState('Mickey Mouse');

  const handleClick = () => {
    setTimeout(() => {
        setText('Donald Duck');
    }, 250);
  };
 
  return (
    <div>
      <h1>{text}</h1>
      <button onClick={handleClick}>click me</button>
    </div>
  );
};
 
export default App;
```

Fill in the code such that we properly assert that the text `'Donald Duck'` eventually appears in the DOM?

```js
import { render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import userEvent '@testing-library/user-event';
import App from './App'

test('Should show text content of Donald Duck', __~async~__ ()=>{
  render(<App/>)
  const button = screen.getByRole('button');
  userEvent.click(button);
  const header = __~await~__ screen__~.findByText~__('Donald Duck');
  expect(header).toHaveTextContent('Donald Duck');
})
```
Hints
- This keyword must be used before any function definition that contains asynchronous logic.
- This keyword must be used before any function call that is asynchronous
- This method from `screen` is used to query for elements that may appear asynchronously.

Extra options:
- .queryByText()
- .getByText()

## Assessment 8 (FITC)

Consider the component below which displays the text `"How is everybody doing?!"` and then removes that text asynchronously after the user clicks on the button:

```js
import { useState } from 'react';
 
const App = () => {
   const handleClick = () => {
       setTimeout(() => {
         document.querySelector('h1').remove();
       }, 250);
   };
   return (
       <div>
           <h1>How is everybody doing?!</h1>
           <button onClick = {handleClick}>Remove Header</button>
       </div>
   );
};
```

Fill in the code to assert that the header is removed asynchronously after a user clicks the button:

```js
import { __~waitFor~__, render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import userEvent '@testing-library/user-event';
import App from './App';

test('should remove header display', __~async~__ () => {
  render(<App/>);
  const button = screen.getByRole('button');
  userEvent.click(button);
   __~await~__ __~waitFor(() => expect(screen.queryByText('How is everybody!!')).toBeNull());~__
});

```

Hints:
- This function can be imported from RTL to wait for asynchronous actions.
- This keyword must be used before any function definition that contains asynchronous logic.
- This keyword must be used before any function call that is asynchronous
- This combination of a function from RTL with a `screen` query method can be used to asynchronously check for the removal of an element.

Extra Options:
- expect(screen.queryByText('How is everybody!!')).toBeNull();
- expect(screen.getByText('How is everybody doing?!')).toBeNull();
