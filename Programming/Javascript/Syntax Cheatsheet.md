## JavaScript Basics

Set of JavaScript basic syntax to add, execute and write basic programming paradigms in Javascript 

### On Page Script

Adding internal JavaScript to HTML

```markup
<script type="text/javascript"> //JS code goes here </script>
```

Copy

### External JS File

Adding external JavaScript to HTML

```markup
<script src="filename.js"></script>
```

Copy

### Functions

JavaScript Function syntax

```javascript
function nameOfFunction() {
    // function body 
}
```

Copy

### DOM Element

Changing content of a DOM Element

```javascript
document.getElementById("elementID").innerHTML = "Hello World!";
```

Copy

### Output

This will print the value of a in JavaScript console

```javascript
console.log(a);
```

Copy

## Conditional Statements

Conditional statements are used to perform operations based on some conditions.

### If Statement

The block of code to be executed, when the condition specified is true.

```javascript
if (condition) {
    // block of code to be executed if the condition is true
}
```

Copy

### If-else Statement

If the condition for the if block is false, then the else block will be executed.

```javascript
if (condition) {
    // block of code to be executed if the condition is true
} else {
    // block of code to be executed if the condition is false
}
```

Copy

### Else-if Statement

A basic if-else ladder

```javascript
if (condition1) {
    // block of code to be executed if condition1 is true
} else if (condition2) {
    // block of code to be executed if the condition1 is false and condition2 is true
} else {
    // block of code to be executed if the condition1 is false and condition2 is false
}
```

Copy

### Switch Statement

Switch case statement in JavaScript

```javascript
switch (expression) {
    case x:
        // code block
        break;
    case y:
        // code block
        break;
    default:
        // code block
}
```

Copy

## Iterative Statements (Loops)

Iterative statement facilitates programmer to execute any block of code lines repeatedly and can be controlled as per conditions added by the programmer.

### For Loop

For loop syntax in javascript

```javascript
for (initialization; condition; incrementation;) {
    // code block to be executed
}
```

Copy

Example:

```javascript
for (let i = 0; i < 5; i++) {
    text += "Iteration number: " + i + "<br>";
}
```

Copy

### While Loop

Runs the code till the specified condition is true

```javascript
while (condition) {
    // code block to be executed
}
```

Copy

### Do While Loop

A do while loop is executed at least once despite the condition being true or false

```javascript
do {
    // run this code in block
    i++;
} while (condition);
```

Copy

## Strings

The string is a sequence of characters that is used for storing and managing text data.

### charAt method

Returns the character from the specified index.

```javascript
str.charAt(3)
```

Copy

### concat method

Joins two or more strings together.

```javascript
str1.concat(str2)
```

Copy

### index of method

Returns the index of the first occurrence of the specified character from the string else -1 if not found.

```javascript
str.indexOf('substr')
```

Copy

### match method

Searches a string for a match against a regular expression.

```javascript
str.match(/(chapter \d+(\.\d)*)/i;)
```

Copy

### replace method

Searches a string for a match against a specified string or char and returns a new string by replacing the specified values.

```javascript
str1.replace(str2)
```

Copy

### search method

Searches a string against a specified value.

```javascript
str.search('term')
```

Copy

### split method

Splits a string into an array consisting of substrings.

```javascript
str.split('\n')
```

Copy

### substring method

Returns a substring of a string containing characters from the specified indices.

```javascript
str.substring(0,5)
```

Copy

## Arrays

The array is a collection of data items of the same type. In simple terms, it is a variable that contains multiple values.

### variable

Containers for storing data.

```javascript
var fruit = ["element1", "element2", "element3"];
```

Copy

### concat method

Joins two or more arrays together.

```javascript
concat()
```

Copy

### indexOf method

Returns the index of the specified item from the array.

```javascript
indexOf()
```

Copy

### join method

Converts the array elements to a string.

```javascript
join()
```

Copy

### pop method

Deletes the last element of the array.

```javascript
pop()
```

Copy

### reverse method

This method reverses the order of the array elements.

```javascript
reverse()
```

Copy

### sort method

Sorts the array elements in a specified manner.

```javascript
sort()
```

Copy

### toString method

Converts the array elements to a string.

```javascript
toString()
```

Copy

### valueOf method

returns the relevant Number Object holding the value of the argument passed

```javascript
valueOf()
```

Copy

## Number Methods

JS math and number objects provide several constant and methods to perform mathematical operations.

### toExponential method

Converts a number to its exponential form.

```javascript
toExponential()
```

Copy

### toPrecision method

Formats a number into a specified length.

```javascript
toPrecision()
```

Copy

### toString method

Converts an object to a string

```javascript
toString()
```

Copy

### valueOf method

Returns the primitive value of a number.

```javascript
valueOf()
```

Copy

## Maths Methods

### ceil method

Rounds a number upwards to the nearest integer, and returns the result

```javascript
ceil(x)
```

Copy

### exp method

Returns the value of E^x.

```javascript
exp(x)
```

Copy

### log method

Returns the logarithmic value of x.

```javascript
log(x)
```

Copy

### pow method

Returns the value of x to the power y.

```javascript
pow(x,y)
```

Copy

### random method

Returns a random number between 0 and 1.

```javascript
random()
```

Copy

### sqrt method

Returns the square root of a number x

```javascript
sqrt(x)
```

