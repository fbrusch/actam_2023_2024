## Introduction to HTML Canvas

### The Challenge of Drawing in HTML

- Can we create arbitrary drawings/graphics using traditional HTML/CSS?
- Issues with using basic HTML elements for complex graphics.

### Example:

```html
<div
  style="width:100px; height:100px;
  border-radius:50%;
  background-color:blue;"
></div>
```

<div style="width:100px; height:100px; border-radius:50%; background-color:blue;"></div>

---

## The Evolution of Web Graphics

### The Need for Better Drawing Tools

- Limitations of traditional methods.
- Plugins like Flash filled the void. (do you remember them?)

### Adobe Flash

- Adobe Flash was a multimedia software platform used for production of animations, rich web applications, desktop applications, mobile apps, and mobile games.
- **Capabilities**:
  - Vector Graphics: Enabled lightweight animations on the web.

---

- **Interactive Media**: Games, interactive forms, and more.
- **Video Playback**: Brought video streaming to many websites.
- **Popularity**:
  - Became ubiquitous in the 2000s; many websites relied on it for multimedia content.

---

### Why Did Flash Become Obsolete?

- Security Concerns:

  - Repeated vulnerabilities led to security issues, requiring frequent patches.

- Mobile Limitations:
  - Not supported on iOS: Apple's decision to not support Flash on its iPhone was a significant blow.
  - Performance and battery issues on Android.

---

- Rise of Alternatives:
  - Modern web standards (HTML5, CSS3, JavaScript) started to offer similar capabilities without the need for plugins.
- The Void:
  - Immediate Impact: Many older websites and games became inaccessible or broken.
  - Long-Term: Paved the way for more secure, efficient, and responsive web technologies.
  - Legacy Flash content slowly got converted or archived.

### Enter HTML5 Canvas

- Introduction of `<canvas>` with HTML5.
- A step forward: native browser support, no plugins.

---

## Capabilities of Canvas

### What Can You Do with Canvas?

1. Draw and fill shapes with colors and gradients.
2. Render text with various styles.
3. Load and manipulate images.
4. Perform pixel operations, like filters.
5. Use transformations and matrices for advanced graphics.

---

### Canvas Context

- Reference to the drawing functions.

```javascript
var canvas = document.getElementById("myCanvas");
var ctx = canvas.getContext("2d");
```

### Properties

- lineWidth, strokeStyle, fillStyle, etc.

### Basic Drawing Methods

- beginPath(), moveTo(), lineTo(), arc(), fill(), stroke().

### Gradients & Patterns

```javascript
var grd = ctx.createLinearGradient(0, 0, 200, 0);
grd.addColorStop(0, "red");
grd.addColorStop(1, "blue");
ctx.fillStyle = grd;
ctx.fillRect(50, 50, 150, 100);
```

<canvas id="gradients"></canvas>

<script>
var canvas = document.getElementById('gradients');
var ctx = canvas.getContext('2d');
    var grd = ctx.createLinearGradient(0,0,200,0);
grd.addColorStop(0,"red");
grd.addColorStop(1,"blue");
ctx.fillStyle = grd;
ctx.fillRect(50, 50, 150, 100);
</script>

---

### Animations

- Clear and redraw to create motion.

```javascript
ctx.clearRect(0, 0, canvas.width, canvas.height);
// draw the next frame
```

---

### The Problem with `setTimeout`

1. **Inconsistency**: `setTimeout` does not guarantee that the callback will run exactly after the specified time due to other tasks that may be queued. This can lead to janky or irregular animations.
2. **Efficiency**: When the tab or window is not in focus, `setTimeout` continues to run, wasting resources and potentially draining battery on mobile devices.
3. **Lack of synchronization**: It does not synchronize with the browser's refresh rate, potentially leading to animations that aren't smooth.

---

### `requestAnimationFrame`

1. **Frame synchronization**: It runs before the next repaint, synchronizing with the browser's refresh rate (usually 60fps). This ensures smoother animations.
2. **Intelligent timing**: The browser can optimize the animation for performance, pausing it when the tab or window isn't in focus.
3. **Battery efficiency**: It doesn't run when the tab is inactive, preserving battery life on mobile devices.

### How to Use `requestAnimationFrame`

1. Call `requestAnimationFrame` and pass a callback function. This function will be invoked before the next repaint.
2. For continuous animations, invoke `requestAnimationFrame` recursively within the callback.

```javascript
function animate() {
  // ... update your animation ...
  requestAnimationFrame(animate);
}
requestAnimationFrame(animate);
```

---

### Keeping Track of Time with `requestAnimationFrame`

### Why We Need Timing Information

1. Animation adjustments based on time ensure smoother and consistent visuals.
2. To cope with variable frame rates and performance hiccups.

### How `requestAnimationFrame` Provides Timing

- The callback function receives a single argument: a high-precision timestamp.
- Represents the number of milliseconds elapsed since the page started loading.

### Example: Moving a Square Horizontally Based on Elapsed Time

```javascript
let lastTimestamp = 0;
const pixelsPerSecond = 100; // Square will move 100 pixels per second.

function animate(timestamp) {
  const deltaTime = timestamp - lastTimestamp; // Time since last frame.

  const moveDistance = (deltaTime / 1000) * pixelsPerSecond; // Convert to seconds.
  // ... adjust square's x-position by moveDistance ...

  lastTimestamp = timestamp; // Update lastTimestamp for next frame.
  requestAnimationFrame(animate);
}

requestAnimationFrame(animate);
```
