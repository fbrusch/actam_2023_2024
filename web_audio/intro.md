---
marp: true
theme: gaia
class:
    - invert
    - lead
---


# Web Audio API

## What's an API?

* So far we played with coding that "just" elaborated values, and changed the state of our runtime/executer/interpreter
* But coding can "reach out" to the external world, both for knowing what's outside (read), and for changing it (write)

---
* Increasingly many parts of the world offer interfaces that can be used directly by code
* This is in contrast with other kinds of interfaces, that are meant to be used by humans

---
* Example: a switch to turn a light on
    * the switch is designed to be mechanically activated by a human, that flips it with his finger
    * Increasingly many electric appliances can be controlled in other ways (e.g. via wifi, or bluetooth etc)
    * these other ways are meant to be used by machines, not humans
---

* This is what an API is: a way for machines to interact with _code_
* APIs open up some possibilities:
    * _automation_: if a bulb offers an API, it is possible to control it from a piece of code, and for instance to turn it on at a specific time (say every day at sunset), or when some event occurs (e.g. someone enters the room) etc
    * this is something that a human could do, but it's more practical/economical to have a machine for
---
* but there are things that are possible with APIs that are not possible without them
* take the bulb example again:
    * if the bulb is made with LEDs, it is difficult to control its brightness by means of voltage modulation
    * so here's an idea: if we want to cut its brightness by half, we just switch it on and off repeatedly
    * on average, the bulb will emit half of the full brightness
---


- "but we would see an annoying flickering light, not a dimmer one!"
* yes, but if we can control the bulb through an API: 
    * we can switch the light with a high frequency (say kHzs)
    * so that the naked eye smooths out the flickering (like what happens at the movies, or with your display!)

---

This to say that, with APIs, we can automate things (relieve humands from doing repetitive tasks), but we can also do things otherwise impossible.

What about music and sounds? can we create sounds/play music otherwise impossible?

Do you have any ideas? (Discussion)
https://www.youtube.com/watch?v=GmxJfGe6Smw&t=50s
https://www.youtube.com/watch?v=evHVh4bqaOQ&t=25s

---

# Web Audio Apis

* In the context of a browser, our Javascript runtime has access to some sound synthesis facilities

* These are accessible through an interface called Web Audio Apis

* Ingredients:
    * audio contexts
    * audio nodes
        * sources
        * processors/effects
    * (modular) routing

---

# Typical workflow 
(from https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)

1. Create audio context
2. Inside the context, create sources — such as <audio>, oscillator, stream
3. Create effects nodes, such as reverb, biquad filter, panner, compressor
4. Choose final destination of audio, for example your system speakers
5. Connect the sources up to the effects, and the effects to the destination.

---

# Example: a sine wave

```js
const c = new AudioContext() 
undefined
var o = c.createOscillator()
undefined
o.connect(c.destination)
AudioDestinationNode {maxChannelCount: 2, context: AudioContext, numberOfInputs: 1, numberOfOutputs: 0, channelCount: 2, …}
o.start()
o.stop()
```

Exercise: can you play a series of three beeps, of half a second each, spaces by half a second pause?

---

# Timing things

In Javascript, we have some timing primitives:

* `setTimeout`: allows to schedule the call to a function sometime in the future
* `setInterval`: allows to schedule the call to a function every so often

---
Example:
```js
function greet() {
    alert("ciao")
}

setTimeout(greet, 1000)
```

Can you play the beeps now?

---

# Controlling features

* we can change the frequency of the sine wave

```js
o.frequency.value = 550
````

Excercise: can you play a beep that alternates frequency between 440 and 550 every 0.5 seconds, for 3 times?

---

