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

The following steps were taken to optimize loading of index.html, to achieve a PageSpeed Insights score > 90:

######CSS

1. Inlined [minified CSS](http://www.cleancss.com/css-minify/) into the HTML document head to avoid loading style.css 

1. Added ```media="print"``` media query attribute to print.css

######JavaScript

1. Loaded Google Fonts asyncronously [using JavaScript](https://www.lockedowndesign.com/load-google-fonts-asynchronously-for-page-speed/), replacing render-blocking stylesheet

1. Moved Google Analytics scripts to the end of the HTML document body to defer loading

1. Set async property on ```<script src="http://www.google-analytics.com/analytics.js"></script>```

######Browser Caching

https://developers.google.com/speed/docs/insights/LeverageBrowserCaching

######Images

1. Compressed all images with a ["lossless"](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization#lossless-vs-lossy-image-compression) compression filter using [Kraken](https://kraken.io/web-interface)

1. Resized large images (i.e. pizzeria.jpg) using [Picresize](http://www.picresize.com/)

####Framerate for pizza.html

To optimize views/pizza.html, you will need to modify views/js/main.js until your frames per second rate is 60 fps or higher. You will find instructive comments in main.js. 

######Pizza sizes


1. Updated changePizzaSizes()

####Resize pizzas in pizza.html

###Results
