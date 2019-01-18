# 04 - Array Cardio Day 1

## Requirements
* Test the array mathods:
  * filter
  * map
  * reduce
  * sort

Photo by [Photographer from Pexels](https://www.theurl.com/).

- - - -

## Exercise
### Initial setup
#### Filter
With the filter method, you pass it a function that will loop over every item in our array and is going to give us the inventor depending on our criteria. There's no need to put an else, as anything not matching will be removed from our list.
Since we want to return a filtered list with the inventors born in the 1500s, our criteria would have to check that the value for year (of birth) of each inventor is within the range of 1500-1599.
```javascript
const fifteen = inventors.filter(function(inventor){
  if(inventor.year >= 1500 && inventor.year < 1600) {
    return true;
  }
});
```

If we want to see our data on a format that is easier to read, we can use `console.table`, instead of the normal console.log.
```javascript
console.table(fifteen);
```

If we wish to reduce the amound of code we can transform our function into an arrow function.
Since the if only returns true or false, we can adapt it to become in line, and remove the if altogether.
```javascript
const fifteen = inventors.filter(inventor => inventor.year >= 1500 && inventor.year < 1600);
```

#### Inventors' first and last names
map takes an array, it does something with it and then returns a new array of the same length.
```javascript
const fullNames = inventors.map(inventor => inventor.fisrt + ' ' + inventor.last);
console.log(fullNames);
```
You can use backticks and use template strings if you don't want to concatenate the spaces.
```javascript
const fullNames = inventors.map(inventor => `${inventor.fisrt} ${inventor.last}`);
console.log(fullNames);
```

#### Sort by birth date
With sort you compare two items at a time, once again you giv ethe criteria and on each comparison the item is given 1 or -1 to put them in order (bubbling).
In this case a and b are the people you're comparing.
```javascript
const ordered = inventors.sort(function(a, b) {
  if (a.year > b.year) {
    return 1;
  } else {
    return -1;
  }
});

console.table(ordered);
```

You can also reduce the code here by using a ternary operator.
```javascript
const ordered = inventors.sort((a, b) => a.year > b.year ? 1 : -1 );

console.table(ordered);
```

#### How many years did all the inventors live
reduce takes items in and builds something on each one and gives you one value at the end. It gives you a 'running total' or waht you've returned from this function last time. It's also going to give you the current item (the inventor).
```javascript
const totalYears = inventors.reduce((total, inventor) => {
  return total + (inventor.passed - inventor.year);
});

console.table(totalYears);
```

Now we need to fix the format, since the first time we ran this the total was undefined, so we need to specify the format by adding the initial value of 0.
```javascript
const totalYears = inventors.reduce((total, inventor) => {
  return total + (inventor.passed - inventor.year);
}, 0);

console.table(totalYears);
```

#### Sort by years lived
```javascript
const oldest = inventors.sort(function(a, b) {
  const lastGuy = a.passed - a.year;
  const nextGuy = b.passed - b.year;

  if(lastGuy > nextGuy) {
    return -1;
  } else {
    return 1;
  }
});
```

Once again we can clean it up a little.
```javascript
const oldest = inventors.sort(function(a, b) {
  const lastGuy = a.passed - a.year;
  const nextGuy = b.passed - b.year;

  return lastGuy > nextGuy ? -1 : 1;
});
console.table(oldest);
```

#### List of boulevards with 'de' somewhere in the name
We wil create the information by selecting the information in this [Wikipedia page](https://en.wikipedia.org/wiki/Category:Boulevards_in_Paris).
For that we need to look at the soruce code/markup to get the DOM elements of the pages to know what to select, in this case anchor tags inside the first element with the `.mw-category` class.
```javascript
const category = document.querySelector('.mw-category');
const links = category.querySelectorAll('a');
```

Then we have to convert the list of links to a list of names by removing anything that isn't the text inside the link.
```javascript
const de = links.map(link => link.textContent);
```

Note: The map methis is not available for Nodelists, so we need to convert links to an array first.
There are two ways to do this, first by using Array.from().
```javascript
const category = document.querySelector('.mw-category');
const links = Array.from(category.querySelectorAll('a'));
```

The second is by using square brackets to create the array and then using the spread operator to spread every item into the array.
A spread takes every item out of something initerable, in this case a NodeList, and put it into the containing array.
```javascript
const category = document.querySelector('.mw-category');
const links = [...category.querySelectorAll('a')];
```

Then we're going to filter that list of names.
```javascript
const de = links
            .map(link => link.textContent)
            .filter(streetName => streetName.includes('de'));
```

#### Sort alphabetically
Test with people.
```javascript
const alpha = people.sort(function(lastOne, nextOne) {
  console.log(lastOne);
});
```

We need to tweak the info a little bit and separate the first and last names, for that we'll use split. We'll take advantage of the format the data is in now, where last name and first name are separated by a comma and a space.
```javascript
const alpha = people.sort(function(lastOne, nextOne) {
  const parts = lastOne.split(', ');
  console.log(lastOne);
});
```

If you leave it as it is, this results in each item returning an array with the two 'parts'. But we don't want that, so we're going to destructure the result and put each part in its own variable.
```javascript
const alpha = people.sort(function(lastOne, nextOne) {
  const [last, first] = lastOne.split(', ');
  console.log(last, first);
});
```

Now we need to add the code to make the comparison for the sort.
```javascript
const alpha = people.sort(function(lastOne, nextOne) {
  const [aLast, aFirst] = lastOne.split(', ');
  const [bLast, bFirst] = nextOne.split(', ');
  return aLast > bLast ? -1 : 1;
});
console.log(alpha);
```

As always you can use this as an arrow function.
```javascript
const alpha = people.sort((lastOne, nextOne) => {
  const [aLast, aFirst] = lastOne.split(', ');
  const [bLast, bFirst] = nextOne.split(', ');
  return aLast > bLast ? -1 : 1;
});
console.log(alpha);
```

#### Sum of instances of different words
We'll use reduce, and as previously mentioned we need an initial value, for this example, it'll be an empty object.
```javascript
const transportation = data.reduce(function(obj, item) {
  console.log(item);
  return obj;
}, {});
```

Count the instances of that item. In case there's none yet you have to explicitly put the counter at 0 for that object, so it can add one.
```javascript
const transportation = data.reduce(function(obj, item) {
  if(!obj[item]) {
    obj[item] = 0;
  }
  obj[item]++;
  return obj;
}, {});
console.log(transportation);
```