Copy

## Dates

Date object is used to get the year, month and day. It has methods to get and set day, month, year, hour, minute, and seconds.

### Pulling Date from the Date object

Returns the date from the date object

```javascript
getDate()
```

Copy

### Pulling Day from the Date object

Returns the day from the date object

```javascript
getDay()
```

Copy

### Pulling Hours from the Date object

Returns the hours from the date object

```javascript
getHours()
```

Copy

### Pulling Minutes from the Date object

Returns the minutes from the date object

```javascript
getMinutes()
```

Copy

### Pulling Seconds from the Date object

Returns the seconds from the date object

```javascript
getSeconds()
```

Copy

### Pulling Time from the Date object

Returns the time from the date object

```javascript
getTime()
```

Copy

## Mouse Events

Any change in the state of an object is referred to as an Event. With the help of JS, you can handle events, i.e., how any specific HTML tag will work when the user does something.

### click

Fired when an element is clicked

```javascript
element.addEventListener('click', () => {
    // Code to be executed when the event is fired
});
```

Copy

### oncontextmenu

Fired when an element is right-clicked

```javascript
element.addEventListener('contextmenu', () => {
    // Code to be executed when the event is fired
});
```

Copy

### dblclick

Fired when an element is double-clicked

```javascript
element.addEventListener('dblclick', () => {
    // Code to be executed when the event is fired
});
```

Copy

### mouseenter

Fired when an element is entered by the mouse arrow

```javascript
element.addEventListener('mouseenter', () => {
    // Code to be executed when the event is fired
});
```

Copy

### mouseleave

Fired when an element is exited by the mouse arrow

```javascript
element.addEventListener('mouseleave', () => {
    // Code to be executed when the event is fired
});
```

Copy

### mousemove

Fired when the mouse is moved inside the element

```javascript
element.addEventListener('mousemove', () => {
    // Code to be executed when the event is fired
});
```

Copy

## Keyboard Events

### keydown

Fired when the user is pressing a key on the keyboard

```javascript
element.addEventListener('keydown', () => {
    // Code to be executed when the event is fired
});
```

Copy

### keypress

Fired when the user presses the key on the keyboard

```javascript
element.addEventListener('keypress', () => {
    // Code to be executed when the event is fired
});
```

Copy

### keyup

Fired when the user releases a key on the keyboard

```javascript
element.addEventListener('keyup', () => {
    // Code to be executed when the event is fired
});
```

Copy

## Errors

Errors are thrown by the compiler or interpreter whenever they find any fault in the code, and it can be of any type like syntax error, run-time error, logical error, etc. JS provides some functions to handle the errors.

### try and catch

Try the code block and execute catch when err is thrown

```javascript
try {
    Block of code to try
    }
catch (err) {
    Block of code to handle errors
}
```

Copy

## Window Methods

Methods that are available from the window object

### alert method

Used to alert something on the screen

```javascript
alert()
```

Copy

### blur method

The blur() method removes focus from the current window.

```javascript
blur()
```

Copy

### setInterval

Keeps executing code at a certain interval

```javascript
setInterval(() => {
    // Code to be executed
}, 1000);
```

Copy

### setTimeout

Executes the code after a certain interval of time

```javascript
setTimeout(() => {
    // Code to be executed
}, 1000);
```

Copy

### close

The Window. close() method closes the current window

```javascript
window.close()
```

Copy

### confirm

The window.confirm() instructs the browser to display a dialog with an optional message, and to wait until the user either confirms or cancels

```javascript
window.confirm('Are you sure?')
```

Copy

### open

Opens a new window

```javascript
window.open("https://www.codewithharry.com");
```

Copy

### prompt

Prompts the user with a text and takes a value. Second parameter is the default value

```javascript
var name = prompt("What is your name?", "Harry");
```

Copy

### scrollBy

```javascript
window.scrollBy(100, 0); // Scroll 100px to the right
```

Copy

### scrollTo

Scrolls the document to the specified coordinates.

```javascript
window.scrollTo(500, 0); // Scroll to horizontal position 500
```

Copy

### clearInterval

Clears the setInterval. var is the value returned by setInterval call

```javascript
clearInterval(var)
```

Copy

### clearTimeout

Clears the setTimeout. var is the value returned by setTimeout call

```javascript
clearTimeout(var)
```

Copy

### stop

Stops the further resource loading

```javascript
stop()
```

Copy

## Query/Get Elements

The browser creates a DOM (Document Object Model) whenever a web page is loaded, and with the help of HTML DOM, one can access and modify all the elements of the HTML document.

### querySelector

Selector to select first matching element

```javascript
document.querySelector('css-selectors')
```

Copy

### querySelectorAll

A selector to select all matching elements

```javascript
document.querySelectorAll('css-selectors', ...)
```

Copy

### getElementsByTagName

Select elements by tag name

```javascript
document.getElementsByTagName('element-name')
```

Copy

### getElementsByClassName

Select elements by class name

```javascript
document.getElementsByClassName('class-name')
```

Copy

### Get Element by Id

Select an element by its id

```javascript
document.getElementById('id')
```

Copy

## Creating Elements

Create new elements in the DOM

### createElement

Create a new element

```javascript
document.createElement('div')
```

Copy

### createTextNode

Create a new text node

```javascript
document.createTextNode('some text here')
```