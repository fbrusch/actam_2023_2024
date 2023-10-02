# Envelopes

## Limiting sounds

* we have seen that we can create oscillators, start and stop them
* We can `start()` and `stop()` oscillators at definite times with `setTimeout()` to obtain _notes_
* that's not the best way, for different reasons
 * abrupt on and off edges are "clicky", for example 
* we can create a _sound envelope_, that give us much more power on the note features


## Envelopes

![](https://en.wikipedia.org/wiki/File:ADSR_parameter.svg)

 * in an envelope, amplitude is modulated to create a sound of finite duration
 * we can create envelopes with different shapes, that can be used to create different sounds
 * the most common envelopes are:
    * **ADSR**: Attack, Decay, Sustain, Release

## Timing

* How can we control amplitude to create the envelope? Can we `setTimeout` changes in amplitude? (for istance through `setTimeout(() => g.gain.value = a)`)
* Doesn't seem a good idea...

## Timing parameters

* There are different ways to change the value of an audio parameter:
    * through `<parameter>.value`
    * controlling with a wafeform (e.g. `o.connect(g.gain)`)
    * through web audio API scheduled events!


## Scheduling change

* There are some methods to schedule changes in audio parameters:
```js
setValueAtTime()
linearRampToValueAtTime()
setTargetAtTime()
exponentialRampToValueAtTime()
setValueCurveAtTime()
```

## Scheduling change (2)

Example:
```js
g.gain.setValueAtTime(0, c.currentTime)
g.gain.linearRampToValueAtTime(1, c.currentTime + 1)
g.gain.linearRampToValueAtTime(0, c.currentTime + 2)
```

Sets the gain to 0 at time 0, and ramps to 1 at by time 1

<button onclick="playBeep()">play beep</button>
<script>
    const c = new AudioContext();
    function playBeep() {
        var o = c.createOscillator();
        var g = c.createGain();
        o.connect(g);
        g.connect(c.destination);
        o.start(0);
        g.gain.setValueAtTime(0, c.currentTime)
        g.gain.linearRampToValueAtTime(1, c.currentTime + 1)
        g.gain.linearRampToValueAtTime(0, c.currentTime + 2)
    }
</script>

## Exercise

Create a page with a button and two sliders

* The button plays a note
* The sliders control the attack and release time of the note

## Interacting with page elements

* How do we interact with page elements?
* Through the DOM API!
* DOM = Document Object Model
* The DOM is a tree of nodes, that represent page elements
* From JS, we can obtain (find) page elements in various ways
* Once we have a reference to a page element, we can do various things

## Getting a reference to a page element

* We can get a reference to a page element by using the `getElementById()` method
* Example:

```javascript
const e = document.getElementById('myButton');
```

* We can set an event handler on a page element:

```javascript
e.addEventListener('click',
    function() {
        alert("clicked");
    })
```

<button id="myButton">Click me!</button>
<script>
    const e = document.getElementById('myButton');
    e.addEventListener('click',
        function() {
            alert("clicked");
    })
</script>

## attack-release

<iframe src="AttackRelease.html" width="300" height="300"></iframe>

## Sequencer in the head

* so we've seen how to schedule notes with `setTimeout`
* and how to precisely control audio parameters change with `*AtTime` methods
* we have all we need for the simplest possible sequencer

## Exercise

* write a function `playSequence` that takes a list/array of notes and plays it repeatedly
* we have to be able to change the notes while the sequence is playing
* example:
```javascript
var notes = [0,2,4,0,0,2,4,0];
playSequence(notes);
[...after a while...]
notes[2] = 15 // changes third note
````

## Solution

```javascript
var notes = [4, 3, 2, 5, 1, 6, 10, 12, 45, null, null, 12];

var period = 500;
var attack = 0.01;
var release = 0.1;

function playSequence(notes, i) {
    if (i >= notes.length) i = 0;
    playNote(noteFrequency(notes[i]), attack, release);
    setTimeout(() => playSequence(notes, i + 1), period);
}

playSequence(notes, 0);
```

## Exercises

* can we introduce pauses?
    * play Fra Martino with the correct delays
* is the system polyphonic
    * define another sequence to play over (like a bassline, or a countermelody)
* can we associate parameters to sequences?
    * for example, can we play a melody with different attack and release times?
    * can we modify parameters while the sequence is playing?

## Solution

```javascript
var options = {
    period: 500,
    attack: 0.01,
    release: 0.1
}

function playSequence(notes, i, options) {
    if (i >= notes.length) i = 0;
    playNote(noteFrequency(notes[i]), options.attack, options.release);
    setTimeout(() => playSequence(notes, i + 1, options), options.period);
}
```

---

## Different sections

* now we can play different "sections", with different parameters and even different times

```javascript
playSequence([1,5,10,4,3],0,{attack:0.01, release:0.1,period:200})
setTimeout( () => playSequence([-4,-5,-10,-20,-30],0,{attack:0.1, release:0.2,period:200}), 5000)
setTimeout( () => playSequence([25,35,25,26,80],0,{attack:0.01, release:0.1,period:200}), 10000)
````
* 
---

## Exercise

* write the best tune you can think with sequences, as a standalone html file that starts pushing a button
* submit as a pull request to the github `tunes` folder



