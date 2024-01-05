
## Boilerplate

```markup
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <!-- Body -->
</body>
</html>
```

Copy

## Headings

There are six headings available in HTML, <h1> is the largest among all, and <h6> is the smallest.

### <h1> Tag

```html
<h1>Heading 1</h1>
```

Copy

### <h2> Tag

```html
<h2>Heading 2</h2>
```

Copy

### <h3> Tag

```html
<h3>Heading 3</h3>
```

Copy

### <h4> Tag

```html
<h4>Heading 4</h4>
```

Copy

### <h5> Tag

```html
<h5>Heading 5</h5>
```

Copy

### <h6> Tag

```html
<h6>Heading 6</h6>
```

Copy

## Container

Container tags are the tags that contain some data such as text, image, etc. There are several container tags in HTML.

### div tag

The div tag or division tag is used to make blocks or divisions in the document.

```html
<div> This is div block </div>
```

Copy

### span tag

The span is a container for inline content

```html
<span> This is span block </span>
```

Copy

### p tag

The p tag is used to create a paragraph in HTML

```html
<p> This is a paragraph </p>
```

Copy

### pre tag

The pre tag represents pre-formatted text

```html
<pre> Hello World </pre>
```

Copy

### code tag

The code tag is used to represent source codes in HTML

```markup
<code>
import python
</code>
```

Copy

## Text Formatting

Text formatting tags are used to format text or data in HTML documents. You can do certain things like creating italic, bold, and strong text to make your document look more attractive and understandable.

### <b> tag

```html
<b>I'm bold text</b>
```

Copy

### <strong> tag

```html
<strong>I'm important text</strong>
```

Copy

### <i> tag

```html
<i>I'm italic text</i>
```

Copy

### <em> tag

```html
<em>Emphasized text</em>
```

Copy

### <sub> tag

```html
<sub>Subscript</sub>
```

Copy

### <sup> tag

```html
<sup>Superscript</sup>
```

Copy

## Lists

Lists can be either numerical, alphabetic, bullet, or other symbols. You can specify list type and list items in HTML for a clean document.

### <ol> tag

The ordered list starts with <ol> tag and each list item starts with an <li> tag

```markup
<ol>
    <li>Data 1</li>
    <li>Data 2</li>
    <li>Data 3</li>
</ol>
```

Copy

### <ul> tag

```markup
<ul>
    <li>Your Data</li>
    <li>Your Data</li>
</ul>
```

Copy

## Media

Media is anything that is present in digital form such as image, video, audio, etc.

### <audio> tag

It is used to embed sound content in the document.

```markup
<audio controls>
    <source src="demo.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
</audio>
```

Copy

### <img> tag

It is used to embed or import images in a webpage.

```html
<img src="Source_of_image" alt="Alternate text">
```

Copy

### <video> tag

It is used to embed videos on a webpage.

```markup
<video width="480" height="320" controls>
    <source src="demo_move.mp4" type="video/mp4">
    Your browser does not support the video tag.
</video>
```

Copy

## Table

A table is a collection of rows and columns. It is used to represent data in tabular form.

### Table Structure

```markup
<table>
    <caption>Demo Table</caption>
    <thead>
        <tr>
            <th>Column1</th>
            <th colspan="2">Column2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Data1</td>
            <td>Data2</td>
            <td>Data2</td>
        </tr>
        <tr>
            <td>Data1</td>
            <td>Data2</td>
            <td>Data2</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>&nbsp;</td>
            <td>Data</td>
            <td>Data</td>
        </tr>
    </tfoot>
</table>
```

Copy

## Links

Links are clickable text that can redirect you to some other page.

### <a> tag

<a> or anchor tag defines a hyperlink. When clicked, it takes you to some other page

```html
<a href="https://www.codewithharry.com/">Visit CodeWithHarry.com!</a>
```

Copy

## Form

The form is used to collect the user's input, generally, the user's data is sent to the server for further processing.

```markup
<form action="/action.php" method="post"> 
    <textarea cols="20" name="comments" rows="5">Comment</textarea><br />
    <label><input name="terms" type="checkbox" value="tandc" />Accept terms</label> <br />
    <input type="submit" value="Submit" />
</form>
```

Copy

## Form Elements

We use various input types and buttons inside a form as form elements

### Text Input

```html
<input type="text" name="username" placeholder="Enter Username">
```

Copy

### Password Input

```html
<input type="password" name="password" placeholder="Enter Password">
```

Copy

### Checkbox

```html
<input type="checkbox" name="agree" value="yes"> I agree
```

Copy

### Radio Button

```html
<input type="radio" name="gender" value="male"> Male<br><input type="radio" name="gender" value="female"> Female
```

Copy

### Submit Button

```html
<input type="submit" value="Submit">
```

Copy

