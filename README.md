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

---

## Putting your Reddit Clone "online"
It's time to host your Reddit Clone online using a real production environment. Even though Cloud9 provides you a public URL for testing your site, it's not meant to be used by the public. For one thing, the Cloud9 virtual machine is super slow and won't be able to accept a large number of requests. But more importantly, Cloud9 will sleep your virtual machine if you don't touch it for a few days, which would bring your site offline.

In this section, we're going to publish your Reddit Clone on the [Heroku platform](https://www.heroku.com/). In short, Heroku is a service to which you can upload your NodeJS code. They will run your code on their servers just like Cloud9 does, but more automated. You can upload your code to Heroku by doing a simple `git push`.

Of course there's a bit more to it than that. For one thing, our Reddit Clone needs a MySQL database. So far, we got away with running a development database server on the same Cloud9 VM as our web server. In production, we'll need to use a production-ready MySQL database. Thankfully, Heroku has got us covered with not one but two MySQL database hosting providers, and they both provide a "free for development" option. Heroku itself is free for development, so you won't have to pay anything to get started :)

Here are the steps you'll need to follow:

1. Create a Heroku account. That's the easy part.
2. Login to Heroku and [create a new application](https://dashboard.heroku.com/new?org=personal-apps)
3. On your app's dashboard, click on the Resources link at the top
4. On the Resources page, scroll down to Addons and search for mysql
5. Choose the ClearDB MySQL and select the "Ignite - Free" option
6. Install the Heroku Command Line client. If you are on Cloud9, follow the [Debian/Ubuntu instructions](https://devcenter.heroku.com/articles/heroku-command-line#debian-ubuntu). On a Mac follow the [OSX instructions](https://devcenter.heroku.com/articles/heroku-command-line#os-x)
7. From your command line, run `heroku login` and follow the prompts. You only have to do this once.
8. From your command line, go in the directory of your project and make sure everything is commited. If you've been using branches, make sure any completed work has been merged to master. Make sure that you are on the master branch
9. Run `heroku git:remote -a NAME_OF_YOUR_HEROKU_APP`
10. Run `git push heroku master`

That's it! You've successfully pushed code on Heroku! Will it work? Not immediately. There's still a lot for us to do. However, we have everything setup to succeed. From now on, whenever we make changes to our code that we need to push on Heroku, we can simply run `git push heroku master`. This will push our code on Heroku, and Heroku will automatically install NPM packages then execute our server.

At this point there are still two major things to resolve. First, Heroku doesn't know what `.js` file to execute to run our server. Second, our code is connecting to a MySQL database at `localhost`. This will not work on Heroku.

### Telling Heroku what code to run
Let's say that you normally execute `node server.js` to run your web server. To tell this to Heroku, you have to create a file at the root of your project and call it `Procfile`, without any extension. In this file, write the following:

```
web: node server.js
```

If your command is different, make sure to change it. If you're running your code with `nodemon`, change it to `node` for Heroku. That's it for this step. If you push your changes to Heroku now, it will try to run your server. This will fail, because your server won't be able to connect to its MySQL database.

### Modifying the server code to use the right database
Remember our friend `process.env`? We can use it to access so-called environment variables. On Cloud9, we are provided with a `PORT` environment variable that we use to make our server listen on the port that Cloud9 wants us to listen on. Well, Heroku has the same `PORT` env var, so our `server.listen(process.env.PORT)` line works both on Cloud9 and Heroku.

It turns out that when you added the ClearDB database to your app, Heroku automatically added an environment variable called `CLEARDB_DATABASE_URL` to your Heroku environment. Of course, this environment variable is not present on Cloud9 nor on your local machine. You can see the value of this environment variable by running the following command from your project directory:

```
heroku config 
```

We will use this to our advantage to modify the `mysql.createConnection` call we are making in our server. It turns out that `createConnection` can accept both an object of options and a URL. Instead of doing:

```js
var conn = mysql.createConnection({
// ...options
});
```

let's do this:

```js
var conn;
if (process.env.CLEARDB_DATABASE_URL) {
  conn = mysql.createConnection(process.env.CLEARDB_DATABASE_URL);
}
else {
  conn = mysql.createConnection(/*same as before for local env*/);
}
```

With this simple code change, the same server code will work both on Cloud9 AND on Heroku. This is why environment variables are so powerful. They are set by the system, and can affect our running code at any level.

Great! Now our server can connect to the proper MySQL database, but there' still one issue: our ClearDB-hosted MySQL server does not have any tables in it! To fix this, we'll need to run some `CREATE TABLES` on that server. Thankfully we can do this by using the command-line MySQL client. But rather than calling it only with the `-u` option, we'll need to provide it two more options: a password and a database name. All this information is included in the `CLEARDB_DATABASE_URL` variable. This URL is of the form:

```
mysql://USERNAME:PASSWORD@HOST/DATABASENAME
```

Find out your connection information by retrieving the URL from your Heroku app, then run the following command line:

```
mysql -uUSERNAME -p -hHOST --database DATABASENAME
```

replacing each value by the one found in the URL. When you press ENTER, you'll be prompted for the password. This is to make sure that your password doesn't make it to the history of your command line, which could pose a security risk.

If successful, you'll be greeted with the MySQL prompt you are familiar with. Don't be mistaken however, you *are* connected to a remote server! If you do a `SHOW TABLES`, it should be empty.

Bring your database to a usable state by running any `CREATE TABLE`s and `ALTER TABLE`s that you need. At this point you should be able to run your Reddit Clone! Go to your `herokuapp.com` URL and see if you can signup.

Still, many things can go wrong. Most probably you'll be getting an "Application Error" page from Heroku. This means your server has crashed for one reason or another. If you do get that, then go to your command line and run the following command:

```
heroku logs --tail
```

This will output the `console.log`s that your server is doing on Heroku. You can stop this command with `Ctrl+C`, but don't stop it now. Read the logs, and see if you can figure out what's wrong. Here are a few things that could have gone wrong:

1. Your server is using an NPM module that you installed without `--save`. Since Heroku reads your `package.json` to install the modules, it won't have installed it. So your server will be working locally, but not on Heroku. Fix this by re-installing the package with `--save`, commit and push to Heroku. Then check the logs again to see what's up
2. You made a typo in `process.env.CLEARDB_DATABASE_URL`. Fix it!
3. You did not add all the proper tables/fields to the production database. If this happens, you'll see some MySQL errors in your logs. Fix them!
4. Anything. Getting your app to load and work correctly on a remote environment is not easy. Many things can go wrong. If you look at the logs carefully, you should be able to figure out what's happening, and fix it as if it was happening on your local machine. You can do it!

As a final step, test all the functionalities of your production Reddit Clone. Make sure you can signup, login, create posts, upvote, downvote, create multiple accounts, etc. Everything should work exactly as on your local. Congratulations, you now have an online Reddit Clone that you can show to your classmates, friends and potential employers :)
