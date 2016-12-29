## Website Performance Optimization portfolio project

### Getting started

Of course I accepted the challenge consisting in optimizing PageSpeed score for Index.html and frames per second for pizza.html.

To get started, I started by cloning the github repository into my computer and to push it online on my own github account as a project page available at https://baptistedodane.github.io/webopimization_Udacity/. I had to create a new branch called gh-pages. That's why all my work is located outside the master branch. Doing so, I was able to test index.html for PageSpeed.

###Part 1: Optimize PageSpeed Insights score for index.html

I started by measuring index.html PageSpeed. The result wasn't promising, with poor 27 for mobile and 29 for desktops. It gave me meaningful advice on how to improve this score thanks to three main changes:

1. Compress & Resize Images. I used online tools to reduce the pictures weight without reducing their quality. I also created a new version of the pizzeria.jpg. The website used the same picture for both index.html and pizza.html while index.html only needed a 100 pixels wide picture. I created pizzeria-small.jpg with a 100 pixels width, was greatly reduced the necessary downloads.
2. Inline & improve CSS. Index.html used to load two differents css files to render the page. I inlined the main css file in index.html within the head. I also added a media attribute to the print.css file so it is only loaded when the user wants to print the page.
3. Avoid render blocking Js. There were a couple of Js scripts that were render blocking while not being absolutely necessary to load the page. I simply add an async attribute to avoid that problem. I also optimized the webfont loading script that used to take a lot time.

###Part 2: Optimize Frames per Second in pizza.html

To optimize views/pizza.html, I had to modify views/js/main.js until your frames per second rate is 60 fps or higher and until the time to resize pizzas is below 5 ms.

I started by looking at the page timeline when scrolling and resizing pizzas to have more insights on where the pitfalls were. Loops created issues, due to heavy functions inside loops and long unnecessary extra-loops.

#####Remove Jank

I modified the loop inside the updatePositions that was generating janks. I put the top variable definition outside the loop and optimized the css animation when updating Positions with translateX and translateZ.
```javascript
var items = document.querySelectorAll('.mover');
/* I put the top variable outside the loop as it requires the scrollTop method
that generates extra computations and slow the process*/
var top = (document.body.scrollTop / 1250);

/* I optimized the loop and used translateX and translateZ functions to speed up
the css animation for the sliding pizzas */
for (var i = items.length; i--;) {
  var phase = Math.sin( top + (i % 5));
  var left = -items[i].basicLeft + 1000 * phase + 'px';
      items[i].style.transform = "translateX("+left+") translateZ(0)";
}
```

I modified the last loop that was running updatePositions when the user scrolls the page. The loop created much more pizzas that was necessary. I reduced the number to 40 what was enough to create the effect without janks when scrolling.
```javascript
window.addEventListener('scroll', updatePositions);

/* There are no need for 200 sliding pizzas on this page. To reduce the loading
time and reach a 60 frames per second animation I reduced this number to 40 What
speeds the loop */
document.addEventListener('DOMContentLoaded', function() {
  var cols = 8;
  var s = 256;
  for (var i = 0; i < 40; i++) {
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
No more Jank!

#####Decrease pizza resize time

I optimized the resizePizzas function to reach the 5 ms goal to resize pizzas. Changes are explained with comments in the code below. I mostly removed the determineDx function that was called in each changePizzaSizes loop what was a real pain. I optimized this loop with a new way to change the pizza size without dx.

```javascript
/* I removed the determineDx function that was called inside the loop to change
each pizza size. I only kept the windowWidth variable to change the pizza
size according to the screen width with the changePizzaSizes function, as well
as the sizeSwitcher function. */
var windowWidth = document.getElementById("randomPizzas").offsetWidth;

// Changes the slider value to a percent width
function sizeSwitcher (size) {
  switch(size) {
    case "1":
      return 0.25;
    case "2":
      return 0.3333;
    case "3":
      return 0.5;
    default:
      console.log("bug in sizeSwitcher");
  }
}
  var newSize = sizeSwitcher(size);


// Iterates through pizza elements on the page and changes their widths
/* I adapted the function to make it work without the determineDx function
and optimized the loop. The new variable allPizzas contains all pizzas that
need to be modified (using the same principle as in the original version
 for the loop)*/
function changePizzaSizes(size) {
  var allPizzas = document.getElementsByClassName("randomPizzaContainer");

  for (var i = 0; i < allPizzas.length; i++) {
    allPizzas[i].style.width = (sizeSwitcher(size) * windowWidth) + 'px';
  }
}

changePizzaSizes(size);
```

Thanks to this change, time to resize pizza is now below 5 ms.
