# Lab 06 — From Scratch

In class we wrote the `Ball` class in its own file, made the balls respond to a click, and then made them respond to your body through the webcam with ml5.js.

In this lab you write the whole thing again, **from scratch, without AI**. Then you put it on your website.

## The rules

- **No AI.** No ChatGPT, no Claude, no Gemini, no Copilot. Turn Copilot off in VS Code before you start.
- **You can read the code from class.** It's in the [in-class repo, Week-6](https://github.com/IAT-806/in-class/tree/main/Week-6). Read it before you start. Go back to it when you're stuck. Don't copy and paste. Type it.
- **You can use the reference pages.** [p5.js](https://p5js.org/reference/) and [ml5.js bodySegmentation](https://docs.ml5js.org/#/reference/body-segmentation).

Check that each step works before you start the next one. Ask if you're stuck.

## Set up

1. Copy your `lab-05/` folder and rename the copy `lab-06`.
2. Empty `sketch.js`. Make a new empty `Ball.js` next to it.
3. In `index.html`, add a `<script>` tag for `Ball.js` before the one for `sketch.js`.

## Step 1: the `Ball` class

1. In `Ball.js`, write `class Ball` with a `constructor` that stores position, color, and radius.
2. Add `move()`, `changeSize()`, and `show()`.
3. Add `update()`. It calls the other three.
4. Add a function `createRandomBall()` that returns a new ball with a random position and color.
5. In `sketch.js`, make an array of balls in `setup()` and update them all in `draw()`.

**Check:** a cloud of jittering balls.

## Step 2: `respond()`

1. In the `constructor`, store the original color and a highlight color.
2. Add `highlight()` and `deHighlight()`.
3. Add `respond(active)`. It calls one or the other.

**Check:** call `respond(true)` from `draw()`, look, take it out.

## Step 3: react to a click

1. Add `contains(px, py)`. It returns `true` if the point is inside the ball.
2. In `sketch.js`, add `mousePressed()`. For every ball, pass `contains(mouseX, mouseY)` to `respond()`.

**Check:** click a ball, it changes color. Click empty space, it changes back.

## Step 4: the webcam

Read the [ml5.js bodySegmentation reference](https://docs.ml5js.org/#/reference/body-segmentation). The code under **Quick Start** is what we used in class.

1. Add the ml5.js `<script>` tag to `index.html`.
2. In `sketch.js`, add variables for the video, the model, the result, and the options. We used `maskType: "person"`.
3. Make `setup()` `async`. Create and hide the video capture. Load the model with `await`. Call `detectStart()`.
4. Write the callback. It stores the result.
5. In `Ball.js`, add `isOnBody(mask)`. Read the mask pixel at the ball's position with `mask.get()` and return `true` or `false` from its alpha.
6. In `draw()`, if there is a result, pass `isOnBody()` to `respond()` for every ball.

**Check:** allow the camera. Stand in front of it. Balls on your body change color. If it's backwards, flip your alpha test.

To see the mask, draw it: `image(segmentation.mask, 0, 0)`.

### Bonus

- Keep the click **and** the body working at the same time.
- Make the active balls do more than change color.
- Add a second class that `extends Ball` and responds differently.

Put a **comment** above each method that says what it does, in your own words.

## How to hand it in

1. Edit `lab-06/index.html`: change the title and the `<h1>` to Lab 06. Write a sentence about which step you had to look up.
2. Add a link to it on `projects/iat-806/index.html`.
3. Commit and sync, then check that it works at:

```
https://your-username.github.io/projects/iat-806/lab-06/
```

GitHub Pages is `https`, so the camera works there. The browser will ask for permission.

In **Canvas**, submit:

1. The link to your live sketch.
2. The link to your `lab-06` folder in your GitHub repo.

Need a reminder on adding a folder to your site? See [section 4 of the personal-website README](https://github.com/IAT-806/personal-website#4-add-a-submission).

## If something doesn't work

Open the browser console (right-click the page → **Inspect** → **Console**) and read the red text.

- **`Ball is not defined`**: the `<script>` tag for `Ball.js` is missing, or it comes after `sketch.js`.
- **`ml5 is not defined`**: the ml5.js `<script>` tag is missing.
- **`await is only valid in async functions`**: `setup()` isn't `async`.
- **`Cannot read properties of undefined (reading 'mask')`**: `draw()` runs before the first result arrives. Check that the result exists first.
- **The camera is black**: check the camera icon in the address bar. Live Server on `localhost` is fine. Any other `http` address isn't.
- **It's slow**: lower the number of balls. We used 500 in class.
- **Works with Live Server but not on GitHub Pages**: check capital letters. `Ball.js` is not `ball.js`.
