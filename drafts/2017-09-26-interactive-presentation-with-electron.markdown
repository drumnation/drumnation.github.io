---
layout: post
title: "Building an Interactive Car Presentation Wrapped in Electron"
date: 2017-09-26 10:39:06 -0400
---

Install electron as a dev dependency

```bash
$ npm install electron --save-dev
```

Add this your package.json

```json
"main": "main.js"
```

Create a file called `main.js` and copy pasta this code.

```js
const { app, BrowserWindow } = require("electron");

let win = null;

function createWindow() {
// Initialize the window to our specified dimensions
win = new BrowserWindow({ width: 1080, height: 1920 });

// Specify entry point
win.loadURL("http://localhost:3000");

// Show dev tools
// Remove this line before distributing
win.webContents.openDevTools();

// Remove window once app is closed
win.on("closed", () => {
        win = null;
    });
}

app.on("ready", () => {
    createWindow();
});

app.on("activate", () => {
    if (win === null) {
        createWindow();
    }
});

app.on("window-all-closed", () => {
    if (process.platform !== "darwin") {
        app.quit();
    }
});
```
This will load the app inside of electron by typing:
```bash
electron .
```

# Alpha Channel Transparent Video

Now that the electron part is out of the way... One other feature of this app was a series of buttons that enable and disable transparent alpha channel video animations on top of a car image.  This was suprisingly difficult to figure out how to make work properly.

HTML5 added transparent video, but in order to use it React needs to be able to render to Canvas instead of ReactDOM.  I tried a number of different HTML5 components and while I was able to play video, I wasn't able to get the background of the video to be transparent.  That's when I discovered `react-html5video`. Since it renders to Canvas the transparent video plays correctly.

I modified the CSS to eliminate all the player controlls and whala I had a system allowing me to turn layers of tranparent video of electricity animations on top of a car diagram on and off.


