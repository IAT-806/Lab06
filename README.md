# Lab 06 — From Scratch

In class we wrote the `Ball` class in its own file, made the balls respond to the mouse, and then made them respond to your body through the webcam with ml5.js.

In this lab you write the whole thing again, **from scratch, without AI**. Then you put it on your website.

## The rules

- **No AI.** No ChatGPT, no Claude, no Gemini, no Copilot. Turn Copilot off in VS Code before you start (click the Copilot icon in the status bar, bottom right, and disable it).
- **You can read the code from class.** It's in the [in-class repo, Week-6](https://github.com/IAT-806/in-class/tree/main/Week-6). Read it before you start, and go back to it when you're stuck. Don't copy and paste. Type it.
- **You can use the reference pages.** [p5.js reference](https://p5js.org/reference/) and [ml5.js bodySegmentation reference](https://docs.ml5js.org/#/reference/body-segmentation).

Finish each step and check that it works before you start the next one.

## Set up

1. In your website repo, copy your `lab-05/` folder and rename the copy `lab-06`.
2. Delete everything inside `lab-06/sketch.js`.
3. Make a new empty file next to it: `lab-06/Ball.js`.
4. In `lab-06/index.html`, add a `<script>` tag for `Ball.js`. It has to come **before** the one for `sketch.js`.

## Step 1: the `Ball` class

In `Ball.js`:

- A class `Ball` with a `constructor` that takes a position, a color, and a radius, and stores them as attributes.
- Methods `move()`, `changeSize()`, and `show()`. Each takes a parameter.
- A method `update()` that calls the other three.
- Outside the class, a function `createRandomBall()` that makes a ball at a random position with a random color and returns it.

In `sketch.js`:

- An empty array for the balls and a variable for how many you want.
- In `setup()`, a loop that fills the array with random balls.
- In `draw()`, a loop that updates every ball.

**Check:** a cloud of jittering balls.

## Step 2: `respond()`

The ball needs to remember two colors: the one it was made with, and the one it shows when it's active. Add them in the `constructor`.

Add three methods to `Ball`:

- `highlight()` switches to the active color.
- `deHighlight()` switches back.
- `respond(active)` takes `true` or `false` and calls one of the two.

**Check:** nothing changes on screen yet. To test it, call `respond(true)` on every ball from `draw()`, look, then take it out.

## Step 3: react to a click

Add a method `contains(px, py)` to `Ball`. It returns `true` if the point is inside the ball and `false` if it isn't. `dist()` and the radius are all you need.

In `sketch.js`, add `mousePressed()`. For every ball: is the mouse inside it? Pass the answer to `respond()`.

**Check:** click on a ball and it changes color. Click on empty space and it changes back.

## Step 4: the webcam

Read the [ml5.js bodySegmentation reference](https://docs.ml5js.org/#/reference/body-segmentation) first. The code under **Quick Start** and **Step-by-Step Guide** is what we used in class. Use it.

1. In `index.html`, add the ml5.js `<script>` tag from the reference page, after the p5.js one.
2. In `sketch.js`, add variables for the video, the model, the latest segmentation result, and the `options`. We used `maskType: "person"`.
3. Make `setup()` `async`. Create the video capture, size it to the canvas, and hide it. Load the model with `await`, then call `detectStart()` with the video and a callback.
4. Write the callback. It stores the result in your variable.
5. Add a method `isOnBody(mask)` to `Ball`. Read the mask's pixel at the ball's position with `mask.get()`. It gives back `[r, g, b, a]`. Decide from the alpha whether the ball is on the body, and return `true` or `false`.
6. In `draw()`, for every ball: if there is a segmentation result, pass `isOnBody()` to `respond()`.

**Check:** allow the camera. Stand in front of it. The balls on your body change color, the rest don't. If it's backwards, your alpha test is backwards.

If you can't tell what the mask looks like, draw it: `image(segmentation.mask, 0, 0)` in `draw()`.

### Bonus

- Keep the click **and** the body working at the same time.
- Make the active balls do more than change color.
- Add a second class that `extends Ball` and does something different when it responds.

Weird is good. Have fun with it.

Put a **comment** above each method that says what it does, in your own words.

## How to hand it in

1. Edit `lab-06/index.html`: change the title and the `<h1>` to Lab 06, and write a sentence or two about which step you had to look up and why.
2. Add a link to it on `projects/iat-806/index.html`.
3. Commit and sync, then check that it works at:

```
https://your-username.github.io/projects/iat-806/lab-06/
```

The browser will ask for camera permission. GitHub Pages is `https`, so the camera works there.

In **Canvas**, submit:

1. The link to your live sketch.
2. The link to your `lab-06` folder in your GitHub repo.

Need a reminder on adding a folder to your site? See [section 4 of the personal-website README](https://github.com/IAT-806/personal-website#4-add-a-submission).

## If something doesn't work

Open the browser console (right-click the page → **Inspect** → **Console**) and read the red text.

- **`Ball is not defined`**: the `<script>` tag for `Ball.js` is missing, or it comes after `sketch.js`.
- **`ml5 is not defined`**: the ml5.js `<script>` tag is missing, or it comes after `sketch.js`.
- **`await is only valid in async functions`**: `setup()` isn't `async`.
- **`Cannot read properties of undefined (reading 'mask')`**: the first frames run before the model has sent a result. Check that the result exists before you use it.
- **The camera is black or never asks for permission**: check the camera icon in the address bar. Live Server on `localhost` is fine. Any other `http` address isn't.
- **It's slow and laggy**: segmentation is heavy. Lower the number of balls. We used 500 in class.
- **Works with Live Server but not on GitHub Pages**: check capital letters in file names. `Ball.js` is not `ball.js`.
