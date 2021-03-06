# Creating an Application

The `Marionette.Application` base class is used as the root of your application.
The `Application` is commonly used to:

  1. Transfer any data from the surrounding page into your application.
  2. Initialize your regions and views.
  3. Start your [router][router].

This section explores the different use cases and how to combine them
effectively.


## Starting your Application

The most common application pattern is to construct your view hierarchy and
populate your initial data from your surrounding screen in the `onStart`
handler.

The application lives in your `driver.js` or `main.js` file and looks like:

```js
var Marionette = require('backbone.marionette');

var Layout = require('./views/layout');


var app = new Marionette.Application({
  onStart: function(options) {
    var layout = new Layout(options);
    layout.render();
  }
});

app.start();
```

This simple pattern will work for many types of application. It also works well
for index pages with data injected from a server-side template.


### Initial Server data

Sometimes we want to pre-load data from the server. We'd usually do this to
minimize server round-trips and load our data faster. Let's look at the
following `index.html` template that our server generated for us:

```html
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    <div id="body-hook"></div>
    <script>
      /** This section was generated by a server template engine e.g. Django
          or Ruby on Rails
      */
      window.initialData = [
        {
          id: 1,
          url: '/item/1',
          strapline: 'Make your applications dance',
          title: 'Marionette'
        },
        {
          id: 3,
          url: '/item/3',
          strapline: '',
          title: 'Backbone.js'
        }
      ];
      /** End server generated section */
    </script>
    <script src="/static/js/app.js"></script>
  </body>
</html>
```

Now we'll modify our `driver.js` file to look for the initial data from the
server:

```js
var Marionette = require('backbone.marionette');

var Layout = require('./views/layout');


var app = new Marionette.Application({
  onStart: function(options) {
    var layout = new Layout(options);
    layout.render();
  }
});

app.start({initialData: window.initialData});
```

All we've done here is pass `window.initialData` as an argument to `app.start`.
Maintaining a dependency on the page in our driver file is perfectly
acceptable - that is its purpose after all! When we want to test our app, we can
simply inject the options directly into `Layout` in our test runner. For this
reason, it's preferable to keep our `driver.js` file as simple as possible - we
want to minimize the amount of setup code we need to duplicate.


[router]: ../approuter/README.md "AppRouter"