### Button

```html
<button type="button">Click Me</button>
```

Copy

### Select (Dropdown) List

```html
<select name="country"><option value="usa">United States</option><option value="canada">Canada</option></select>
```

Copy

### Textarea

```html
<textarea name="comments" rows="4" cols="50">Enter comments here</textarea>
```

Copy

### File Input

```html
<input type="file" name="fileupload">
```

Copy

### Range Input

```html
<input type="range" name="volume" min="0" max="100">
```

Copy

### Number Input

```html
<input type="number" name="quantity" min="1" max="10">
```

Copy

### Email Input

```html
<input type="email" name="email" placeholder="Enter Email">
```

Copy

### Search Input

```html
<input type="search" name="query" placeholder="Search">
```

Copy

### URL Input

```html
<input type="url" name="website" placeholder="Enter URL">
```

Copy

### Date Input

```html
<input type="date" name="birthdate">
```

Copy

## Characters & Symbols

Some symbols are not directly present on the keyboard, but there are some ways to use them in HTML documents. We can display them either by entity name, decimal, or hexadecimal value.

### Copyright Symbol (©)

```html
&copy;
```

Copy

### Less than (<)

```markup
&lt;
```

Copy

### Greater than (>)

```html
&gt;
```

Copy

### Ampersand (&)

```html
&amp;
```

Copy

### Dollar ($)

```html
&dollar;
```

Copy

## Random Text

### Elon Musk

```html
Elon Reeve Musk FRS is an entrepreneur and business magnate. He is the founder, CEO, and Chief Engineer at SpaceX; early stage investor, CEO, and Product Architect of Tesla, Inc.; founder of The Boring Company; and co-founder of Neuralink and OpenAI. A centibillionaire, Musk is one of the richest people in the world.
```

Copy

## Semantic Elements

Semantic elements are those that convey their meaning and purpose clearly through their name alone.

### <section> tag

It defines a section in the document

```css
<section>This is a section</section>
```

Copy

### <article> tag

It represents self-contained content

```css
<article> Enter your data here </article>
```

Copy

### <aside> tag

It is used to place content in the sidebar

```css
<aside> Your data </aside>
```

Copy

### Meta Tags

Meta tags define metadata about the document, such as author, description, and keywords.

```markup
<meta name="description" content="This is a description of the page">
<meta name="keywords" content="HTML, CSS, JavaScript">
<meta name="author" content="Author Name">
```

Copy

### CSS Integration

CSS integration can be done to style our HTML document using internal or external CSS.

```markup
<style>
  body { background-color: lightblue; }
</style>
<link rel="stylesheet" type="text/css" href="styles.css">
```

Copy

### Accessibility

Make your webpage accessible to all users with these best practices.

```markup
<img src="image.jpg" alt="Description of Image">
<label for="name">Name:</label> <input type="text" id="name" name="name">
```

Copy

### Responsive Design

Design your webpage to adapt to different screen sizes using CSS media queries.

```markup
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
  @media (max-width: 600px) {
    body { font-size: 18px; }
  }
</style>
```

Copy

### JavaScript Integration

Embed JavaScript directly or link to an external file for added functionality.

```markup
<script>
  alert('Hello, World!');
</script>
<script src="script.js"></script>
```

Copy

### Comments

Comments allow you to leave notes in your code, which are ignored by browsers.

```html
<!-- This is a comment -->
```

Copy

### Other tips for HTML Beginners

#### 1. Use a Modern Text Editor

Tools like [Visual Studio Code (VS Code)](https://code.visualstudio.com/ "vs code homepage") provide syntax highlighting, autocompletion, and other helpful features. VS Code also supports Emmet for faster HTML and CSS coding.

#### 2. Utilize Emmet

Emmet is a plugin available in many text editors that allows you to write HTML and CSS more quickly. For example, typing `ul>li*5` and then pressing 'Tab' will create a `<ul>` element with five `<li>` children as shown below:

```markup
<ul>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
</ul>
```

Copy

#### 3. Indent Your Code

Properly indenting your code makes it more readable. Most modern text editors offer automatic indentation.

#### 4. Always Close Tags

Make sure to close all of your HTML tags. This helps to avoid unexpected rendering issues.

#### 5. Learn CSS and JavaScript

Understanding CSS and JavaScript can help you create more dynamic and visually appealing websites. Consider studying them alongside HTML.

#### 6. Use Comments Wisely

Comments can be helpful for leaving notes and reminders in your code, or for temporarily disabling a section of HTML.

#### 7. Practice Regularly

Like any skill, practice makes perfect. Try building different types of web pages to improve your understanding and proficiency in HTML.

#### 8. Learn about Responsive Design

Responsive design ensures that your website looks good on all devices. Learning about media queries and flexible grid layouts is essential for modern web development.