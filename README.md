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

### Pre-work
Before starting to add or modify the HTML of your pages, you should make sure to have a similar layout for all your pages. Let's say that you generated HTML for the homepage which looks like this:

```html
<h1>home page!</h1>
<ul>
  <li>first post</li>
  <li>second post</li>
  <li>...</li>
</ul>
```

Perhaps you want all your pages to look like this:

```html
<!doctype html>
<html>
  <head>
    <title>Custom title for that page</title>
    <link rel="stylesheet" href="css/app.css"/>
  </head>
  <body>
    <header>
      <nav>same or different nav when logged in / logged out???</nav>
      <h1>Welcome to reddit!</h1>
    </header>
    
    <main>
      <!-- THIS PART SHOULD BE DIFFERENT FOR EACH PAGE -->
    </main>
    
    <footer>&copy; 2016 myself!</footer>
  </body>
</html>
```

First, [make sure you started using EJS](https://scotch.io/tutorials/use-ejs-to-template-your-node-application) by following this guide or many others.

Once you have EJS up and running, check out [`ejs-mate`](https://github.com/JacksonTian/ejs-mate) to add a common layout to your site.

### Work
Using your newly acquired knowledge, as well as your `renderLayout` function, modify the HTML structure of your Reddit clone to take advantage of semantic HTML5 elements. Among other things, you can make use of `<nav>`, `<article>`, `<main>` and `<aside>`.

At the very least, your page should have a constant header and footer, and a main section with all the content of that page.

---

## CSS3, Flexbox and styling forms
Learn more about CSS3 and flexbox using the following resources, and any other you can find:

### Flexbox: layouts the easy way
* http://flexbox.io/#/ Will ask you to register. **DO IT!!**
* http://flexboxin5.com/
* http://flexboxfroggy.com/

Using your newly-acquired knowledge on flexbox, as well as anything else you found out about CSS, refactor the homepage's list of posts to look like the following:

![](https://i.imgur.com/erVHYGq.png)

**STOP**: How are you going to load a CSS file from your HTML page? If you add a `<link>` tag with `href="/css/app.css"`, will it work? Does your web server have a handler for GET requests to `/css/app.css`? It doesn't, but it doesn't need to! [ExpressJS has a middleware for serving so-called "static files"](http://expressjs.com/en/starter/static-files.html). Read up on it and implement it in your server before going forward.

Once it's setup, create a static file at `css/app.css` in your `static` directory to complete this exercise.

Each post item in the list should be organized as a flexbox container.

The left side should take only as much space as it needs to output the up/down vote arrows and the vote score. **If you don't have vote scores in your app, replace it with a random number for the moment**.

The right side should expand to take all remaining space. Inside should be the title, and below that, yet another container.

The bottom of the right side container should have the "created by" on the left, and "created at" all the way to the right.

**HINT**: Remember, the goal here is to use as much of flexbox as possible. You shouldn't need to float things to the left or right, or any vertical alignment.

### Styling forms
Our Reddit clone, however tiny it may be already contains three FORMS! After reading [this MDN article about styling forms](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Forms/Styling_HTML_forms), use some CSS to make your forms look nice.

Try to implement an overall look for your forms and use some CSS classes to style them as you wish.

### More...
You can come back to this section and keep adding CSS to your page as we discover more together in class.

## Adding interactivity with JavaScript and jQuery
At the moment our Reddit clone is a little bit less user-friendly than it could be. For one thing, voting on content is refreshing the page every time. Another thing we may want to do is suggest a title for a user as they're adding a new URL, to make it easier.

As you know, any interactivity happening in your browser will be mostly due to JavaScript. So far, we've been writing JavaScript for quite a few weeks, but never actually used it in the browser. It turns out that the browser offers us a lot of APIs for doing interactive things **once a webpage has already loaded**. Some of these things include:

* Manipulating the content of the current document using [the DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
* [Listening for various events like clicking, hovering, typing](https://developer.mozilla.org/en-US/docs/Web/Events)
* [Making HTTP requests directly from the browser](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)

If you read the above documents, you may get a feeling that these APIs provided by the browser are quite low-level. They're not super straight forward and some pitfalls exist.

You may have heard of [jQuery](http://jquery.com/) as the go-to library for adding interactivity on your page. jQuery is no more than JavaScript code that packages some nice functions for doing the things we mentioned above. As it turns out, some of these things are much easier to do with jQuery, which explains why it has become so popular.

Here are the jQuery references for the above activities:

* [Manipulating the contents of a page with jQuery](https://learn.jquery.com/using-jquery-core/)
* [Listening for events like clicking, typing, etc. on a page](https://learn.jquery.com/events/)
* [Making HTTP requests directly from the browser with jQuery](https://learn.jquery.com/ajax/)
* [Try jQuery!](http://try.jquery.com/)

Based on the above references as well as your own research, try to complete one or two of the following activities:

### Suggesting a title before someone adds new content (browser-side only)
Here we are basically going to reproduce the following functionality:

![](http://g.recordit.co/x9OS40EY9n.gif)

1. Next to the URL field, let's add a button that says "suggest title".
2. **On click** of this button, we want to make an **ajax request** for the URL
3. When we receive the HTML for the page, we want to parse it again using jQuery
4. Using jQuery's DOM functions, find the `title` element and put its content in the title field
5. Optionally show a "loading" text while your are doing the work

**NOTE**: The user stays on the same page while this is going on. Everything happens without refreshing the page.

**NOTE 2**: What happens when you try to make an AJAX request to an external site? It seems like the browser is blocking you. How could you go around this?

You are probably getting an error that looks like this:

`XMLHttpRequest cannot load http://www.domain.com/some-url/ Origin null is not allowed by Access-Control-Allow-Origin.`

Here, you are being blocked by the [same origin policy](https://en.wikipedia.org/wiki/Same-origin_policy). Your browser is preventing the JavaScript on your domain to make AJAX calls to another domain because that could be destructive if not controlled.

For example, imagine if you land on my website, and some JavaScript code makes an AJAX request to your bank's website. If that were let through, my JavaScript could retrieve your bank account numbers and balances and send them over to me over the network.

Since we won't be able to do external request to every website in the world from our own domain, the best way around that will be to make the requests **from our own web server**. Here's how it will work:

Let's say you want to find out the title of the page at https://www.decodemtl.com/web-development-full-time/. If you make an AJAX request to this URL your browser will block you. Instead, make an AJAX call to your own web server, at the resource called, say, `/suggestTitle` and pass it the URL in a query string: `/suggestTitle?url=https://www.decodemtl.com/web-development-full-time/`.

On your server, implement an `app.get('/suggestTitle')` that will use the `request.query.url`, make a request to that web page, and retrieve the HTML code of the page.

Then, you'll need to parse the HTML on the server side to find the content of the `<title>` tag. Wouldn't it be nice if you had jQuery on the server? Well, you do! The [cheerio NPM package](https://cheeriojs.github.io/cheerio/) does specifically that :)

So the process will work like this:

1. User enters a URL in the title field
2. User clicks on the "suggest title" button
3. Your code does an AJAX query to `/suggestTitle?url=...`
4. Your server receives this request, and retrieves the `url` from the query string parameters
5. Your server does a web request (e.g. using the `request` NPM package) to the submitted URL
6. Your server receives the HTML for the web page and passes it to the `cheerio` library
7. `cheerio` returns the `<title>` of the page
8. Your server sends a `response.json` with the title, like `{"title": "web development in montreal"}`
9. Your browser code receives the response from the server
10. Your browser code updates the Title field with the text of the response

### Voting without refreshing the page!
When a user clicks on the submit button of either of the two voting forms, the following happens:

1. The browser takes the form data and makes a query string out of it: `voteDirection=1&contentId=123`
2. The browser looks at the form's `action` and `method` and makes an HTTP request accordingly
3. The server receives the (POST) request and form data, and does what it needs to do
4. The server sends a **redirect response** back to the homepage
5. The browser receives the redirect, and does another HTTP GET to the homepage

While this experience follows the **statelessness** principle of HTTP, it's not super friendly for our users. For one thing, there is a flash of content as the page refreshes. Moreover, the server is indiscriminately redirecting to the homepage, but we may have been on another page that displays posts. Not only that, but we may have been scrolled to the bottom of the page and now we are back to the top.

For all these reasons, it may be nicer for our users if we could take such a small interaction as voting and make it happen **without refreshing the page**. Even though we're staying on the same page, we still have to follow the rules of HTTP. Among other things, it means that *our browser will still have to send an HTTP POST request to make the vote happen*. The difference is, this request will be done by the JavaScript code running in the browser. This is AJAX :)

Here are the high level steps we will be following...

When a user submits either of the two voting forms:

1. Our code should **listen to this submit event** and prevent the submit from taking place
2. Our code should **find the two hidden inputs** inside the form, and [find out their value](https://api.jquery.com/val/)
3. Our code should **do an ajax POST request** to our `/vote` URL, passing it the parameters `voteDirection` and `contentId`
4. At this point, our server will receive the POST and do its usual business of creating the vote
5. For now, our server is sending a redirect response, which our AJAX code may not have much use for...

In the server-side version, our POST handler was redirecting us back to the homepage, the browser was requesting the home page again, and the new votes count would display itself. In this browser-only version, what could we do? We made the POST request, but the vote count stays the same... How could we fix this?

If only we could get the new vote count as the result of our POST request... Well why not? We're the ones who wrote the web server, so we can make it do whatever we want! Let's modify our Reddit server's POST handler to `/vote`. Instead of sending a redirect response to the client, let's find out the new vote score for the content that was voted on, and return it to the user as JSON. Instead of:

1. Check if the user is logged in
2. Create a vote or update existing vote
3. Send a `response.redirect` response to `/` homepage

Let's do the following:

1. Check if the user is logged in (this stays the same)
2. Create a vote or update existing vote (this stays the same)
3. Make a new Sequelize query for the Content with its vote score
4. Send a `response.json` with an object `{newVoteScore: XX}` where `XX` is the new vote score

Then, on the browser side, in the response to our AJAX query, we will receive this JSON. Let's do the following:

1. Receive the JSON response from doing the POST to `/vote` by AJAX
2. Parse the JSON response to an object using `JSON.parse`
3. Retrieve the `newVoteScore` property of the object
4. Update the user interface to represent the new vote score using jQuery's DOM functions
