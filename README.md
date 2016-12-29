## Website Performance Optimization portfolio project

### Getting started

Of course I accepted the challenge consisting in optimizing PageSpeed score for Index.html and frames per second for pizza.html.

To get started, I started by cloning the github repository into my computer and to push it online on my own github account as a project page available at https://baptistedodane.github.io/webopimization_Udacity/. I had to create a new branch called gh-pages. That's why all my work is located outside the master branch. Doing so, I was able to test index.html for PageSpeed.

####Part 1: Optimize PageSpeed Insights score for index.html

I started by measuring index.html PageSpeed. The result wasn't promising, with poor 27 for mobile and 29 for desktops. It gave me meaningful advice on how to improve this score thanks to three main changes:

1. Compress & Resize Images. I used online tools to reduce the pictures weight without reducing their quality. I also created a new version of the pizzeria.jpg. The website used the same picture for both index.html and pizza.html while index.html only needed a 100 pixels wide picture. I created pizzeria-small.jpg with a 100 pixels width, was greatly reduced the necessary downloads.
2. Inline & improve CSS. Index.html used to load two differents css files to render the page. I inlined the main css file in index.html within the head. I also added a media attribute to the print.css file so it is only loaded when the user wants to print the page.
3. Avoid render blocking Js. There were a couple of Js scripts that were render blocking while not being absolutely necessary to load the page. I simply add an async attribute to avoid that problem. I also optimized the webfont loading script that used to take a lot time.

####Part 2: Optimize Frames per Second in pizza.html

To optimize views/pizza.html, you will need to modify views/js/main.js until your frames per second rate is 60 fps or higher. You will find instructive comments in main.js.

You might find the FPS Counter/HUD Display useful in Chrome developer tools described here: [Chrome Dev Tools tips-and-tricks](https://developer.chrome.com/devtools/docs/tips-and-tricks).
