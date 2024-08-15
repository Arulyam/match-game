# Match Game
## Contents
- Introduction
- Main Functions

## Introduction	
Coded out the game's functionality by completing a variety of functions. Each function built upon each other and was heavily commented to display functionality. 

## Main Functions
### 1. **`createNewCard()`**
This function creates and returns a new DOM element that represents a card. The structure of the card is as follows: 
```html 
<div class="card">
	<div class="card-down"></div>
	<div class="card-up"></div>
</div> 
```
At this point, the code isn't "attaching" this element to the DOM yet. In the `appendNewCard()` function, the included CSS will display this HTML structure as a card. 

**TEST:** `createNewCardTest()`
<hr>

### 2. **`appendNewCard(parentElement)`**

| Parameter | Type | Example Argument |
| ---- | ---- | ---- |
| `parentElement` | DOM Element | `document.getElementById("card-container")` |

This function calls `createNewCard()` and attaches the returned card to the DOM so it actually shows up on the page (without a picture for now). 

The card is attached as a "child" of the parameter `parentElement`.  Here is what `parentElement` looks like at the beginning of the function:
```html
<div id="card-container">
</div>
```
And here is what it looks like at the end:
```html
<div id="card-container">
	<div class="card">
		<div class="card-down"></div>
		<div class="card-up"></div>
	</div>
</div>
```

**TEST:** `appendNewCardTest()`
<hr>

### 3. **`shuffleCardImageClasses()`**
The `style.css` file has styles for six classes named `image-1` through `image-6`. When applied to a card, these classes make the card face show the corresponding image when flipped. 
	
This function returns a "shuffled" array with two of each of these class names as strings. In the next function, we'll combine this and `appendNewCard()` to create and assign random pictures to cards.

The "underscore.js" library is used to shuffle the array. The library is called underscore because it uses an `_` character as an object to contain helper methods. "underscore.js" is loaded in the HTML via a CDN link then opened up the documentation linked below to learn how to use the `_.shuffle()` method.

- [Underscore.js CDN Link](https://cdnjs.com/libraries/underscore.js/1.4.1) 
- [_.shuffle Documentation](https://www.tutorialspoint.com/underscorejs/underscorejs_shuffle.htm)

> **🗒 Note:** We ignore the "require" syntax shown in the documentation as this is for non-browser environments. The `_` variable was automatically available when we linked the CDN.

**TEST:** `shuffleCardImageClassesTest()`
<hr>

### 4. **`createCards(parentElement, shuffledImageClasses)`**

| Parameter | Type | Example Argument |
| ---- | ---- | ---- |
| `parentElement` | DOM Element | `document.getElementById("card-container")` |
| `shuffledImageClasses` | Array | `["image-5", "image-1", "image-3"...]` |

This function: 

1. Creates all twelve cards with random images and appends each one to the DOM.
2. Returns an array of custom card objects that we can use to organize our cards on the JavaScript side, separate from the HTML document / DOM. 

Each iteration created a card that was a child of `parentElement` (using `appendNewCard()`) and added an image class name to the element from the **already shuffled** `shuffledImageClasses` parameter.

Created a new variable to store an array of custom card objects. Then pushed a new object to the array each iteration of the loop. The custom card object had the following properties: 

- `index` — Which iteration of the loop this is.
- `element` — The DOM element for the card.
- `imageClass` — The string of the image class on the card. 

Finally, returned the array so we could use the data elsewhere in our program.  

**TEST:** `createCardsTest()`
<hr>

### 5. **`doCardsMatch(cardObject1, cardObject2)`**

| Parameter | Type | Example Argument |
| ---- | ---- | ---- |
| `cardObject1` | Object | `{index: 3, imageClass: "image-5", element: cardElement}` |
| `cardObject2` | Object | `{index: 7, imageClass: "image-1", element: cardElement}` |

Once we knew what our card objects looked like, wrote a function to determine if two cards match and used properties of our card objects from the `createCards()` function: `index`, `element`, and `imageClass`.

This function returns `true` when `cardObject1` and `cardObject2` have the same image class name and `false` otherwise. 

**TEST:** `doCardsMatchTest()`
<hr>

### 6. **`incrementCounter(counterName, parentElement)`**

| Parameter | Type | Example Argument |
| ---- | ---- | ---- |
| `counterName` | String | `"flips"` |
| `parentElement` | DOM Element | `document.getElementById("flip-count")` |

This function adds one to a counter being displayed on the webpage (either the number of cards flipped or the number of cards matched)—the trick is that we used an "object dictionary" to store the two counters (flips/matches).

Essentially, `counters["flips"]` stores the flip count and `counters["matches"]` stores the match count. The name `"flips"`, `"matches"`, or something else was supplied as an argument to the `counterName` parameter. The `parentElement` parameter is the DOM element that shows the counter in the HTML (e.g. `<span id="flip-count">`). The `innerText` of this element determines what value is displayed for the counter.

**TEST:** `incrementCounterTest()`
<hr>

### 7. **`onCardFlipped(newlyFlippedCard)`**

| Parameter | Type | Example Argument |
| ---- | ---- | ---- |
| `newlyFlippedCard` | Object | `{index: 9, imageClass: "image-1", element: cardElement}` |

This function is invoked each time the user flips a card. It has one parameter: `newlyFlippedCard` which is the custom card object associated with whichever card has just been flipped. This function handles the majority of our game's logic: 

- Is this the first or second card flipped in a sequence?
- Do the cards match? 
- Is the game over?

First off, used the `incrementCounter` function to add one to the flip counter's UI. 

Once the flip counter was incremented, checked if this is the first card that had been flipped (if `lastCardFlipped` was null). If this was the first card flipped, reassigned the value of `lastCardFlipped` to the card object that was just flipped and exited the function with a `return`. 

If two cards were flipped but they *DON'T* match, removed the "flipped" class from each card's DOM element, set `lastCardFlipped` back to null, and returned.

If two cards were flipped and they matched, incremented the match counter and optionally added a "glow" effect to the matching cards. Played either the win audio or match audio based on whether the user had the number of matches needed to win or not. Both audio files were loaded in `provided.js` and were stored in variables were accessed with `matchAudio` and `winAudio`, respectively.

Finally, reset `lastCardFlipped` to null.

**No testing is provided for this function.**
<hr>

### 8. **`resetGame()`**
This function is called when the user clicks the Reset button at the bottom of the page. 

Started by removing all children from the card-container `<div>`. There are many ways to accomplish this as shown in the the Example section of the [MDN removeChild documentation](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild#examples). 

Next, reset the flip and match counts displayed in the HTML to zero. Then, reset the `counters` dictionary to an empty object.

Lastly, set `lastCardFlipped` back to `null` and used the included `setUpGame()` function to set up a new game.

**No testing is provided for this function.**
<hr/>

### 9. **`setUpGame()`**
This function sets up the game by calling the `createCards()` function and adding an `onclick` to each new card created.

**No testing is provided for this function.**