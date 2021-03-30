# Writing Tests

Resources:

* Curriculum Doc: [Overview](http://curriculum-documentation.codecademy.com/Testing/overview/) on testing
* Curriculum Doc: [Test Standards](http://curriculum-documentation.codecademy.com/Content-Standards/test/)
* Curriculum Doc: [Mocha Tests](http://curriculum-documentation.codecademy.com/Testing/mocha/)
* [Mocha Documentation](https://mochajs.org/)
* [Structured.js Documentation](https://github.com/codecademy-engineering/structuredjs)
* Curriculum Doc: [WebSCT Tests](http://curriculum-documentation.codecademy.com/Testing/webSCT/)
* Curriclum Doc: [Generic Code Testing](http://curriculum-documentation.codecademy.com/Testing/generic-code/)
* [Regexr](https://regexr.com/)

## Mocha Tests

[Mocha](https://mochajs.org/) will be the testing framework you will use the most. To use Mocha tests, your workspace type should be set to `node`. Your workspace should also have an empty folder named `test` in the root folder (in some places, it may say you should create a folder called `tests`, but this is NOT true. Folder should be named `test`). This will be the place where test files per each checkpoint will be automatically loaded. You will be creating a test file per checkpoint and the test files load when the particular checkpoint is to be tested in the learning environment.

Select `Mocha Test` as the test type for the checkpoint. There are number of templates available, which you can see when you press the "Insert Template" button.

The "Basic Structured.js test" template uses [Structured.js](https://github.com/codecademy-engineering/structuredjs) which provides interface for verifying the structure of the JavaScript code. It is slightly more advanced than generic code testing with regular expressions. You can use Structured tests to match patters of code and can also use callback functions to check values of arguments passed to a function. The variable callbacks return JSON objects containing some extra information about the arguments, which is parsed using [Esprima](https://esprima.org/).

The "Basic Rewire.js test" template imports code from a specific file. Use this template to test values of specific variables that learners were asked to declare and set in the code.

Generally, I use the "Mocha Test with Rewire.js and Structured.js" because in most cases we will be doing a combination of getting learner defined functions and matching structure of code defined inside specific functions.

```js
let structureOne = function() {
   ellipse($x, $y, 30, 100);
};

let isMatchOne = Structured.match(code, structureOne);

assert.isOk(isMatchOne, `Did you use the \`ellipse()\` function in the \`draw()\` function?`)
});
```

In the code example above, we see `structureOne` function that defines the structure of code you are looking for. The `code` argument in `Structured.match()` function call contains the code block you are trying to find the structure of `structureOne` in. The `assert` statement checks if `isMatchOne` returned `true` and displays the error message if `isMatchOne` returned `false`.

When variables are written with `$`, you can refer to these in a callback function that can be refered to as the third argument of the `Structured.match()` function call and defined separately outside as a function.

```js
let varCallbacks = {
  "$x, $y, $w, $h": function(x, y, w, h){
    if(x.type === "BinaryExpression"){
      let expression = x.left.name + x.operator + x.right.value;
      if(expression !== 'width/2'){
        return {failure: "Did you set the x position of the `ellipse()` function as `width / 2`?"};
      }
    }else{
      return {failure: "Did you set the x position of the `ellipse()` function using the `width` variable?"};
    }

    if(y.type === "Identifier"){
      if(y.name !== "mouseY"){
        return {failure: "Did you set the y position of the `ellipse()` function as `mouseY`?"};
      }
    }else{
       return {failure: "Did you set the y position of the `ellipse()` function as `mouseY`?"};
    }


    if(w.value !== 100){
      return {failure: "Did you set the width of the `ellipse()` function as `100`?"};
    }

    if(h.value !== 200){
      return {failure: "Did you set the height of the `ellipse()` function as `200`?"};
    }

    return true;
  }
}

//ellipse(width / 2, height / 2, 100, 200);
let structureOne = function() {
  ellipse($x, $y, $w, $h);
};


let isMatchOne = Structured.match(learnerDeclaredDraw.toString(), structureOne, {varCallbacks});

assert.isOk(isMatchOne, varCallbacks.failure || `Did you use the \`ellipse()\` function in the \`draw()\` function?`);
});
```

Make sure that your callback function (`varCallbacks`) for the `Structured.match()` call is wrapped with curly brackets (`{}`).  

```js
let isMatchOne = Structured.match(learnerDeclaredDraw.toString(), structureOne, {varCallbacks});
```

Your `varCallbacks` function should map the string of all variables with `$` in your `structureOne`, separated by commas, to a function that takes those variables as arguments like below. If the string key is missing any of the variables with `$` in your structure function, it will not run the code inside the callback function.

```js
let varCallbacks = {
  "$x, $y, $w, $h": function(x, y, w, h){
    //variable test code here
  }
}
```

When you want to check what the Structured.js returns for the variables in the callback function, include the line below:

```js

let varCallbacks = {
  "$x, $y, $w, $h": function(x, y, w, h){
    return {failure: `${JSON.stringify(x)}`};
  }
}

```

This will fail the test and display the JSON object of what the x variable returns as a string format in the workspace when you run the checkpoint.

Within the callback function, you can check the types and values of each variable. This is also where you can set specific error messages for different patterns of learner code errors. In the `assert` statement, make sure to add `varCallbacks.failure` as the primary error message.

```js
assert.isOk(isMatchOne, varCallbacks.failure || `Did you use the \`ellipse()\` function in the \`draw()\` function?`);
```

Take a look at this [example mocha test](https://author.codecademy.com/exercises/a4906c69d9a523ec7dba2d783e120fac/drafts/5f89ffcfca13aa00121df836). 

* First checkpoint checks if the function exists in a particular JavaScript file, in this case **sketch.js**. It checks whether a `setup()` function exists and ensures that it is a type of `"function"`.
* Second checkpoints gets the `draw()` function call and selects the code block inside and including the `draw()` function. Since the `Structured.match()` function requires both arguments to be type of string, the `learnerDeclaredDraw` variable is converted to a string with `.toString()` method. The structure for an ellipse function with four arguments is defined inside the `structureOne` function and matchies whether this pattern occurs in the code block of `learnerDeclaredDraw.toString()`.
* Third checkpoint uses the `varCallback` function to check for the values of each variables defined in the `structureOne` function.

## WebSCT Test

From the curriculum documentation: "WebSCT Tests are written in JavaScript, and they run in the learner's browser, inside the context of their iframe window of the Web Browser component. For this reason, inside a WebSCT test, you have access to the full document object of a learner's page."

You can use WebSCT tests to check the value of variables that you've asked the learners to set for a checkpoint. This testing method can be useful as it doesn't care where in the file the particular line of code is written.

Say we have a code block like below and we want to test if the `ballPosX` variable is set to `50` when the condition of the if statement is met.

```js
if(keyIsPressed && key === ' ') {
   ballPosX = 50;
   ballPosY = 50;
   started = false;
}
```

Take a look at the test code below. Couple of things to note:
* The value of the variable is tested within the `document.body.onload` function block because this test will, by default, run before the canvas is done loading. We need to wait for the canvas to load, and only then, the code block above (which we will assume is inside the `draw()` function) has fully loaded.
* We manually set p5.js built in variable `keyIsPressed` to `true` and `key` to `' '`. The `keyIsPressed` variable should be set back to it's original value after the testing is done.

```js
var learnerVariableNameToTest = 'ballPosX';

// Test for variable identifier existence
try {
  ballPosX; 
} catch(e) {
  if (e instanceof ReferenceError) {
    throw new Error('Did you define a variable with the name `' + learnerVariableNameToTest + '`?');
  }
}

// Test variable values

document.body.onload = function(){
  keyIsPressed = true;
	key = ' ';

	var expectedValue = 50;
	if (ballPosX !== expectedValue) {
  	throw new Error('Did you give `' + learnerVariableNameToTest + '` a value of ' + expectedValue + '?');
	}

	keyIsPressed = false;
}
```

## Generic Code Test (Component Test)

This type of code test imports code from a particular file as text and uses regular expressions to find a particular pattern. It's helpful to use tools like [regexr](https://regexr.com/) to copy-paste the code you are testing with to derive the regular expression for the pattern you are looking for in the code.

## General Tips

When trying to figure out how to test a particular JavaScript code, it can be helpful to find a Codecademy lesson that covers the general topic and look at how the tests in a particular exercise is written. For example, if you are trying to figure out how to test a for loop, you can take a look at the Learn JavaScript course and see how the checkpoints in [Nested Loops](https://www.codecademy.com/courses/introduction-to-javascript/lessons/loops/exercises/for-loops-iii) exercise is written in Author.
