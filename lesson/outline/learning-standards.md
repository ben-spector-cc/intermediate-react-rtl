## What is React Testing Library?

### Learning Standard Text

React Testing Library (RTL) is a library for testing React applications. RTL provides tools for testing the end-user's experience rather than the implementation and logic of the underlying React components. RTL is guided by the following principle, which can be found in the [documentation](https://testing-library.com/docs/react-testing-library/intro/), "The more your tests resemble the way your software is used, the more confidence they can give you."

React Testing Library is NOT a test runner or framework, nor is it specific to any test runner or framework (though Jest is recommended).

### Tags

JavaScript, React, Unit Testing

### Additional Notes / Resources Used

- https://testing-library.com/docs/react-testing-library/intro/

<hr>

## Installing React Testing Library

### Learning Standard Text

If you are using `create-react-app` to initialize your React project, the React Testing Library (RTL) will already be included. 

To manually install RTL with `npm`, use the following command:

```sh
npm install --save-dev @testing-library/react
```

* Though not required, the `--save-dev` flag will add this library as a development dependency rather than a production dependency.


### Tags

JavaScript, React, Unit Testing

### Additional Notes / Resources Used

- https://testing-library.com/docs/react-testing-library/intro/

<hr>

## Installing React Testing Library

### Learning Standard Text

If you are using `create-react-app` to initialize your React project, the React Testing Library (RTL) will already be included. 

To manually install the RTL with `npm`, use the following command (though not required, the `--save-dev` flag will add this library as a development dependency rather than a production dependency):

```sh
npm install --save-dev @testing-library/react
```

Once installed, RTL can be imported into your project:

```js
// app.test.js
import { render, screen } from '@testing-library/react';
```

### Tags

JavaScript, React, Unit Testing

### Additional Notes / Resources Used

- https://testing-library.com/docs/react-testing-library/intro/

<hr>

## Rendering Components

### Learning Standard Text

The React Testing Library (RTL) provides a `render()` method for rendering React components in the testing environment. Once rendered in this way, you can easily view the rendered DOM tree using the `debug()` method of the `screen` object.

In this example, the RTL is unaware of the underlying implementation of the `<App />` component and only sees the output (an `<h1>` element).
```js
import { render, screen } from '@testing-library/react';

import { App } from './App.js'
/* App.js

export const App = () => {
  return (
    <h1>Hello World</h1>
  )
}
*/

it('should render a component', () => {
  render(<App />);
  screen.debug();

  /* 
  Jest Output:
    <body>
      <div>
        <h1>
          Hello World
        </h1>
      </div>
    </body>
  */
});
```

### Tags

JavaScript, React, Unit Testing

### Additional Notes / Resources Used

- https://testing-library.com/docs/react-testing-library/example-intro
- https://www.robinwieruch.de/react-testing-library

<hr>

## Querying the Screen

### Learning Standard Text

The `screen` object from the React Testing Library (RTL) provides methods for querying the rendered elements of the DOM in order to make assertions about their text content, attributes, etc....

In this example, the methods `screen.getByText()` and `screen.getByRole()` are used to demonstrate two different ways you can query the DOM for the same element. 

```js
import { render, screen } from '@testing-library/react'
import '@testing-library/jest-dom';

import { App } from './App.js'
/* App.js

export const App = () => {
  return (
    <h1>Hello World</h1>
  )
}
*/

it('should render a header', () => {
  render(<App />);
  expect(screen.getByText("Hello World")).toBeInTheDocument();
  expect(screen.getByRole("header")).toHaveTextContent("Hello World");
});
```

The assertion matcher methods `expect('...').toBeInTheDocument()` and `expect('...').toHaveTextContent()` are from the `@testing-library/jest-dom` library. It is common for this library to be used alongside the React Testing Library.

### Tags

JavaScript, React, Unit Testing

### Additional Notes / Resources Used

- https://testing-library.com/docs/react-testing-library/example-intro
- https://www.robinwieruch.de/react-testing-library
- https://testing-library.com/docs/queries/about

<hr>

## Events

### Learning Standard Text

The `@testing-library/user-event` library is an extension of `@testing-library` that provides tools for simulating user interactions with the DOM. The provided `userEvent` object and the methods it contains can be used to simulate clicks, typing, and much more.

In this example, the `userEvent.type()` method is used to simulate a user typing the text `'Hello World!'` into the rendered text box.

```js
import React from 'react'
import {render, screen} from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import '@testing-library/jest-dom';

it('should change the text of the input', () => {
  render(<textarea />);

  userEvent.type(screen.getByRole('textbox'), 'Hello World!')
  expect(screen.getByRole('textbox')).toHaveValue('Hello World!')
})
```

### Tags

JavaScript, React, Unit Testing

### Additional Notes / Resources Used

- https://github.com/testing-library/user-event#typeelement-text-options

<hr>

## queryBy* variants

### Learning Standard Text

When using the React Testing Library (RTL) to determine if an element is NOT present in the rendered DOM, the `screen.queryBy*` variants should be used over their `screen.getBy*` counterparts.

If the queried element cannot be found, the `screen.getBy*` variants will throw an error causing the test to fail. `screen.queryBy*` will instead return `null`.

In this example, using `screen.getByRole('foobar')` will result in an error and a failing test while using `screen.queryByRole('foobar')` will result in a passing test.

```js
// Assume that there is no element with the role 'foobar'

// getByRole('foobar') will throw an error if no element is found
expect(screen.getByRole('foobar')).toBeNull();

// queryByRole simply returns null if no element is found
expect(screen.queryByRole('foobar')).toBeNull();
```

### Tags

JavaScript, React, Unit Testing

### Additional Notes / Resources Used

- https://testing-library.com/docs/react-testing-library/example-intro
- https://www.robinwieruch.de/react-testing-library
- https://kentcdodds.com/blog/common-mistakes-with-react-testing-library#using-query-variants-for-anything-except-checking-for-non-existence

<hr>

## findBy* variants

### Learning Standard Text

When using the React Testing Library (RTL) to query the rendered DOM for an element that will appear as a result of an asynchronous action (for example, after some API fetch occurs), the `screen.findBy*` variants should be used instead of the `waitFor()` method and the `screen.getBy*` variants.

In this example, a header with the test `'Goodbye'` will appear after a Promise resolves. While both approaches will accurately find this element, the approach that uses `screen.findByText()` is more concise and can produce better error messages in the event that the element is not found.

```js
// Wait for an element with the test 'Goodbye' to appear before proceeding.
const header = await waitFor(() =>
  screen.getByText('Goodbye')
)

// This does the same as above, but more succinctly and with better error logging
const header = await screen.findByText('Goodbye');
```

### Tags

JavaScript, React, Unit Testing

### Additional Notes / Resources Used

- https://testing-library.com/docs/react-testing-library/example-intro
- https://www.robinwieruch.de/react-testing-library
- https://kentcdodds.com/blog/common-mistakes-with-react-testing-library#using-waitfor-to-wait-for-elements-that-can-be-queried-with-find

<hr>
