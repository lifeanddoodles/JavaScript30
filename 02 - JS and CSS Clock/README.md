# 02 - JS and CSS Clock

## Requirements
* Add .playing class that triggers animation for the keys':
  * Size
  * Border
  * Shadow
* Play associated sound

- - - -

## Exercise
### Initial setup
Create function `setDate`. Test it works every second.
```javascript
function setDate() {
  console.log('Hi');
}
setInterval(setDate,1000);
```

Get the seconds with the `Date()` function.
```javascript
function setDate() {
  const now = new Date();
  const seconds = now.getSeconds();
  console.log(seconds);
}
setInterval(setDate,1000);
```

Turn amount into a degree, so it translates to the appropiate rotation amount. 0 seconds has to be 0 degrees, 60 seconds has to be 360 degrees.
We use a rule of 3 to do the conversion.
In the case of seconds we have to divide the seconds by 60 and then multiply it by 360, because that would be the 100%.
```javascript
function setDate() {
  const now = new Date();
  const seconds = now.getSeconds();
  secondsDegrees = ((seconds / 60) * 360);
  console.log(secondsDegrees);
}
setInterval(setDate,1000);
```

Apply styles to the actual element so it rotates the correct amount of degrees.
Offset the original 90deg position of the element by adding 90 to the secondsDegrees amount.
```javascript
const secondHand = document.querySelector('.second-hand');

function setDate() {
  const now = new Date();
  const seconds = now.getSeconds();
  secondsDegrees = ((seconds / 60) * 360) + 90;
  secondHand.style.transform = (`rotate(${secondsDegrees}deg)`);
}
setInterval(setDate,1000);
```


Add minutes.
```javascript
const secondHand = document.querySelector('.second-hand');
const minuteHand = document.querySelector('.minute-hand');
function setDate() {
  const now = new Date();
  const seconds = now.getSeconds();
  secondsDegrees = ((seconds / 60) * 360) + 90;
  secondHand.style.transform = (`rotate(${secondsDegrees}deg)`);

  const minutes = now.getMinutes();
  minutesDegrees = ((minutes / 60) * 360) + 90;
  minuteHand.style.transform = (`rotate(${minutesDegrees}deg)`);
}
setInterval(setDate,1000);
```

Add hours.
```javascript
const secondHand = document.querySelector('.second-hand');
const minuteHand = document.querySelector('.min-hand');
const hourHand = document.querySelector('.hour-hand');

function setDate() {
  const now = new Date();
  const seconds = now.getSeconds();
  secondsDegrees = ((seconds / 60) * 360) + 90;
  secondHand.style.transform = (`rotate(${secondsDegrees}deg)`);

  const minutes = now.getMinutes();
  minutesDegrees = ((minutes / 60) * 360) + 90;
  minuteHand.style.transform = (`rotate(${minutesDegrees}deg)`);

  const hours = now.getHours();
  hoursDegrees = ((hours / 12) * 360) + 90;
  hourHand.style.transform = (`rotate(${hoursDegrees}deg)`);
}
setInterval(setDate,1000);
```

Note: Once loop is completed transition goes backwards since the count goes back to 0 and the animation is based on it, so we need to find a solution for that, or just remove the CSS animation in the meantime.