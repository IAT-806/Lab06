# Lab 06 — From Scratch

In class we wrote the `Ball` class in its own file, made the balls respond to the mouse, and then made them respond to your body through the webcam with ml5.js.

In this lab you write the whole thing again, **from scratch, without AI**. You can bring it to class, or if you finished it, add it to your website. You can submit this AFTER class on Wednesday. Next week. I'll do a little lecture, and we'll do studio time.

## The rules

- Please do not use AI for this lab assignment. It might be slow, but I want to help build your confidence.
- **You can read the code from class.** It's in the [in-class repo, Week-6](https://github.com/IAT-806/in-class/tree/main/Week-6). Read it before you start, understand what's happening. Write a plan for what you need to do.
- **You can use the reference pages.** [p5.js reference](https://p5js.org/reference/) and [ml5.js bodySegmentation reference](https://docs.ml5js.org/#/reference/body-segmentation).
- You can google. And ask AI for things. But generic things. Not ones that solve your problem:
  - For example. How can I make a method of a class accept multiple arguments.
  - If you do, please share what you asked.

Finish each step and check that it works before you start the next one.

## Set up

1. In your website repo, copy your `lab-05/` folder and rename the copy `lab-06`.
2. Delete everything inside `lab-06/sketch.js`.
3. Make a new empty file next to it: `lab-06/Ball.js`.
4. In `lab-06/index.html`, add a `<script>` tag for `Ball.js`. It has to come **before** the one for `sketch.js`.

## Step 1: the `Ball` class

1. In `Ball.js`, write `class Ball`. The `constructor` stores position, color, and radius.
2. Add `move()`, `changeSize()`, and `show()`.
3. Add `update()`. It calls the other three.
4. Add `createRandomBall()`. It returns a new ball with a random position and color.
5. In `sketch.js`, fill an array with balls in `setup()`. Update all of them in `draw()`.

**Check:** a cloud of jittering balls.

## Step 2: `respond()`

1. In the `constructor`, store the original color and a highlight color.
2. Add `highlight()` and `deHighlight()`.
3. Add `respond(active)`. It calls one or the other.

**Check:** call `respond(true)` from `draw()`. Look. Take it out.

## Step 3: react to a click

1. Add `contains(px, py)`. It returns `true` if the point is inside the ball.
2. In `sketch.js`, add `mousePressed()`. For every ball, pass `contains(mouseX, mouseY)` to `respond()`.

**Check:** click a ball, it changes color. Click empty space, it changes back.

## Step 4: the webcam

Read the [ml5.js bodySegmentation reference](https://docs.ml5js.org/#/reference/body-segmentation) first. The code under **Quick Start** is what we used in class. Use it.

1. Add the ml5.js `<script>` tag to `index.html`.
2. In `sketch.js`, add variables for the video, the model, the result, and the options. We used `maskType: "person"`.
3. Make `setup()` `async`. Create and hide the video. Load the model with `await`. Call `detectStart()`.
4. Write the callback. It stores the result.
5. In `Ball.js`, add `isOnBody(mask)`. Read the mask pixel at the ball's position with `mask.get()`. Return `true` or `false` based on its alpha.
6. In `draw()`, if there is a result, pass `isOnBody()` to `respond()` for every ball.

**Check:** allow the camera. Stand in front of it. Balls on your body change color. If it's backwards, flip your alpha test.

To see the mask, draw it: `image(segmentation.mask, 0, 0)`.

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
3. The submission will be after class on Wednesday.

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
