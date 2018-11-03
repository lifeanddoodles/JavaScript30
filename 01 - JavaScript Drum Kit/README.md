# 01 - JavaScript Drum Kit

## Requirements
* Add .playing class that triggers animation for the keys':
  * Size
  * Border
  * Shadow
* Play associated sound

Photo by [Josh Sorenson from Pexels](https://www.pexels.com/photo/group-of-people-raise-their-hands-on-stadium-976866/).

- - - -

## Exercise
### Initial setup
Detect any `keydown` event with `addEventListener`.
```javascript
window.addEventListener('keydown', function(e){
  console.log(e);
});
```

We are interested in the `keyCode` info inside the object.
```javascript
window.addEventListener('keydown', function(e){
  console.log(e.keyCode);
});
```

### Add audio
Start filtering `addEventListener` for a single element, i.e. data-key with value 65.
Selector does not need to be a class, it can be an [attribute selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors). Similar syntax as CSS files.
```javascript
window.addEventListener('keydown', function(e){
  const audio = document.querySelector('audio[data-key="65"]');
  this.console.log(audio);
});
```

Change the `data-key` attribute to be a variable.
```javascript
window.addEventListener('keydown', function(e){
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  console.log(audio);
});
```
 
If there's no audio associated with the key being pressed, just `return` to stop the rest of the function from running.
```javascript
window.addEventListener('keydown', function(e){
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  console.log(audio);
  if(!audio) return;
});
```

Play it has audio associated with it, play it.
```javascript
window.addEventListener('keydown', function(e){
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  console.log(audio);
  if(!audio) return;
  audio.play();
});
```

Adjust playback settings from audio file, so it plays from the start, even if it was already playing.
```javascript
window.addEventListener('keydown', function(e){
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  console.log(audio);
  if(!audio) return;
  audio.currentTime = 0;
  audio.play();
});
```

### Add animation
Select pressed key. You can test we are actually selecting the correct one with a `console.log`.
```javascript
window.addEventListener('keydown', function(e){
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  if(!audio) return;
  audio.currentTime = 0;
  audio.play();
  console.log(key);
});
```

Add `playing` class to animate the key.
```javascript
window.addEventListener('keydown', function(e){
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  if(!audio) return;
  audio.currentTime = 0;
  audio.play();
  key.classList.add('playing');
});
```

#### Remove playing class when finished
Get a *node list* of all the elements with a `key` class.
Remove class once key is played with a listener for a `transitionend` event.
We have to use the forEach because you cannot 'listen' to a node list directly, you have to explicitly [loop through the items](https://developer.mozilla.org/en-US/docs/Web/API/NodeList).
```javascript
window.addEventListener('keydown', function(e){
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  if(!audio) return;
  audio.currentTime = 0;
  audio.play();
  key.classList.add('playing');
});

const keys = document.querySelectorAll('.key');
keys.forEach(key => key.addEventListener('transitionend', removeTransition));
```

Create the `removeTransition` function.
Test all the properties that have been transitioned.
```javascript
window.addEventListener('keydown', function(e){
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  if(!audio) return;
  audio.currentTime = 0;
  audio.play();
  key.classList.add('playing');
});

function removeTransition(e) {
  console.log(e.propertyName);
}

const keys = document.querySelectorAll('.key');
keys.forEach(key => key.addEventListener('transitionend', removeTransition));
```

Filter so only the property `transform` removes its transition.
Remove the class `playing` from the selected item.
```javascript
window.addEventListener('keydown', function(e){
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  if(!audio) return;
  audio.currentTime = 0;
  audio.play();
  key.classList.add('playing');
});

function removeTransition(e) {
  if (e.propertyName !== 'transform') return;
  this.classList.remove('playing');
}

const keys = document.querySelectorAll('.key');
keys.forEach(key => key.addEventListener('transitionend', removeTransition));
```

### Bonus step
If you wish, remove the anonymous function in the event listener and make it a named function.
Place the event listener at the end.
```javascript
function playSound(e){
  const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
  const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
  if(!audio) return;
  audio.currentTime = 0;
  audio.play();
  key.classList.add('playing');
}

function removeTransition(e) {
  if (e.propertyName !== 'transform') return;
  this.classList.remove('playing');
}

const keys = document.querySelectorAll('.key');
keys.forEach(key => key.addEventListener('transitionend', removeTransition));

window.addEventListener('keydown', playSound);
```