---
title: Connecting Web Audio API To A Source File
updated: 2015-10-25
---

In attempting to use the Web Audio API to visualize an audio file, I found plenty of helpful documents, particularly on the MDN documentation. I highly recommend spending some time reading through the relevant documentation before starting.

The examples on the MDN site are really helpful in understanding inputs, outputs, and purpose of each method, but I had trouble linking everything together. So here’s an example of visualizing a local sound file to a canvas element on a blank HTML page:

**First, let’s create an html audio tag:**

```
<audio controls>
  <source src = "sound.mp3" type = "audio/mpeg">
</audio>
```

**Next**, create a audio context somewhere in the JS script. Use that audio context to create a media source node by passing in the audio element:

```javascript
var audioCtx = new window.AudioContext();
var audioElement = $('audio')[0];
var source = audioCtx.createMediaElementSource(audioElement);
```

Note here, that we need to access the zeroth element of  wrapped jQuery object.

**Next**, create an analyzer node, which can do all the cool stuff, then link it to the source node:

```javascript
var analyser = audioCtx.createAnalyser();
source.connect(analyser);
```

Now, almost everything is connected. When the source variable was created, the audio context assumes that that you’re interested in doing something with the source file, such as distorting the sound, adding reverbs, etc. As such, the audio context automatically forces the audio source to not be output to the speakers, unless otherwise instructed. To ensure that the audio file still plays, **connect the source to the audio context’s destination**, usually speakers:

```javascript
source.connect(audioCtx.destination);
```

Now everything is in place! Start getting data from the analyzer and plot them. For this example, plot the volume on the y-axis, and the frequency on the x-axis:

```javascript
var canvas = $('canvas')[0];
var canvasCtx = canvas.getContext('2d');

var draw = function() {
  var freqDomain = new Uint8Array(analyser.frequencyBinCount);
  analyser.getByteFrequencyData(freqDomain);
  canvasCtx.clearRect(0,0,WIDTH,HEIGHT);
  for (var i = 0; i < analyser.frequencyBinCount; i++) {
    var value = freqDomain[i];
    var percent = value / 256;
    var height = HEIGHT * percent;
    var offset = HEIGHT - height - 1;
    var barWidth = WIDTH/analyser.frequencyBinCount;
    var hue = i/analyser.frequencyBinCount * 360;
    canvasCtx.fillStyle = "rgb(200,0,0)";
    canvasCtx.fillRect(i * barWidth, offset, barWidth, height);
  }
  window.requestAnimationFrame(draw);
};
draw();
```

Hope that helps!

Check out the repo at [https://github.com/kweng2/WebAudioViz](https://github.com/kweng2/WebAudioViz)

It’s also hosted here: [http://kweng2.github.io/WebAudioViz/index.html](http://kweng2.github.io/WebAudioViz/index.html)
