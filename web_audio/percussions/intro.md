---
title: percussions
---

/_ Stile del bottone "Play Kick" _/

# Playing with samples

- we have seen how to generate sounds using oscillators, gains etc
- original waveforms are predefined
- what if we want to generate sounds using our own waveforms?

---

- why should we care?
- maybe we want to generate some percussive sounds
- kicks can be synthesised from a sine wave
- but other sounds can't

---

- for instance: a snare needs some white noise
- How can we generate white noise?

---

- in Web Audio we have a very general tool: `buffer` and `BufferSource`

- a BufferSource is a node that can play a `buffer`
- a buffer is a chunk of audio data, represented by _samples_
- we can create a buffer, fill it with data and then play it

---

- this creates a buffer with 1 channel, `context.samplerate`(defaults to 44.1khz) samples per second and samples for two seconds of audio

```javascript
var buffer = context.createBuffer(
  1,
  context.sampleRate * 2,
  context.sampleRate
);
```

---

- to manipulate buffer samples, we have to obtain its `channelData`:

```javascript
var bufferData = buffer.getChannelData(0);
```

now we can fill the buffer with data:

```javascript
for (var i = 0; i < bufferData.length; i++) {
  bufferData[i] = Math.random() * 2 - 1;
}
```

---

- and finally we can play the buffer:

```javascript
var source = context.createBufferSource();
source.buffer = buffer;
source.connect(context.destination);
source.start(0);
```

<button id="playBuffer" onclick="playRandomBuffer()">play buffer</button>

<script src="intro.js"></script>

---

## Excercises

- can you create a buffer with a single channel and fill it with a sine wave?
  <button onclick="playSine(440)">play sine wave</button>
- can you create a buffer with a single channel and fill it with white noise, amplitude modulated with a sine wave of frequency 10Hz?
  <button onclick="playModulatedNoise()">play white noise</button>

---

## Percussive sounds

---

### A kick drum

- Deep, punchy sound.
- Fast drop in frequency and amplitude.

<canvas id="kickCanvas" width="600" height="200" style="border:1px solid #000;"></canvas>

<script>drawKick()</script>

<button class="button-play-kick" onclick="playKick()">Play Kick</button>

---

## Snare Drum

- characterized by a combination of a tonal body with a sharp, crisp noise (produced by the "snares" on the bottom of the drum).

Components:

- _Tonal Body_: A brief decaying sinusoidal wave to represent the body of the snare sound.
- _Noise Element_: An overlay of a "noisy" pattern to represent the snares or the rattling sound.

---

<canvas id="snareCanvas" width="600" height="200" style="background-color: black; border:1px solid #000;"></canvas>

<script>drawSnare()</script>

<button onclick="playSnare()">play snare</button>

---

## Hi-hat (open and closed)

- Closed Hi-Hat:

  - Produces a short, sharp "tick" sound.
  - Typically used to keep rhythm.

- Open Hi-Hat:

  - Produces a longer, sustained "sssh" sound.
  - It's more resonant and has a wash of overtones.

---

- For the closed hi-hat, we can use a very brief noise burst, representing the short, sharp sound.

<canvas id="closedHiHatCanvas" width="600" height="200" style="background-color: black; border:1px solid #000;"></canvas>

<script>drawClosedHiHat()</script>

<button class="button" onclick="playClosedHiHat()">Play Closed Hi-Hat</button>

---

- For the open hi-hat, we can use a longer and more spread out noise pattern, representing the sustained shimmering sound.

<canvas id="openHiHatCanvas" width="600" height="200" style="background-color: black; border:1px solid #000;"></canvas>

<script>drawOpenHiHat()</script>

<button onclick="playOpenHiHat()">Play Open Hi-Hat</button>
