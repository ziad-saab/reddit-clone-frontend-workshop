# Enhancing our Reddit clone with semantic HTML5, CSS3 and some JavaScript/jQuery

We ended week four of the bootcamp with a basic working Reddit clone. In doing so, we learned many things about HTTP and the web, more specifically on the back-end side of things.

In the next two weeks, we will be concentrating more on the browser side -- so-called front-end development -- keeping in mind that both "sides" are somewhat connected.

This first front-end workshop is more open-ended than the ones we have previously worked on. As a trainee, you are starting to become more independent. You have worked hard to find more and more information by yourself, and it has paid off :)

Therefore, instead of concentrating on one particular subject, today we propose to start discovering the world of front-end development by looking at many different aspects at the same time. In the coming days, we will formalize some of this knowledge with the help of a few short lectures.

## Semantic HTML5 tags
In this section, we will enhance our Reddit clone with HTML5 semantic tags. To do this, we'll have to rewrite some of our rendering functions to take advantage of these new elements. Here are some resources to get you started:

### Resources
* [Let's talk about semantics](http://html5doctor.com/lets-talk-about-semantics/)
* [MDN's HTML elements reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element#Content_sectioning)
* [Why to use semantic elements instead of DIV?](http://stackoverflow.com/a/17272444/1420728)
* [How to use HTML5 sectioning elements?](http://blog.teamtreehouse.com/use-html5-sectioning-elements)

### Work
Using your newly acquired knowledge, modify the HTML structure of your Reddit clone to take advantage of semantic HTML5 elements. Among other things, you can make use of `<nav>`, `<article>`, `<main>` and `<aside>`.

**Before doing so**, you would be better served reading and implementing the ReactJS server-side rendering in your Reddit clone by looking at [this repo](https://github.com/ziad-saab/serverside-jsx-demo) and/or asking us for help in the matter.

### Pre-work: using JSX with React to render our pages
JSX can help us render pretty much everything, except that pesky `<!doctype html>` string. Moreover, a component function that uses JSX will not actually return an HTML *string*, but more something like an *HTML structure*. Transforming it to a string means you have to call one more of React's functions to transform the HTML structure into a string.

Here's a function that will do that with any structure you pass it, and add the `<!doctype html>` to it:

```javascript
var ReactDOMServer = require('react-dom/server');
function renderHtml(jsxStructure) {
  var outputHtml = ReactDOMServer.renderToStaticMarkup(jsxStructure);

  return '<!doctype html>' + outputHtml;
}
```

One more thing: NodeJS does not understand JSX. This means that if we put JSX code in our `server.js`, we will get back a Syntax Error.

To help NodeJS understand JSX, we have to make use of the [Babel transpiler](http://babeljs.io). Babel transforms newer JavaScript code (ES6, JSX) into an equivalent ES5 that Node and our browsers can understand.

Based on the current setup of your `server.js`, here is the easiest way to get started with JSX:

* Install the following NPM packages and make sure to `--save`:
  * `babel-register`: This package will hook into Node's `require` function, and enable loading files with JSX syntax
  * `babel-preset-react`: Babel does nothing by default without presets. This tells Babel to transform JSX
  * `babel-preset-es2015`: While this is not necessary, it will enable us to use ES6 features in our code, even the ones NodeJS may not support
  * `react`: Will help us write our view components
  * `react-dom`: Will help us transform our view structures into strings of HTML

* Create a file called `.babelrc` at the root of your project, with the following in it:

  ```json
{
    "presets": ["react", "es2015"]
}
```

This file tells Babel what transformations to do on your code, because by default it will not do anything :)

* In your `server.js`, add the following line on top:

  ```javascript
require('babel-register');
```

You don't need to assign this to a variable. Requiring this module will hook to the `require` function, and enable us to require files with JSX!

* Create a file called `rendering.jsx`. This will contain all our JSX components. Later on we can modularize this file even more :)
* In `rendering.jsx`, have the following basic code:

  ```javascript
var React = require('react');
```

* Add the code of the `renderHtml` function from above.

Once that is setup, you can start with your first component! Let's look at something like the login form for example...

Here's what your login form code could look like right now:

```javascript
app.get('/login', function(req, res) {
  var maybeError = request.query.error;

  var html = "<form action='/login' method='post'>";
  if (maybeError) {
    html += "<div>" + maybeError + "</div>";
  }
  html += "<input type='text' name='username'>";
  //...
  // and then finally
  res.send(html);
})
```

1. Take only the part that returns the HTML, and put it in a function that receives a data object.
2. Put that function in `rendering.jsx` and add it to the module.exports
3. Make it render a full page starting at the `<html>` tag
4. Use the `renderHtml` function to make it output a string:

```javascript
function renderLogin(data) {
  // create the HTML structure with interpolations
  var structure = (
    <html>
      <head>
        <title>Login!</title>
      </head>
      <body>
        <h1>Login</h1>
        <form action="/login" method="post">
          {data.error && <div>{data.error}</div>}
          <input type="text" name="username"/>
          .....
        </form>
      </body>
    </html>
  );

  // return the html
  var html = renderHtml(structure);

  return html;
}
```

As a first step, try to do the same thing for every page that outputs HTML:

* login form
* signup form
* content create form
* homepage

Once this is done, perhaps we can extract the layout element?

```javascript
function Layout(data) {
  return (
    <html>
      <head>
        <title>{data.title}</title>
      </head>
      <body>
      {data.children}
      </body>
    </html>
  );
}
```

This component is the same across all pages, except a different TITLE and CONTENT. Here's how to refactor the login function above to use this component:

```javascript
function renderLogin(data) {
  // create the HTML structure with interpolations
  var structure = (
    <Layout title="Login!">
      <h1>Login</h1>
      <form action="/login" method="post">
        {data.error && <div>{data.error}</div>}
        <input type="text" name="username"/>
        .....
      </form>
    </Layout>
  );

  // return the html
  var html = renderHtml(structure);

  return html;
}
```

See if you can refactor every page to use the layout. When that is done, go back to the beginning of the exercise and try to output your structure using HTML semantic tags!

Use the `Layout` component to add a **navigation menu** to all your pages, using the `<nav>` component. As this is an open-ended exercise, see in what other places you can use HTML semantic elements like article, main, aside or section.

---

## CSS3, Flexbox
Learn more about CSS3 and flexbox using the following resources, and any other you can find:

* http://flexbox.io/#/
* http://flexboxin5.com/
* http://flexboxfroggy.com/

Using your newly-acquired knowledge on flexbox, as well as anything else you found out about CSS, refactor the homepage's list of posts to look like the following:



## jQuery
https://learn.jquery.com/ajax/
