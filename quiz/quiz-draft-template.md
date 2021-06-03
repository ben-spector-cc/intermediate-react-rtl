# Quiz

Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/quiz/) for guidance on writing quiz assessments.

<hr>

## Assessment 1 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.

What is React Testing Library (RTL)?

Response: React Testing Library is a library that allows one to test React components by mimicking how an actual user would interact with a particular component. It does not concern itself with the implementation details of the component.

- That's right!

Response: React testing library is a tool that allows one to check the current state and props passed to a React component.

- Not quite. RTL is a testing framework that allows one to check if a React component is working as intended from the end users perspective. it does not give any information on a component's state. 

Response: React testing library is a tool that allows one to test React components by checking whether the event handlers associated with a component are updating state as intended.

- Not quite. While React testing library is a tool for testing React components. It does not consider the actual implementation details of the component such as the event handlers associated with it.

Response: React testing library is a tool that manages routing in your react applications.

- Not quite. React Testing library does not give an app any routing capablitites. It is simply meant as a library to test whether a React component is working as intended.

<hr>


## Assessment 2 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.

What is the correct command to install React Testing Library and add it as a dependency to your application?

Response: `npm install --save-dev @testing-library/react`

- That's right!

Response: `npm install @testing-library/react`

- Not quite. While that command will add React Testing Library to an application, to save it as a dependency in `package.json` the `--save-dev` flag must be included.

Response: `npm install react-testing-library`

- Not quite. React Testing Library is part of the `@testing-library` package.

Response: `npm install @testing-library/react-testing-library --save-dev`

- Not quite. While close, the name of the correct package is `@testing-library/react`.

<hr>


## Assessment 3 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.

Consider the component below:

```js
const Header = ()=>{
    return <h1> Hello friends </h1>
}
```

What is the correct code to include this component in a RTL unit test and then print out its contents?

Response: 

```js

import { render, screen } from '@testing-library/react';
import Header from './Header'

test('Should display content of Header component',()=>{
    render(<Header/>)
    screen.debug()
})

```

- That's right!

Response:

```js
import { render, screen } from '@testing-library/react';
import Header from './Header'

test('Should display content of Header component',()=>{
    ReactDOM.render(<Header/>)
    screen.debug()
})

```

- Not quite. To include a component in a unit test, the `render()` method does not have to be prefaced with `ReactDOM.`

Response:

```js
import { render, screen } from '@testing-library/react';
import Header from './Header'

test('Should display content of Header component',()=>{
    render(<Header/>)
    screen.debug(Header)
})

```

- Not quite. `screen.debug()` does not take in any arguments.

Response:

```js
import Header from './Header'

test('Should display content of Header component',()=>{
    render(<Header/>)
    screen.debug(Header)
})

```

- Not quite. `render()` and `screen` have not been imported from `@testing-library/react`.


<hr>


## Assessment 4 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.

Consider the component below:

```js
const App = ()=>{
    return (
        <div>
            <h1> What is your name? </p>)
            <label for="male">Enter Name:</label>
            <input id="name">
            <button type = "submit">Submit </button>
        </div>
})

export default App
```

Fill in the code such that -

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
    const header = ______
    const button = ______
    // Confirm text of header
    ______
    // Confirm button node present in Document
})

