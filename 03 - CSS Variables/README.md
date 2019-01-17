# TheName

## Requirements
* Update CSS variables with JS to change:
  * Spacing
  * Blur
  * Base color

Photo by [Photographer from Pexels](https://www.theurl.com/).

- - - -

## Exercise
### Initial setup
With CSS variables, if you change the value of the variable, the places that reference it will be updated on the page, Saas could not do that.
You declare CSS variables on an element, in this case on the `:root` (the highest level that you can get).
```css
  :root {
    --base: #ffc600;
    --spacing: 10px;
    --blur: 10px;
  }
```

You can start using these default values immediately by adding the styles to the image.
```css
  img {
    padding: var(--spacing);
    background: var(--base);
    filter: blur(var(--blur));
  }
```

And then the elements with the highlight class.
```css
  .hl {
    color: var(--base);
  }
```

### Use JavaScript to change the values
Select all the inputs on the page.
```javascript
const inputs = document.querySelectorAll('.controls input');
```

This returns a `NodeList`. It's not an array, so a lot of methods are not available. It is common to convert the NodeList to an array to use some methods, but we don't need to do that to loop through it, we can use the `forEach` method (beware of compatibility with older browsers, though).

#### Listen to events on the inputs
Add a function to handle the chenges. We'll just do a quick `console.log` for now.
```javascript
function handleUpdate() {
  console.log(this.value);
}
```

Then we listen for a change event on each of the inputs.
Here we want to listen for the `change` event and when that is called, we're going to call `handleUpdate`.
```javascript
inputs.forEach(input => input.addEventListener('change', handleUpdate));
```

Add another event listener for `mousemove`, so the values update as you drag the mouse on the slider, not just when you let go.
```javascript
inputs.forEach(input => input.addEventListener('change', handleUpdate));
inputs.forEach(input => input.addEventListener('mousemove', handleUpdate));
```

#### Update values
Now we go back to the handleUpdate function, to reflect the actual changes on the page.

We need to consider the suffix of our values, what type of measurement so to speak they're using, if any. This has already been integrated in our markup with the `data-sizing` attribute.

`dataset` is an object that will contain all the data attributes from that specific element, in this case it's just sizing. Add the empty string/nothing option so you don't append undefined by mistake.
```javascript
function handleUpdate() {
  const suffix = this.dataset.sizing || '';
}
```

Use console.log the suffix to check if it's working.
```javascript
function handleUpdate() {
  const suffix = this.dataset.sizing || '';
  console.log(suffix);
}
```

Now that you've checked it's working, it's time to update the actual variables.
To select a variable we need to select our entire document, our root, and we're going to set a property of base, spacing and blur. We used it in the names of the inputs
```javascript
function handleUpdate() {
  const suffix = this.dataset.sizing || '';
  console.log(this.name); //gives the names of the inputs
}
```

Usually you change the property like this:
```javascript
function handleUpdate() {
  const suffix = this.dataset.sizing || '';
  document.documentElement.style.setProperty('--base', '10px'); // or --spacing or --blur with their respective values
}
```

Since they're going to be variable, depending on the input updated, we insert the name dynamically with `this.name`.
```javascript
function handleUpdate() {
  const suffix = this.dataset.sizing || '';
  document.documentElement.style.setProperty(`--${this.name}`, this.value);
}
```

Don't forget to add the suffix.
```javascript
function handleUpdate() {
  const suffix = this.dataset.sizing || '';
  document.documentElement.style.setProperty(`--${this.name}`, this.value + suffix);
}
```

Note: You can also scope variables to use them on a specific element. The closer or more specific rule has more weight and ends up overriding the general CSS rules, just like normal CSS.
```html
<h2 style="--base: #BADA55">Update CSS Variables with <span class='hl'>JS</span></h2>
```