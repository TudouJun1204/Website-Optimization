## Website Performance Optimization portfolio project

Your challenge, if you wish to accept it (and we sure hope you will), is to optimize this online portfolio for speed! In particular, optimize the critical rendering path and make this page render as quickly as possible by applying the techniques you've picked up in the [Critical Rendering Path course](https://www.udacity.com/course/ud884).

To get started, check out the repository and inspect the code.

### Getting started

#### Part 1: Optimize PageSpeed Insights score for index.html

Some useful tips to help you get started:

1. Check out the repository
-  To inspect the site on your phone, you can run a local server

  ```bash
  $> cd /path/to/your-project-folder
  $> python -m SimpleHTTPServer 8080
  ```

2. Open a browser and visit localhost:8080
- Download and install [ngrok](https://ngrok.com/) to the top-level of your project directory to make your local server accessible remotely.

  ``` bash
  $> cd /path/to/your-project-folder
  $> ./ngrok http 8080
  ```

- Copy the public URL ngrok gives you and try running it through PageSpeed Insights! Optional: [More on integrating ngrok, Grunt and PageSpeed.](http://www.jamescryer.com/2014/06/12/grunt-pagespeed-and-ngrok-locally-testing/)

Profile, optimize, measure... and then lather, rinse, and repeat. Good luck!

#### Part 2: Optimize Frames per Second in pizza.html

To optimize views/pizza.html, you will need to modify views/js/main.js until your frames per second rate is 60 fps or higher. You will find instructive comments in main.js.

1. downscaled and compressed the pizzeria image.
  - According to the pizza.html page, you don't need an image larger than 400 px wide.

2. Minify JS
  - Compressed JavaScript files as main-mim.js.

2. to achieve a resizing time less than 5 ms
  - Replace query selector with getElementById();
  - Replace querySelectorAll to getElementsByClassName();
  which is supposed to be faster.

```js
// .. code
document.addEventListener('DOMContentLoaded', function() {
  var cols = 8;
  var s = 256;
  var pizzasOnPage = Math.ceil(screen.height / s) * 8
  console.log(pizzasOnPage);
  var movingPizzas = document.getElementById('movingPizzas1');
  /* Move the items variable out of the loop above.
     Replace query selector with getElementById, which is supposed to be faster. */

  for (var i = 0; i < pizzasOnPage; i++) {
    var elem = document.createElement('img');
    elem.className = 'mover';
    elem.src = "images/pizza.png";
    elem.style.height = "100px";
    elem.style.width = "73.333px";
    elem.basicLeft = (i % cols) * s;
    elem.style.top = (Math.floor(i / cols) * s) + 'px';
    movingPizzas.appendChild(elem);
  }
  items = document.getElementsByClassName('mover');
  updatePositions();
});
// .. code
```

3. Due to the mod 5 operator, there are only 5 different values for the phase. Whichever the value of i, i%5 is always a value from 0 to 4. So, calculating phase values inside the main loop is a waste of resources.

   the best option is making two loops, one for the phases (0 to 4) and the other for the positions (0 to items.length).


```js
// .. code
var items = document.getElementsByClassName('mover');
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");
  var phase = [];
  var scrollTop = document.documentElement.scrollTop;
  for (var i = 0; i < 5; i++) {
      phase.push(Math.sin(scrollTop / 1250 + i));
  }

  for (var i = 0, max = items.length; i < max; i++) {
      items[i].style.left = items[i].basicLeft + 100 * phase[i%5] + 'px';
  }
// .. code
```

### Optimization Tips and Tricks
* [Optimizing Performance](https://developers.google.com/web/fundamentals/performance/ "web performance")
* [Analyzing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "analyzing crp")
* [Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "optimize the crp!")
* [Avoiding Rendering Blocking CSS](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "render blocking css")
* [Optimizing JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [Measuring with Navigation Timing](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api"). We didn't cover the Navigation Timing API in the first two lessons but it's an incredibly useful tool for automated page profiling. I highly recommend reading.
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">The fewer the downloads, the better</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">Reduce the size of text</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">Optimize images</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP caching</a>

### Customization with Bootstrap
The portfolio was built on Twitter's <a href="http://getbootstrap.com/">Bootstrap</a> framework. All custom styles are in `dist/css/portfolio.css` in the portfolio repo.

* <a href="http://getbootstrap.com/css/">Bootstrap's CSS Classes</a>
* <a href="http://getbootstrap.com/components/">Bootstrap's Components</a>