```

- Hint : You can use the `screen.getByX` methods to extract DOM nodes.
- Hint : Remember jest-dom gives you access to addtional jest assertions. E.g. expect(node).ToBeChecked()

Response: 
```js
const header = screen.getByText('What is your name?')
```

Response: 

```js
const button = screen.getByRole('button')
```

Response: 

```js
expect(header).ToHaveTextContent('What is your name?')
```

Response: 

```js
expect(button).ToBeInTheDocument()
```

<hr>


## Assessment 5 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.

Consider the component below:

```js
const Form = ()=>{
    return (
        <div>
            <label for="male">Enter Text:</label>
            <input id="name">
            <button type = "submit">Submit </button>
        </div>}
```

What is the correct code that mimics a user typing 'Hello' and then pressing the sumbmit button?

Response: 

```js

import { render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import userEvent '@testing-library/user-event';

import Form from './Form'

test('Should display content of Header component',()=>{
    render(<Form/>)
    const input = screen.getByRole('textbox')
    userEvent.type(input, 'Hello')
    const button = screen.getByRole('button')
    userEvent.click(button)
})

```

- That's right!

Response:

```js
import { render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import userEvent '@testing-library/user-event';

import Form from './Form'

test('Should display content of Header component',()=>{
    render(<Form/>)
    const input = screen.getByRole('textbox')
    userEvent.type('Hello')
    const button = screen.getByRole('button')
    userEvent.click(button)
})

```

- Not quite. The first argument of `userEvent.type()` should be the input node.

Response:

```js
import { render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import userEvent '@testing-library/user-event';

import Form from './Form'

test('Should display content of Header component',()=>{
    render(<Form/>)
    const input = screen.getByRole('textbox')
    userEvent.type('Hello')
    const button = screen.getByRole('button')
    userEvent.press(button)
})

```

- Not quite. To press the button the correct method is `.click()`.

Response:

```js
import { render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';

import Form from './Form'

test('Should display content of Header component',()=>{
    render(<Form/>)
    const input = screen.getByRole('textbox')
    userEvent.type(input, 'Hello')
    const button = screen.getByRole('button')
    userEvent.click(button)
})

```

- Not quite. The `userEvent` object was not imported from `@testing-library/user-event`.

<hr>

## Assessment 6 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.


What query method variant returns `null`, if it can't find a DOM element.

Response: `.queryByX`

- That's right!

Response: `.findByX`

- Not quite. The `findByX` query methods are used to query for DOM elements that appear asynchronously.

Response: `.getByX`

- Not quite. The `.getByX` method variant returns `undefined` if it can't find a DOM node.

<hr>



## Assessment 7 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.

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
  )
};
 
export default App;
```

What is the correct code you'd use to assert that the text `'Donald Duck'` eventually appears in the DOM?

Response: 

```js

import { render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import userEvent '@testing-library/user-event';
import App from './App'

test('Should show text content of Donald Duck',async ()=>{
  render(<App/>)
  const button = screen.getByRole('button');
  userEvent.click(button);
  const header = await screen.findByText('Donald Duck');
  expect(header).toHaveTextContent('Donald Duck');
})

```

- That's right!

Response:

```js
import { render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import userEvent '@testing-library/user-event';
import App from './App'

test('Should show text content of Donald Duck',()=>{
  render(<App/>)
  const button = screen.getByRole('button');
  userEvent.click(button);
  const header = screen.findByText('Donald Duck');
  expect(header).toHaveTextContent('Donald Duck');
})

```

- Not quite. The `findByX` query methods return a promise and thus the callback function for the unit test should contain the `async/await` keywords.

Response:

```js
import { render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import userEvent '@testing-library/user-event';
import App from './App'

test('Should show text content of Donald Duck',()=>{
  render(<App/>)
  const button = screen.getByRole('button');
  userEvent.click(button);
  const header = screen.queryByText('Donald Duck');
  expect(header).toHaveTextContent('Donald Duck');
})

```

- Not quite. The `queryByText()` call would return null as it is not asynchronouse and the text `Donald Duck` appears asynchronously.

<hr>

## Assessment 8 (Multiple Choice Template)
Check out the [content standards](http://curriculum-documentation.codecademy.com/Content-Standards/multiple-choice/) for guidance on multiple choice assessments.

Consider the component below:

```js
import { useState } from 'react';
 
const App = () => {
   const handleClick = () => {
       setTimeout(() => {
         document.querySelector('h1').remove()
       }, 250);
   };
   return (
       <div>
           <h1>How is everybody!!</h1>
           <button onClick = {handleClick}>Remove Header</button>
       </div>
   );
};
```

What is the correct code you'd use to assert that the header is removed after a user clicks the button?

Response: 

```js

import { waitFor, render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import userEvent '@testing-library/user-event';
import App from './App'

test('should remove header display', async () => {
  render(<App/>)
  const button = screen.getByRole('button');
  userEvent.click(button);
   await waitFor(() => expect(screen.queryByText('How is everybody!!')).toBeNull())
});

```

- That's right!

Response:

```js
import { waitFor, render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import userEvent '@testing-library/user-event';
import App from './App'

test('should remove header display', async () => {
  render(<App/>)
  const button = screen.getByRole('button');
  userEvent.click(button);
  const header = await screen.findByText('How is everybody!!')
  expect(header).toBeNull())
});

```

- Not quite. The `findByX` query are used to query for nodes that appear asynchronously. It cannot be used to query for methods that are **removed** asynchronously.

Response:

```js
import { waitFor, render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import userEvent '@testing-library/user-event';
import App from './App'

test('should remove header display', () => {
  render(<App/>)
  const button = screen.getByRole('button');
  userEvent.click(button);
  setTimeout(()=>{
    expect(screen.queryByText('How is everybody!!')).toBeNull()
  }), 250)
});

```

- Not quite. While the assertion in the `setTimeout` callback would be fired after 250ms, setTimeout functions are not used to check whether a node is removed asynchronously.

<hr>

