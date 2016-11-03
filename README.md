# Enhancing Reddit Clone with semantic HTML5, responsive CSS and jQuery

We ended week four of the bootcamp with a basic working Reddit Clone. In doing so, we mainly concentrated on the back-end aspect: the database and the web server calls. This workshop will take your Reddit Clone to the next level. Here, we will concentrate on the front-end: semantic HTML tags, responsive CSS and some interactive features with jQuery.

In a first part, you will be updating the HTML of your Reddit Clone. It's important to have well-structured HTML: screen readers, web crawlers like Google and any other computer program looking at your site will make better sense of it if you use proper headings, sectioning elements and meta tags.

## Semantic HTML tags
In this section, we will enhance our Reddit Clone with HTML5 semantic tags. To do this, we'll have to rewrite some of our rendering functions to take advantage of these new elements. We already talked about semantic HTML at the beginning of the week, and here are more resources to help you understand what it's all about:

* [Let's talk about semantics](http://html5doctor.com/lets-talk-about-semantics/)
* [MDN's HTML elements reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element#Content_sectioning)
* [Why to use semantic elements instead of DIV?](http://stackoverflow.com/a/17272444/1420728)
* [How to use HTML5 sectioning elements?](http://blog.teamtreehouse.com/use-html5-sectioning-elements)

### Step 1: template engine
Make sure that you are already using the Pug template engine and a `layout.pug` file, so that all your pages have the same base HTML structure. You should already have this up and running from last week. If you don't, please go back and add Pug to your Reddit Clone, and make sure that you are using a layout.

### Step 2: semantic HTML
Going through all the views you created, make sure there are as little `<div>`s as possible. As a general rule of thumb, `<div>` should only be used to group or wrap content for the purpose of styling it.

### Step 3: meta information
Having a common layout for all the pages of your site does not mean that the `<title>` and `<meta>` tags should have the same content for all pages. Your `layout.pug` file can make use of template variables and locals the same way as any other template.

For this step, go through all the `GET` endpoints of your web server that are outputting HTML. This is the homepage, the subreddits page, the single post page (if you have one) and the forms pages: signup, login and create post.

For each page, make sure that it gets a unique `<title>`. You can do this by having a default title for your whole app, using ExpressJS locals. You can do it like this:

```js
var app = express();
//....
app.locals.title = "default title";
```

Then later on when you render a template, let's say the login template, you can do this:

```js
response.render('login', {title: "Login to Reddit Clone"})
```

When rendering your template, Express will merge the template data with its own locals, with the template data taking priority. This way, you always have a fallback for the title. If you have a single-post page, the title could be the title of the post for example.

Finally, if you have a single-post page, also make sure that you have a `<meta name="description" content="XXXX">` in your output. The description could be the same as the title of that post.

---

## Adding CSS
In this section, we'll make our Reddit Clone look like a real site! We'll even do "better" than Reddit and have a responsive site under a single URL.

We already looked at basic CSS selectors and properties in class, and here are some more resources to help you.
* http://flexbox.io/#/ Will ask you to register. **DO IT!!**
* http://flexboxin5.com/
* http://flexboxfroggy.com/
* [CSS positioning explained](https://medium.freecodecamp.com/css-positioning-explained-by-building-an-ice-cream-sundae-831cb884bfa9#.hsl38uncf)
* [30 CSS selectors you must memorize](https://code.tutsplus.com/tutorials/the-30-css-selectors-you-must-memorize--net-16048)


### Step 1: Make it responsive
Using your responsive grid from the responsive CSS workshop, create a layout for your Reddit Clone. You can take inspiration from the current Reddit, as well as Mobile Reddit at [m.reddit.com](https://m.reddit.com).

### Step 2: Style the forms
Let's face it, the base style of form elements in browsers leaves much to be desired. Our Reddit clone, however tiny it may be already contains at least three forms! After reading [this MDN article about styling forms](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Forms/Styling_HTML_forms) to help you out, do it!

Create some CSS classes that you can apply to all your form elements. Text and password inputs should have some padding to them, as well as a flat border. Buttons should have the same padding as your inputs so that they look aligned.

### Step 3: Make it unique
Take some time to discover what can be done with CSS. One of our favourite resources is [CSS Zen Garden](http://csszengarden.com/).

---

## Adding interactivity with JavaScript and jQuery
At the moment our Reddit clone is a little bit less user-friendly than it could be. For one thing, voting on content is refreshing the page every time. Another thing we may want to do is suggest a title for a user as they're adding a new URL, to make it easier.

As you know, any interactivity happening in your browser will be mostly due to JavaScript. So far, we've been writing JavaScript for quite a few weeks, but only recently have we started using it in the browser. It turns out that the browser offers us a lot of APIs for doing interactive things **once a webpage has already loaded**. Some of these things include:

* Manipulating the content of the current document using [the DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
* [Listening for various events like clicking, hovering, typing](https://developer.mozilla.org/en-US/docs/Web/Events)
* [Making HTTP requests directly from the browser](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)

If you read the above documents, you may get a feeling that these APIs provided by the browser are quite low-level. They're not super straight forward and some pitfalls exist.

We already started looking at jQuery, and here are some more references for you:

* [Manipulating the contents of a page with jQuery](https://learn.jquery.com/using-jquery-core/)
* [Listening for events like clicking, typing, etc. on a page](https://learn.jquery.com/events/)
* [Making HTTP requests directly from the browser with jQuery](https://learn.jquery.com/ajax/)
* [Try jQuery!](http://try.jquery.com/)

The following exercises are completely independent. They can be done in any order you like. Make sure to do each one in a branch so that you can go back to a clean state if you haven't finished one of them.

### Suggesting a title before someone adds new content
Here we are going to reproduce the following functionality:

![](http://g.recordit.co/x9OS40EY9n.gif)

1. Next to the URL field, let's add a button that says "suggest title".
2. **On click** of this button, we want to make an **ajax request** for the URL that the user entered, to retrieve the HTML content for that page. For example, if the user entered `http://www.decodemtl.com` then we need to get the HTML page for DecodeMTL, so that we can extract the title from it.
3. When we receive the HTML for the page, let's use a basic string manipulation to find the title: start by splitting the HTML string using `<title>` as a separator. This will give you two parts. Take the second part, and replace `</title>` with an empty string. There, you now have the title of the target page inside your AJAX callback.
4. Use jQuery's `.val(newValue)` function to change the value of the title field to be what you found
5. Find a way to show a "loading" indicator while the AJAX call is going. What you want to do is `.show()` a `<span>` that contains the text "Loading..." right before you fire off your AJAX call, and `.hide()` it in your AJAX response callback.

**NOTE**: The user stays on the same page while this is going on. Everything happens without refreshing the page.

What happens when you try to make an AJAX request to an external site? It seems like the browser is blocking you. How could you go around this?

You are probably getting an error that looks like this:

`XMLHttpRequest cannot load http://www.domain.com/some-url/ Origin is not allowed by Access-Control-Allow-Origin.`

Here, you are being blocked by the [same origin policy](https://en.wikipedia.org/wiki/Same-origin_policy). Your browser is preventing the JavaScript on your domain to see the response of AJAX calls to another domain, because that could be a big security risk. Here's an example of a security issue that could arise from this.

Let's say that I own the website evil.com and I get you to load my site. Once you load it, I have a `<script>` tag that runs on my page. The script does an AJAX call to `https://www.yourbankdomain.com/accounts`. Since it's *your* browser doing this call and you are logged in to your bank, the response to this HTTP request will contain all your back accounts with all their balances, probably as an HTML document. Once I receive this information, I make another AJAX call to *my* server, and I send myself your banking information. Anyone who lands on my evil.com site can have their information stolen. I could also make an AJAX call to `https://www.facebook.com/messages` and retrieve all your personal messages. Remember, an AJAX call is just an HTTP request done by your browser.

For these reasons and many more, browsers will prevent you from reading the content of AJAX requests made to other domains by default. [The server at the other end can decide to allow such cross-origin requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS) in case where it's not holding any sensitive information. But many sites do not do that.

Since we won't be able to do external request to every website in the world from our own domain, the best way around that will be to make the requests **from our own web server**, by creating a simple proxy. Here's how it will work:

Let's say you want to find out the title of the page at https://www.decodemtl.com/web-development-full-time/. If you make an AJAX request to this URL your browser will block you from seeing the response. Instead, make an AJAX call to your own web server at the resource called, say, `/suggestTitle` and pass it the URL in a query string: `/suggestTitle?url=https://www.decodemtl.com/web-development-full-time/`.

On your Reddit web server, implement an `app.get('/suggestTitle')` that will use the `request.query.url`, make a request to that web page using the NPM `request` module, and retrieve the HTML code of the page.

Then, you'll need to parse the HTML on the server side to find the content of the `<title>` tag. You already have that code from step 3 above, and since we're using JavaScript on the server too, you can re-use it!

So the process will work like this:

1. User enters a URL in the title field
2. User clicks on the "suggest title" button
3. Your code does an AJAX query to your server at `/suggestTitle?url=...`
4. Your reddit server receives this request, and retrieves the `url` from the query string parameters
5. Your server does a web request using the `request` NPM package to the submitted URL
6. Your server receives the HTML for the web page and parses the `<title>`
8. Your server sends a `response.json` with the title, like `{"title": "web development in montreal"}`
9. Your browser AJAX callback receives the response from the server
10. Your browser code updates the Title field with the text of the response

The reason why it's not a security risk for your server to make the call is because it's not authenticated with cookies. If your server does a call to `https://www.facebook.com/messages`, then it won't receive any interesting response. The security risk comes from allowing you to make such cross-domain requests in the browser only.

### Voting without refreshing the page
When a user clicks on the submit button of either of the two voting forms, the following happens:

1. The browser takes the form data and makes a query string out of it: `voteDirection=1&contentId=123`
2. The browser looks at the form's `action` and `method` and makes an HTTP request accordingly
3. The server receives the (POST) request and form data, and does what it needs to do
4. The server sends a **redirect response** back to the homepage
5. The browser receives the redirect, and does another HTTP GET to the homepage

While this experience follows the **statelessness** principle of HTTP, it's not super friendly for our users. For one thing, there is a flash of content as the page refreshes. Moreover, the server is indiscriminately redirecting to the homepage, but we may have been on another page that displays posts. Not only that, but we may have been scrolled to the bottom of the page and now we are back to the top.

For all these reasons, it would be nicer for our users if we could take such a small interaction as voting and make it happen **without refreshing the page**. Even though we're staying on the same page, we still have to follow the rules of HTTP. Among other things, it means that *our browser will still have to send an HTTP POST request to make the vote happen*. The difference is, this request will be done by the JavaScript code running in the browser. This is AJAX :)

Here are the high level steps we will be following...

1. Replace the two voting forms with two simple buttons, upvote and downvote
2. Add two [data attributes](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_data_attributes) to each button, one for the post id and one for the vote direction. For a given post, your buttons could look like this:

```html
<button class="vote-button" data-postid="123" data-direction="1">UPVOTE</button>
<button class="vote-button" data-postid="123" data-direction="-1">DOWNVOTE</button>
```
3. Listen for clicks on those buttons
4. When a click happens, in the event handler use jQuery's [`.data(key)`](https://api.jquery.com/data/#data2) function to find the vote direction and post id, and make an AJAX POST request to the `/vote` endpoint with that information
5. On the server-side, change the `/vote` endpoint to return a JSON with `{"success": true}` if the vote goes through, instead of the `response.redirect` that you are doing now.
6. On the browser side, if the AJAX request is successful, adjust the vote score as necessary. If moving from up to downvote, you need to remove 2 points. If moving from no vote to up, then +1, no vote to down then -1, from down to up then +2. To do this 100% correctly, you'll need to know if the user already voted on a post as the page loads. On reddit, they show you this by changing the color of the arrow from grey to orange or blue.

### Auto-suggest subreddit
In the create post form, the user has to choose a subreddit that they want to post to. You may have implemented that in a few different ways in your static HTML version: either radio buttons, or a `<select>` list, or even a simple text field.

For this exercise, we're going to use the [jQuery-Autocomplete](https://github.com/devbridge/jQuery-Autocomplete) library to suggest a subreddit to our users as they type. For example, if you have subreddits called "Montreal", "MononcSerge" and "MonasteryStays", they should all be suggested to the user if they start typing "M" or "Mo" or "Mon".... This is the same feature you see on Google when you start searching, or on reddit.com/submit if you are logged in.

First, [load the jQuery-Autocomplete library in your page from CDNJS](https://cdnjs.com/libraries/jquery.devbridge-autocomplete).

Then, implement a `GET /suggest` endpoint on your reddit server. It should do the following:

1. Retrieve the query string parameter called `query`. This is weird, it will be `request.query.query`. This is the default parameter that jQuery-Autocomplete passes to your server
2. Do a MySQL request on your subreddits table using a `SELECT * FROM subreddits WHERE name LIKE "Mon%"` as an example.
3. Retrieve all the subreddits that match
4. Send a JSON response in the [format suggested by jQuery-Autocomplete](https://github.com/devbridge/jQuery-Autocomplete#response-format). `value` is what will be displayed to the user, and `data` is what will be passed to the `onSelect` callback (see below). 

After that, you should simply have to call the `autocomplete` plugin on your subreddit text input field, and pass it the appropriate parameters. Look at the [usage section of the jQuery-Autocomplete documentation](https://github.com/devbridge/jQuery-Autocomplete#usage)

When the user selects a subreddit, you can set the subreddit ID as an [`<input type="hidden">`](http://www.echoecho.com/htmlforms07.htm) field inside the `<form>` from your `onSelect` callback. This way, when the form is submitted by the browser, it will have the subreddit ID in the POST data.
