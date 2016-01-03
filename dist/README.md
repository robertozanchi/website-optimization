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

### Optimization

#### Optimize Critical Rendering Path for index.html

The following steps were taken to optimize loading of index.html, to achieve a PageSpeed Insights score > 90:

1. Resize pizzeria.jpeg

1. Replaced CSS in style.css with [minified CSS](http://www.cleancss.com/css-minify/) placed into the page head

1. Added media query ```media="print"``` to print.css

1. Loaded Google Fonts asyncronously [using JavaScript](https://www.lockedowndesign.com/load-google-fonts-asynchronously-for-page-speed/), replacing render-blocking stylesheet

1. Moved both Google Analytics scripts to the end of the page body

1. Set async property on ```<script src="http://www.google-analytics.com/analytics.js"></script>```

####Part 2: Optimize Frames per Second in pizza.html

To optimize views/pizza.html, you will need to modify views/js/main.js until your frames per second rate is 60 fps or higher. You will find instructive comments in main.js. 

1. Updated changePizzaSizes()

