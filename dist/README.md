## Website Performance Optimization portfolio project

Your challenge, if you wish to accept it (and we sure hope you will), is to optimize this online portfolio for speed! In particular, optimize the critical rendering path and make this page render as quickly as possible by applying the techniques you've picked up in the [Critical Rendering Path course](https://www.udacity.com/course/ud884).

### Getting started

Some useful tips to help you get started:

1. Clone or donwload the [repository from GitHub](https://github.com/robertozanchi/website-optimization) and copy the ```dist``` directory on your local machine
1. To inspect the site on your phone, you can run a local server

  ```bash
  $> cd /path/to/your-project-folder
  $> python -m SimpleHTTPServer 8080
  ```

1. Open a browser and visit localhost:8080
1. Download and install [ngrok](https://ngrok.com/) to make your local server accessible remotely.

  ``` bash
  $> cd /path/to/your-project-folder
  $> ngrok http 8080
  ```

1. Copy the public URL ngrok gives you and run it through [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)

###Optimization

####Critical Rendering Path for index.html

The following steps were taken to optimize loading of index.html, achieving a PageSpeed Insights score of 94:

######CSS

1. Inlined CSS into the HTML document head to avoid loading style.css 

1. Added ```media="print"``` media query attribute to print.css

1. [Minified](http://www.cleancss.com/css-minify/) all inline CSS code and CSS files

######JavaScript

1. Loaded Google Fonts asyncronously [using JavaScript](https://www.lockedowndesign.com/load-google-fonts-asynchronously-for-page-speed/), replacing render-blocking stylesheet

1. Moved Google Analytics scripts to the end of the HTML document body to defer loading

1. Set async property on ```<script src="http://www.google-analytics.com/analytics.js"></script>```

######Images

1. Compressed all images with a ["lossy"](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization#lossless-vs-lossy-image-compression) compression filter using [Kraken](https://kraken.io/web-interface)

1. Created a resized copy of pizzeria.jpg - named pizzeria.sml.jpg - to load on index.html using [Picresize](http://www.picresize.com/)

Other possible steps to increase page loading speed include [leveraging browser chaching](https://developers.google.com/speed/docs/insights/LeverageBrowserCaching) and [compression](https://developers.google.com/speed/docs/insights/EnableCompression)

####Framerate for pizza.html

To optimize framerate in views/pizza.html, I modified views/js/main.js to increase frames per second rate to 60 fps or higher.

######Optimize updatePositions() function in main.js

Source code for updatePositions()

```js
// Moves the sliding background pizzas based on scroll position
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");

  var items = document.querySelectorAll('.mover');
  for (var i = 0; i < items.length; i++) {
    var phase = Math.sin((document.body.scrollTop / 1250) + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }
```

Optimized code for updatePositions()

1. Moved scrollTop method calculation outside of the loop

1. Applied translateX() and translateZ() transform functions to the sliding pizza elements

```js
// Moves the sliding background pizzas based on scroll position
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");

  var items = document.querySelectorAll('.mover');
  // Move scrollTop calculation outside of the loop
  var scroll = (document.body.scrollTop / 1250);

  for (var i = 0; i < items.length; i++) {
    var phase = Math.sin(scroll + (i % 5));
    var left = -items[i].basicLeft + 1000 * phase + 'px';
    // Applies translateX and translateZ transform functions
    items[i].style.transform = "translateX("+left+") translateZ(0)";
  }
```

######Optimize document.addEventListener() function in main.js

Source code for document.addEventListener()

```js
// Generates the sliding pizzas when the page loads.
document.addEventListener('DOMContentLoaded', function() {
  var cols = 8;
  var s = 256;
  for (var i = 0; i < 200; i++) {
    var elem = document.createElement('img');
    elem.className = 'mover';
    elem.src = "images/pizza.png";
    elem.style.height = "100px";
    elem.style.width = "73.333px";
    elem.basicLeft = (i % cols) * s;
    elem.style.top = (Math.floor(i / cols) * s) + 'px';
    document.querySelector("#movingPizzas1").appendChild(elem);
  }
  updatePositions();
});
```

Optimized code for document.addEventListener()

1. Changed number of sliding pizzas to 50

1. Compressed images/pizza.png

```js
// Generates the sliding pizzas when the page loads.
document.addEventListener('DOMContentLoaded', function() {
  var cols = 8;
  var s = 256;
  // Change number of sliding pizzas to 50
  for (var i = 0; i < 50; i++) {
    var elem = document.createElement('img');
    elem.className = 'mover';
    // Use new resized and compressed image file
    elem.src = "images/pizza.png";
    elem.style.height = "100px";
    elem.style.width = "73.333px";
    elem.basicLeft = (i % cols) * s;
    elem.style.top = (Math.floor(i / cols) * s) + 'px';
    document.querySelector("#movingPizzas1").appendChild(elem);
  }
  updatePositions();
});
```

####Resize pizzas in pizza.html

In order to resize the pizzas in under 5ms in pizza.html, changes were made in main.js.

######Pizza sizes

1. Updated changePizzaSizes()

Source code for changePizzaSizes()

```js
// Iterates through pizza elements on the page and changes their widths
function changePizzaSizes(size) {
  for (var i = 0; i < document.querySelectorAll(".randomPizzaContainer").length; i++) {
    var dx = determineDx(document.querySelectorAll(".randomPizzaContainer")[i], size);
    var newwidth = (document.querySelectorAll(".randomPizzaContainer")[i].offsetWidth + dx) + 'px';
    document.querySelectorAll(".randomPizzaContainer")[i].style.width = newwidth;
  }
}
```

Optimized code for changePizzaSizes()

1. Pulled the determineDx() function out of the loop.

1. Pulled newwidth calculation out of the loop

1. Created a new variable "elements" to hold all pizza elements

1. Looped through elements to apply new width

```js
// Iterates through pizza elements on the page and changes their widths
function changePizzaSizes(size) {
  // Pulled determineDx() out of the loop
  var dx = determineDx(document.querySelector(".randomPizzaContainer"), size);
  // Pulled newwidth calculation out of the loop
  var newwidth = (document.querySelector(".randomPizzaContainer").offsetWidth + dx) + 'px';
  // Created a new variable "elements" to hold all pizza elements
  var elements = document.querySelectorAll(".randomPizzaContainer");
  // Loop through elements to apply new width
  for (var i = 0; i < elements.length; i++) {
    elements[i].style.width = newwidth;
  }
}
```