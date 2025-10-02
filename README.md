# <img src="./docs/img/indom.svg" alt="InDom" width="147" height="57"> - 3.7KB modern JavaScript DOM library - powerful, easy and automates cleanup [![License MIT](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/constcallid/indom/LICENSE)

![quick taste example 1](./docs/img/readme-quick-taste-1b.jpg)![quick taste example 2](./docs/img/readme-quick-taste-2b.jpg)

- **Lightweight:** Only **3.7KB gzipped** – adds minimal overhead.

- **Complete Innovative DOM Solution:** Comprehensive API with single-instance objects per element – eliminating duplication and boosting performance for element selection, manipulation, traversal, event handling and more. 

- **Modern JavaScript:** Built with ES2022, empowers clean and maintainable code.

- **Powerful Cleanup:** State (event listeners, data, etc.) **automatically** removed when elements are destroyed. Leak-proof by design, no need for manual cleanup to avoid memory leaks. 

- **Stack Agnostic:** Set events with InDom, remove elements with any library (an older JS DOM library, a modern large JS framework, etc.) – cleanup still happens automatically. This allows gradual adoption of InDom at any pace.

- **Fast & Dependency-Free:** Optimized for performance with zero external dependencies.

- **Modern Browser Support:** Compatible with all modern browsers (see browser support for more).

- **Powerful Fields Handling:** Advanced form field handling with automatic type normalization and validation.

- **Multiple Formats:** Distributed in Plain JavaScript, ESM modules, and TypeScript formats.

- **TypeScript Ready:** Includes built-in types (ES2022 compatible) available in `/src`.

<br>

## Table of Contents

<hr>

### Getting Started

[Shortcuts](#shortcuts-for-convenience) | [getOne → **$1**](#indomgetoneselector-container) | [get → **$a**](#indomgetselector-container) | [getById → **$id**](#indomgetbyidid) | [new InDom → **$n**](#new-indomsource) | [onReady](#indomonreadyfn)

<hr>

### API Reference

[Shortcuts](#shortcuts) | [getOne → **$1**](#indomgetoneselector-container) | [get → **$a**](#indomgetselector-container) | [getById → **$id**](#indomgetbyidid) | [new InDom → **$n**](#new-indomsource) | [onReady](#indomonreadyfn)

[getValue](#getvaluecontainer) | [getValues → **$v**](#indom-getvalues-args) | [setValue](#setvalue-value-container)

[on (+ onClick , onEnter etc.)](#on-type-fn-opts) | [onRemove](#onremove-fn) | [off](#off-type-fn)

[getElement / el](#getelement-el) | [remove](#remove) | [is](#is-selector) | [getParent](#getparent-selector) | [getSelfOrParent](#getparent-selector) | [getNext](#getnext-selector) | [getPrev](#getprev-selector) | [append](#append-children) | [prepend](#prepend-children) | [after](#after-siblings) | [before](#before-siblings) | [setHtml](#setdata-key-value) | [getHtml](#getdata-key)

[setData](#setdata-key-value) | [getData](#getdata-key) | [hasData](#hasdata-key) | [removeData](#removedata-key) | [setAttr](#setattr-key-value) | [getAttr](#getattr-key) | [hasAttr](#hasattr-key) | [removeAttr](#removeattr-key)

[getBox](#getbox) | [getOuterBox](#getouterbox) | [getRelativeBox](#getrelativebox) | [addClass](#addclass-names) | [hasClass](#hasclass-name) | [removeClass](#removeclass-names)  | [setStyle](#setstyle-property-map-value) | [getStyle](#getstyle-properties)
<hr>

### Additional Topics

[Shortcuts](#shortcuts-for-convenience) | [getOne → **$1**](#indomgetoneselector-container) | [get → **$a**](#indomgetselector-container) | [getById → **$id**](#indomgetbyidid) | [new InDom → **$n**](#new-indomsource) | [onReady](#indom-onready-fn)

<hr>
	
<br>


## API


### Shortcuts
The convenience shortcuts ($1, $a, $id, $n, $v) are optional and can be renamed, scoped differently, or removed entirely based on your preferences.

[↑TOC](#table-of-contents)

### `InDom.getOne(selector, container?)`
Shortcut: **$1**

Queries the DOM using the CSS selector and returns an `InDom` object that contains the matching DOM element.
Returns `null` if no matching element is found.

**Parameters:**
- `selector` {string}: CSS selector string
- `container` {ParentNode|InDom} (optional): The container element or InDom object to search within. Defaults to `document`.

**Returns:** `InDom | null`

**Examples:**
```js
$1('.example>div').setStyle('color', 'blue');
/*
	If .example>div doesn't match any element, $1('.example>div') will return null.
	Attempting to call a method on null will result in a TypeError.
	If you want to avoid this error when the element is not found,
	use the optional chaining operator (?.) e.g.:
*/
$1('.example>div')?.setStyle('color', 'blue');

$1('.example>div').onClick(n => {
	//n here is the InDom object
	n.addClass('clicked').setStyle({ 'color': 'red', 'font-size': '120%' });
});

// Set style to the first 'span', of the first '.example>div'
$1('span', $1('.example>div')).setStyle('color', 'green');

//or:
const div = $1('.example>div');
$1('span', div).setStyle('color', 'green');
```

[↑TOC](#table-of-contents)

### `InDom.get(selector, container?)`
Shortcut: **$a**

Queries the DOM using the CSS selector and returns an `InDomArray` of `InDom` objects for each matching DOM element.
Returns an empty `InDomArray` if no matching elements are found.

**Parameters:**
- `selector` {string}: CSS selector string
- `container` {ParentNode|InDom} (optional): The container element or InDom object to search within. Defaults to `document`.

**Returns:** `InDomArray`

**Examples:**
```js
// Set style on every '.example'
$a('.example').setStyle('color', 'blue');

// Set click event on every '.example>span'
$a('.example>span').onClick(n => {
	n.setStyle('color','green');
});

const example1 = $1('.example');
//Set data 'init': 1 on direct children 'div' of the first '.example'
$a('>div', example1).setData('init', 1);

//InDomArray objects themselves don't have get* methods
//Get the left and top of each '.example>div' relative to viewport
$a('.example>div').each(n => {
	// .getBox() returns the bounding box
	const box = n.getBox();
	console.log(`left:${box.left} top:${box.top}`);
});
```

[↑TOC](#table-of-contents)

### `InDom.getById(id)`
Shortcut: **$id**

Fetches the element with the specified ID and returns an `InDom` object that contains it.
Returns `null` if no element with the given ID is found.

**Parameters:**
- `selector` {string} id - The ID of the element to fetch

**Returns:** `InDom | null`

**Examples:**
```js
// You could get the InDom object by its ID using the general selector method:
const example1 = $1("#test"); 

// But it's more efficient, especially in HTML documents with many DOM elements,
// to get it directly by ID:
const example2 = $id("test");

```

[↑TOC](#table-of-contents)

### `new InDom(source)`
Shortcut: **$n**

Creates a new `InDom` object from a given underlying DOM element or an HTML string representing a DOM element.

**Parameters:**
- `source` {Document|Element|string}: DOM Element or HTML string of a DOM element

**Returns:** `InDom` - New `InDom` object or an existing one (if one already exists for the given source element).

**Throws:**
- `TypeError`: If the source is not a valid DOM Element, the document or HTML string of one DOM Element

**Examples:**
```js
// Example 1
const img2 = $n(
	'<img src="example-star.png" alt="second star image example" width="50" height="50">');
$1('.img-example-2').append(img2);

// Example 2
const container = $id('img-container');
const btn = $1('>.btn', container);
/** @type {InDom} */
let img;

// Define the click handler for the button (no need for the InDom object 
// or the event arguments here).
// It either loads an image for the first time or toggles the image source 
// on subsequent clicks.
btn.onClick(() => {

	// Image has not been loaded/created yet
	if (!img) {
		// Check if an image load is already in progress to prevent duplicate requests.
		if (btn.getData('loading') === 1) {
			btn.setHtml('the image is loading...');
			// Exit the handler early as no further action is needed.
			return;
		}

		// Create a native HTML Image object.
		const imgEl = new Image();

		// Define the callback for when the image finishes loading successfully.
		imgEl.onload = () => {
			// Wrap the loaded native image element in an InDom object
			img = $n(imgEl);
			img.setAttr('alt', 'a star image example');
			// Append the InDom-wrapped image to the container element.
			container.append(img);
			btn.setData('loading', 0).setHtml('change img');
		};

		// Configure the image source and initial properties.
		imgEl.src = 'example-star.png';
		imgEl.width = 50;
		imgEl.height = 50;

		// Set a flag on the button to indicate that an image load is now in progress.
		btn.setData('loading', 1);
		// Exit the handler as the load process has started.
		// Note: A production implementation should also handle img.onerror etc.
		return;
	}

	// Image already exists, toggle its source
	// Check the current src to determine which image to switch to.
	if (img.getAttr('src') == "example-cloud.png") {
		// If it's currently showing the 'cloud' image, switch to the 'star' image.
		img.setAttr('src', 'example-star.png')
			.setAttr('alt', 'a star image example');
		// Exit after that
		return;
	}
	// It is a 'star', switch to the 'cloud' image.
	img.setAttr('src', 'example-cloud.png')
		.setAttr('alt', 'a cloud image example');
	return;
});


// Notice
const test1 = $n('<div><span>one single parent element</span></div>');
// will work , but: 
try {
	const test2 =
		$n('<div><span>div 1</span></div><div><span>div 2</span></div>');
	// will throw a TypeError because you can create one InDom object only for one element 
}
catch (e) {
	console.log(e);
}
// in case you need to insert multiple elements set the HTML of the parent element 
// or append / prepend HTML to it , and then get the InDom object you want: e.g. 
$1('.example>div')
	.setHtml('<div><span>div 1</span></div><div><span>div 2</span></div>');
const test3 = $1('.example>div>div:nth-child(2)');
test3.setStyle('color', 'blue');


//InDom objects are created only once for the same DOM element
const a = $1('.example');
const b = $1('.example');
if (a === b) {
	console.log('it\'s the same object');
}
```

[↑TOC](#table-of-contents)

### `InDom.onReady(fn)`

Registers a function to run when DOM is ready (or immediately if already ready).

**Parameters:**
- `fn` {function}

**Throws:**
- `TypeError`: If `fn` handler is not a function 

**Examples:**
```js
// If the JavaScript file (containing InDom) is loaded and executed before the HTML DOM 
// content is fully parsed, attempting to select elements immediately might fail because 
// they don't exist yet. Additionally, adding event listeners to elements that haven't 
// been parsed yet will also fail. Use the InDom.onReady() function to ensure your code 
// runs only after the DOM is fully loaded and ready.

InDom.onReady(() => {
    // Safe to use InDom for quering DOM elements and attach event listeners here
    $1('.example').addClass('on');
});
```

[↑TOC](#table-of-contents)


### `.getValue(container?)`
**Available on:** `InDom`

Returns the current value of the element, normalized for its type.
- Single value inputs (`input`, `textarea`, etc.): string or `null`.
- `select` (single): string or `null`.
- `select` (multiple): array of selected values or empty array.
- `input[type=checkbox]` (same name group): array of checked values.
- `input[type=radio]` (same name group): string of the checked value or `null`.
- `input[type=file]` (single or multiple): `FileList` object (zero or more files).

**Parameters:**
- `container` {Document|Element|InDom} (optional) - Scope for checkbox and radio group lookups. When provided, only searches within this container for related elements. Defaults to `document`.

**Returns:**
- {string|string[]|FileList|null} string for single values, array for multiple/select, FileList for file inputs, `null` when no selection or the element lacks a `value` property

**Throws:**
- `Error`: If the underlying element has been removed  

**Examples:**
```html
<div class="input-examples">
	<div><input type="text" name="username" value=""></div>
</div>
```
<details><summary>view full HTML</summary>

```html
<div class="input-examples">
	<div><input type="text" name="username" value=""></div>
	<div><textarea name="message"></textarea></div>
	<div>
		<select name="color">
			<option value="red">Red</option>
			<option value="green" selected>Green</option>
			<option value="blue">Blue</option>
		</select>
	</div>
	<div>	
		<select name="size" multiple>
			<option value="s">Small</option>
			<option value="m">Medium</option>
			<option value="l">Large</option>
		</select>
	</div>
	<div>
		<input type="radio" name="payment" value="credit" id="credit">
		<label for="credit">Credit Card</label>
		<input type="radio" name="payment" value="debit" id="debit">
		<label for="debit">Debit Card</label>
		<input type="radio" name="payment" value="paypal" id="paypal">
		<label for="paypal">PayPal</label>
	</div>
	<div>
		<input type="checkbox" name="features" value="wifi" id="wifi">
		<label for="wifi">WiFi</label>
		<input type="checkbox" name="features" value="bluetooth" id="bluetooth">
		<label for="bluetooth">Bluetooth</label>		
		<input type="checkbox" name="features" value="gps" id="gps">
		<label for="gps">GPS</label>		
	</div>
	<div>
		<input type="file" name="documents" multiple accept=".pdf,.doc,.docx">
	</div>
</div>
```
</details>

```js
const container = $1('.input-examples');

// Iterate through each direct div child of the container.
$a('>div', container).each(div => {
    // Find the first child element within the current div.
    // Based on the HTML structure, this is the actual field input/textarea/select/etc.
    const field = $1('>*', div);

    // Create a button 
    const btn = $n("<span class='btn'>log value</span>");

    // Append the button to the current div so it sits next to the field.
    div.append(btn);

    // Attach a click event listener to the button.
    btn.onClick(() => {
        // When the button is clicked, log the field's name and its current value.
        console.log(`name: ${field.getAttr("name")} value:`);

        // Call the getValue() method on the field and log the result.
        // The output will vary based on the type of field and its current state.
        console.log(field.getValue());		
    });
    /*
    Expected outputs based on initial HTML state:
    - input "username" -> string 
    - textarea "message" -> string
    - select "color" -> string (selected color)
    - select "size" multiple -> array of strings (selected sizes), empty if none selected
    - radio "payment" -> string (selected payment), null if none selected
    - checkbox "features" -> array of strings (selected features), empty if none selected
    - file "documents" -> FileList object, empty if none selected with .length 0
    */	
});
```

[↑TOC](#table-of-contents)

### `InDom.getValues(...args?)`
Shortcut: **$v**

Returns a plain object with field names as keys and their [getValue()](#getvalue-container) results as values.
<div style="color:#091987;line-height:130%;font-size:112%;">
- one call → all document field's values as JS object.
- checkbox groups / multiple selects become arrays automatically
- duplicate names in different forms / sections → add a container argument
- dynamic fields (name_34, name_65) → auto-group under name:{'34':'Alice','65':'Bob'}
</div>
**Parameters:**
- `...args` {...string|string[]|InDom} (optional) - Field names (rest or array) or an InDom object as last arg to limit scope

**Returns:**
- {Object} map of field names to their current values  

**Throws:**
- `TypeError`: If a given field name is not a non-empty string 

**Examples:**
```js 
// every input/textarea/select field in document
let o = $v();
console.log(o);
//{"username":"Alice","message":"","color":"blue","size":["s","m"],"payment":null,
//"features":["wifi","gps"]...}

// every field inside first .input-examples
o = $v($1('.input-examples'));
console.log(o);
//{"username":"Alice","message":"","size":["s","m"],"payment":null,"features":[]...}

// only username + features (whole document)
o = $v('username','features');
console.log(o);
//{"username":"Alice","features":["wifi","gps"]}

// only username + features (inside first .input-examples)
o = $v('username','features',$1('.input-examples'));	
console.log(o);
//{"username":"Alice","features":[]}
// the same as $v(["username","features"],$1('.input-examples'));
```
```html
<div>
	<input type="text" name="name_34" value=""><input type="text" name="age_34" value="">
</div>
<div>
	<input type="text" name="name_65" value=""><input type="text" name="age_65" value="">
</div>
```
```js
// pick normal + grouped fields
o = $v('username','name_','age_');
console.log(o);
//{"username":"Alice","name":{"34":"Bob","65":"Carol"},"age":{"34":"28","65":"32"}}

// harvest all (default: group underscores)
o = $v();
console.log(o);
//{"username":"Alice","message":"",..."name":{"34":"Bob","65":"Carol"},
//"age":{"34":"28","65":"32"}}

// harvest all WITHOUT grouping
o = $v([]);
console.log(o);
//{"username":"Alice","message":"",..."name_34":"Bob","age_34":"28",
//"name_65":"Carol","age_65":"32"}
```
[↑TOC](#table-of-contents)

### `.on(type, fn?, opts?)`
**Available on:** `InDom`, `InDomArray`

<div style="color:#091987;line-height:130%;font-size:112%;">Registers an event listener that is automatically removed when the element is removed from the DOM (no matter how) , preventing memory leaks.</div>

**Parameters:**
- `type` {string} - Event type, e.g. 'click', 'keydown'
- `fn` {Function} (optional) - Event handler function; omit for mouse/click to trigger the event
- `opts` {AddEventListenerOptions} (optional) - Event options (once, passive, etc.)

**Returns:**
`{Function|Function[]}` - The internal handler(s) – pass to .off() to remove manually

**Throws:**
- `Error`: If the underlying element is not connected to DOM, or document not yet loaded 
- `Error`: If the underlying element(s) has been removed  
- `Error`: If auto-trigger is used with non-mouse event
- `TypeError`: If handler is not a function (when provided)

**Shorthand methods:**  
- `onClick(fn?, opts?)` → `.on('click', fn, opts)`  
- `onDoubleClick(fn?, opts?)` → `.on('dblclick', fn, opts)`  
- `onEnter(fn?, opts?)` → `.on('mouseenter', fn, opts)`  
- `onLeave(fn?, opts?)` → `.on('mouseleave', fn, opts)`  
- `onFocus(fn?, opts?)` → `.on('focus', fn, opts)`  
- `onBlur(fn?, opts?)` → `.on('blur', fn, opts)`  
- `onChange(fn?, opts?)` → `.on('change', fn, opts)`  
- `once(type, fn, opts?)` → `.on(type, fn, { ...opts, once: true })`

**Examples:**
```js
// log every keypress in username / message fields
$a('[name="username"], [name="message"]').on('keydown', (n, e) => {
	console.log(`name:${n.getAttr('name')} , key pressed:${e.key} 
		, current value:${n.getValue()}`);
	if (e.key === 's') {
		e.preventDefault(); // block 's' key
	}
});

// add / remove hover class on every .example>div
$a('.example>div').onEnter(n => n.addClass('on'));
$a(".example>div").onLeave(n => n.removeClass('on'));

// simple accordion: only one panel open at a time
const menu = $1('#menu');
const menuBtn = $1('>.btn', menu);
const search = $1('#search');
const searchBtn = $1('>.btn', search);

menuBtn.onClick(() => {
	if (menu.hasClass('on')) { menu.removeClass('on'); return; }
	if (search.hasClass('on')) { searchBtn.onClick(); } // close other
	menu.addClass('on');
});

searchBtn.onClick(() => {
	if (search.hasClass('on')) { search.removeClass('on'); return; }
	if (menu.hasClass('on')) { menuBtn.onClick(); } // close other
	search.addClass('on');
});

// clicking anywhere adds 'clicked' class to the clicked element
$n(document).onClick((_, e) => $n(e.target).addClass('clicked'));
```

[↑TOC](#table-of-contents)

### `.onRemove(fn)`
**Available on:** `InDom`, `InDomArray`

Registers a callback function that runs after the object's internal state (listeners, data) has been cleaned up, and just before its element is removed from the DOM.

**Parameters:**
- `fn` {Function} - The callback function

**Returns:**
`{Function|Function[]}` - The internal handler(s) – pass to .off('onRemove', …) to unregister

**Throws:**
- `TypeError`: If `fn` is not a function
- `Error`: If the underlying element(s) has been removed  

**Examples:**
```js
const example = $1('.example');

// callback fires no matter how the element is removed
example.onRemove(() => {
  console.log('removed:', example);
  alert('press OK → element disappears');
});

// Through InDom remove method on the object
example.remove();

// Through InDom setHtml on body 
$1('body').setHtml('empty');

// Through native DOM removal
document.querySelector('.example').remove();

// Through native innerHTML on its parent 
document.querySelector('.example').parentElement.innerHTML = 'empty';
```

[↑TOC](#table-of-contents)

### `.off(type?, fn?)`
**Available on:** `InDom`, `InDomArray`

Removes event listener(s) registered with `.on()` or its shorthand methods.

**Parameters:**  
- `type` {string} (optional) - Event type. If omitted, **all** listeners are removed. 
- `fn` {Function | Function[]} (optional) - Handler(s) returned by `.on()`. If omitted, **all** listeners of `type` are removed. 

**Returns:**  
{InDom|InDomArray} - `this` for chaining

**Throws:**  
- `TypeError`: If `type` is provided but is not a non-empty string.  
- `RangeError` for `InDomArray`: If `fn` array length does not match collection length.
- `Error`: If the underlying element has been removed  

**Examples:**
```js 
const divs = $a('.example>div');

divs.onClick(n => {
	console.log(n);
	/*
		this function is visible in DevTools: 
		#e (#events) / click / Set entry / [[TargetFunction]]
	*/
});

// a simple logger example 
divs.onEnter(n => console.log(`onEnter in:${n.getHtml()}`));

const addOnFns = divs.onEnter(n => n.addClass('on'));
const removeOnFns = divs.onLeave(n => n.removeClass('on'));

// remove addOnFns and removeOnFns but keep the first onEnter logger
divs.off('mouseenter',addOnFns).off('mouseleave',removeOnFns);

// remove every mouseenter handler (including the logger)
divs.off('mouseenter');

// remove every handler of every type (including onClick)
divs.off();
```

[↑TOC](#table-of-contents)

### `.getElement()` / `.el()`
**Available on:** `InDom`

Read-only reference to the underlying DOM element of the InDom object.

**Returns:**  
{Element|Document} - Element or the `document`

**Throws:**  
- `Error`: If the underlying element has been removed  

**Examples:**
```js
console.log($1('.example>div').el().scrollTop);
```

[↑TOC](#table-of-contents)

### `.remove()`
**Available on:** `InDom`, `InDomArray`

Cleans the internal state of the InDom object(s) and removes the underlying DOM element(s) from the document. 
This method is also triggered automatically, when the element is removed from the DOM by any other means.

**Throws:**  
- `Error`: If the underlying element (or an element in case of InDomArray) has already been removed  

**Examples:**
```js
// remove the first .example>div
$1(".example>div").remove(); 

// remove all .example>div
$a(".example>div").remove();
```

[↑TOC](#table-of-contents)

### `.is(selector)`
**Available on:** `InDom`

Checks if the underlying element matches a CSS selector

**Parameters:**  
- `selector` {string} - CSS selector to match against.

**Returns:**  
{boolean} -  True if matches , false if doesn't

**Throws:**  
- `Error`: If the underlying element has been removed  

**Examples:**
```js
const example = $1('.example>div');
console.log(example.is('div'));
//true 
console.log(example.is('.test'));
//false
```

[↑TOC](#table-of-contents)

### `.getParent(selector?)`
**Available on:** `InDom`

Returns the InDom object for the closest ancestor (or direct parent if no selector) that matches the selector.  
Returns `null` if nothing is found.

**Parameters:**  
- `selector` {string} (optional) - CSS selector to test against ancestors.

**Returns:** `InDom | null`

**Throws:**  
- `Error`: If the underlying element has been removed  

**Examples:**
```js
const span = $1('.example>div>span');

console.log(span.getParent().getHtml());
// <span>this is a first test</span>

console.log(span.getParent('.example').getHtml());
// <div> <span>this is a first test</span></div>...
```

[↑TOC](#table-of-contents)

### `.getSelfOrParent(selector)`
**Available on:** `InDom`

Returns `this` if its underlying element matches the selector, otherwise the InDom objet for its closest ancestor element that matches.  
Returns `null` if nothing is found.

**Parameters:**  
- `key` {selector} - CSS selector to test against `this` and ancestors.

**Returns:** `InDom | null`

**Throws:**  
- `Error`: If the underlying element has been removed  

**Examples:**
```js
// delegate clicks on all links (present or future)
$n(document).onClick((_, e) => {
	// _ instead of n because we only need the event object here (for IDEs)
	const link = $n(e.target).getSelfOrParent("a");
	if (link) {
		console.log(`link:${a.getAttr('href')} clicked`);
	}
});

// test link (works even if added later)
$1('body').append(`<a href="https://github.com/constcallid/indom" target="_blank">
	InDom - modern JavaScript DOM library</a>`);
```

[↑TOC](#table-of-contents)

### `.getNext(selector)`
**Available on:** `InDom`

Returns the InDom object for next sibling element (or the next sibling that matches the selector).
Returns `null` if nothing is found.

**Parameters:**  
- `selector` {string} (optional) - CSS selector to test against siblings.

**Returns:** `InDom | null`

**Throws:**  
- `Error`: If the underlying element has been removed  

**Examples:**
```html
<div class="sibling-example">
	<div class="a">.a test</div>
	<div>test</div>
	<div class="c">.c test</div>
</div>
```
```js
const span = $1('.sibling-example>.a');

console.log(span.getNext().getHtml());
// test

console.log(span.getNext('.c').getHtml());
// .c test
```

[↑TOC](#table-of-contents)

### `.getPrev(selector)`
**Available on:** `InDom`

Returns the InDom object for previous sibling element (or the previous sibling that matches the selector).
Returns `null` if nothing is found.

**Parameters:**  
- `selector` {string} (optional) - CSS selector to test against siblings.

**Returns:** `InDom | null`

**Throws:**  
- `Error`: If the underlying element has been removed  

**Examples:**
```js
const span = $1('.sibling-example>.c');

console.log(span.getPrev().getHtml());
// test

console.log(span.getPrev('.a').getHtml());
// .a test
```

[↑TOC](#table-of-contents)

### `.append(...children)`
**Available on:** `InDom`, `InDomArray`

Appends one or more HTML strings, DOM elements or InDom objects to the end of the underlying element(s).

**Parameters:**  
- `...children` {string|Element|InDom} - Content to append (variadic; single array is flattened)

**Returns:**  
{InDom|InDomArray} - `this` for chaining

**Throws:**  
- `Error`: If the underlying element(s) has been removed 

**Examples:**
```html
<ul class="example-1"><li>li 1</li><li>li 2</li></ul>
<div class="example-2"><span>span 1</span><div>div 1</div><div>div 2</div></div>
```
```js
//isolated steps
const ul = $1('ul.example-1');

// raw HTML string
ul.append('<div>test</div>');
console.log(ul.getHtml()); 
// <li>li 1</li><li>li 2</li><div>test</div>

// native DOM element
const img = new Image();
img.src = 'example-star.png';
img.width = img.height = 50;
ul.append(img);
console.log(ul.getHtml()); 
// <li>li 1</li><li>li 2</li><img …>

// InDom object
ul.append($n(img)); // same img, in InDom object
console.log(ul.getHtml()); // identical markup
// <li>li 1</li><li>li 2</li><img …>

// InDomArray (moved from .example-2)
const donor = $1('.example-2');
ul.append($a('>div', donor)); // moves both divs
console.log(ul.getHtml());
// <li>li 1</li><li>li 2</li><div>div 1</div><div>div 2</div>
console.log(donor.getHtml()); 
// <span>span 1</span> (divs gone)

// bulk append to every <li> of ul
$a('>li', ul).append('<span>test</span>');
console.log(ul.getHtml()); 
// <li>li 1<span>test</span></li><li>li 2<span>test</span></li>
```

[↑TOC](#table-of-contents)

### `.prepend(...children)`
**Available on:** `InDom`, `InDomArray`

Prepends one or more HTML strings, DOM elements or InDom objects to the end of the underlying element(s).

**Parameters:**  
- `...children` {string|Element|InDom} - Content to append (variadic; single array is flattened)

**Returns:**  
{InDom|InDomArray} - `this` for chaining

**Throws:**  
- `Error`: If the underlying element(s) has been removed 

**Examples:**
```js
// prepend examples (mirror of append examples)
const ul = $1('ul.example-1');

ul.prepend('<li>first</li>'); // string
ul.prepend(img); // Dom Element
ul.prepend($n(img)); // InDom object (same img) 
ul.prepend($a('>div', donor)); // InDomArray
$a('>li', ul).prepend('<span>test</span>'); // bulk prepend to every <li> of ul
```

[↑TOC](#table-of-contents)

### `.after(...siblings)`
**Available on:** `InDom`, `InDomArray`

Inserts one or more HTML strings, DOM elements or InDom objects after the underlying element(s).
When multiple items are provided they are inserted in reverse order so the first item appears first in the DOM.

**Parameters:**  
- `...siblings` {string|Element|InDom} - Content to append (variadic; single array is flattened)

**Returns:**  
{InDom|InDomArray} - `this` for chaining

**Throws:**  
- `Error`: If the underlying element(s) has been removed 

**Examples:**
```html
<ul class="example-1"><li>li 1</li><li>li 2</li></ul>
<div class="example-2"><span>span 1</span><div>div 1</div><div>div 2</div></div>
```
```js
//isolated steps
const ul = $1('ul.example-1');
const firtsLi = $1(">li",ul);

// raw HTML string
//firtsLi.after('<div>test</div>');
console.log(ul.getHtml());
// <li>li 1</li><div>test</div><li>li 2</li>

// native DOM element
const img = new Image();
img.src = 'example-star.png';
img.width = img.height = 50;
//firtsLi.after(img);
console.log(ul.getHtml());
//<li>li 1</li><img ...><li>li 2</li>

// InDom object
firtsLi.after($n(img)); // same img, in InDom object
console.log(ul.getHtml()); // identical markup
//<li>li 1</li><img ...><li>li 2</li>

// InDomArray (moved from .example-2)
const donor = $1('.example-2');
firtsLi.after($a('>div', donor)); // moves both divs
console.log(ul.getHtml());
//<li>li 1</li><div>div 1</div><div>div 2</div><li>li 2</li>
console.log(donor.getHtml());
// <span>span 1</span> (divs gone)

// bulk after to every <li> of ul
$a('>li', ul).after('<span>test</span>');
console.log(ul.getHtml());
//<li>li 1</li><span>test</span><li>li 2</li><span>test</span>
```

[↑TOC](#table-of-contents)

### `.before(...siblings)`
**Available on:** `InDom`, `InDomArray`

Inserts one or more HTML strings, DOM elements or InDom objects before the underlying element(s).

**Parameters:**  
- `...siblings` {string|Element|InDom} - Content to append (variadic; single array is flattened)

**Returns:**  
{InDom|InDomArray} - `this` for chaining

**Throws:**  
- `Error`: If the underlying element(s) has been removed 

**Examples:**
```js
// before examples (mirror of after examples)
const ul = $1('ul.example-1');
const firtsLi = $1(">li", ul);

firtsLi.before('<div>test</div>'); // raw HTML string
firtsLi.before(img); // native DOM element
firtsLi.before($n(img)); // same img, in InDom object
firtsLi.before($a('>div', donor)); // InDomArray (moves both divs)
$a('>li', ul).before('<span>test</span>'); // bulk before to every <li> of ul
```

[↑TOC](#table-of-contents)

### `.setHtml(content)`
**Available on:** `InDom`, `InDomArray`

Sets the innerHTML of the underlying element(s).

**Parameters:**  
- `content` {string} - Content to insert (coerced to string)

**Returns:**  
{InDom|InDomArray} - `this` for chaining

**Throws:**  
- `Error`: If the underlying element(s) has been removed 

**Examples:**
```js
const div1 = $1('.example>div');

//set onClick on the every span child of div1
$a('>span', div1).onClick(n => console.log('clicked', n));


//replace innerHTML → old spans gone, listener gone
div1.setHtml('<span>another test</span>');

// re-register on the new span(s):
$a('>span', div1).onClick(n => console.log('clicked', n));

// or:
div1.onClick((_, e) => {
	const span = $(e.target).getSelfOrParent('.example>div>span');
	if (span) {
		console.log('clicked', span);
	}
});
```

[↑TOC](#table-of-contents)

### `.getHtml()`
**Available on:** `InDom`

Returns the innerHTML of the underlying element.

**Returns:**  
{string} - Content

**Throws:**  
- `Error`: If the underlying element has been removed 

**Examples:**
```js
const html = $1(".example>div").getHtml(); 
console.log(html);
//<span>this is a test</span>
```

[↑TOC](#table-of-contents)

### `.setData(key, value)`
**Available on:** `InDom`, `InDomArray`

Stores a key/value pair in memory **or** updates the element’s `data-*` attribute (if it already exists) with the stringified value.

**Parameters:**  
- `key` {string} - Data key
- `value` {any} - Data value (will be coerced to string for `data-*` attributes)

**Returns:**  
{InDom|InDomArray} - `this` for chaining

**Throws:**  
- `Error`: If the underlying element has already been removed  

**Examples:**
```js
const div = $1('.example>div');

// click-counter stored in memory
div.onClick(() => {
	// read counter (default 0 if never stored)
	const clicks = div.getData('clicked') ?? 0;
	div.setData('clicked', clicks + 1);
});

// store object and int
div.setData('user', { id: 34, name: 'Bob' });
console.log(div.hasData('user') ? 'has data key: user' 
	: 'doesn\'t have data key: user');
//has data key: user

div.setData('test', 1);

console.log([div.getData('user'), div.getData('test')]); // [{id: 34, name: 'Bob'}, 1]

// remove only 'user'
div.removeData('user');
console.log([div.getData('user'), div.getData('test')]); // [undefined, 1]	


// grab every editable field inside the first .input-examples
const fields = $a('input, textarea, select', $1('.input-examples'));

// snapshot original values as JSON strings
fields.each(n => n.setData('originalValue', JSON.stringify(n.getValue())));

// button to check if anything changed
$1('body').append('<div>Any field modified?</div>');
$1('.checkBtn').onClick(() => {
	// InDomArray extends Array and inherits all standard array methods.
	// true if any field's current value differs from its snapshot (stops at first true)
	const modified = fields.some(n =>
		n.getData('originalValue') !== JSON.stringify(n.getValue())
	);
	console.log(modified ? 'modified' : 'same');
});

// The above is to demonstrate different concepts because with InDom you could just:
const section = $1('.input-examples');
$1('body').append('<div>Any field modified? (rw)</div>');
const original = JSON.stringify($v(section));
$1('.rwCheckBtn').onClick(() => console.log(
	original === JSON.stringify($v(section)) ? 'same' : 'modified'
));
```

[↑TOC](#table-of-contents)

### `.getData(key)`
**Available on:** `InDom`

Returns the `data-*` attribute value (as string) if it exists, otherwise the in-memory value.  
Returns `null` if the key is not found in either place.

**Parameters:**  
- `key` {any} - Key to look up (stringified for `data-*` attributes)

**Returns:**  
{any} - The stored value, or `null` if not found

**Throws:**  
- `Error`: If the underlying element has been removed  

**Examples:**
see [setData()](#setdata-key-value)

[↑TOC](#table-of-contents)

### `.hasData(key)`
**Available on:** `InDom`

Returns `true` if the underlying element has a `data-*` attribute **or** in its internal memory map, otherwise `false`.

**Parameters:**  
- `key` {string} - Data key to test (automatically prefixed with `data-` for attribute check)

**Returns:**  
{boolean} - `true` when the key exists, `false` otherwise

**Throws:**  
- `Error`: If the underlying element has been removed

**Examples:**
see [setData()](#setdata-key-value)

[↑TOC](#table-of-contents)

### `.removeData(key)`
**Available on:** `InDom`, `InDomArray`

Removes the key from the `data-*` attribute **or** from internal memory map,  whichever contains it. 

**Parameters:**  
- `key` {any} - Key to remove (stringified for `data-*` attributes)

**Returns:**  
{InDom|InDomArray} - `this` for chaining

**Throws:**  
- `Error`: If the underlying element(s) has been removed  

**Examples:**
see [setData()](#setdata-key-value)

[↑TOC](#table-of-contents)

### `.setAttr(key, value)`
**Available on:** `InDom`, `InDomArray`

Sets an attribute value to the underlying element.

**Parameters:**  
- `key` {string} - Attribute key.
- `value` {any} - Attribute value (will be coerced to string for ). 

**Returns:**  
{InDom|InDomArray} - `this` for chaining

**Throws:**  
- `Error`: If the underlying element(s) has been removed 

**Examples:**
```js
const img = $n('<img src="example-star.png" width="50" height="50">');

// helper: return attr value if the attribute exists or 'no alt' if doesn't
const getImgAlt = () => img.hasAttr('alt') ? img.getAttr('alt') : 'no alt';

console.log(`img alt:${getImgAlt()}`); // no alt 

img.setAttr('alt','example image'); 
console.log(`img alt:${getImgAlt()}`); // example image

img.removeAttr('alt');
console.log(`img alt:${getImgAlt()}`); // no alt
```

[↑TOC](#table-of-contents)

### `.getAttr(key)`
**Available on:** `InDom`

Gets the value of an attribute of the underlyuing element.

**Parameters:**  
- `key` {string} - The attribute key.

**Returns:**  
{string|null} - The attribute , null if the attribute doesn't exists  

**Throws:**  
- `Error`: If the underlying element has been removed 

**Examples:**
see [setAttr()](#setattr-key-value)

[↑TOC](#table-of-contents)

### `.hasAttr(key)`
**Available on:** `InDom`

Checks if the underlying element has an attribute.

**Parameters:**  
- `key` {string} - The attribute key.

**Returns:**  
{boolean} - `true` if the attribute exists, `false` otherwise.

**Throws:**  
- `Error`: If the underlying element has been removed 

**Examples:**
see [setAttr()](#setattr-key-value)

[↑TOC](#table-of-contents)

### `.removeAttr(key)`
**Available on:** `InDom`, `InDomArray`

Removes an attribute from the underlying element(s). 

**Parameters:**  
- `key` {string} - Attribute key. 

**Returns:**  
{InDom|InDomArray} - `this` for chaining

**Throws:**  
- `Error`: If the underlying element(s) has been removed 

**Examples:**
see [setAttr()](#setattr-key-value)
[↑TOC](#table-of-contents)

[↑TOC](#table-of-contents)

### `.getBox()`
**Available on:** `InDom`

Returns a DOMRect with the underlying element’s viewport-relative bounding box.

**Returns:**  
{DOMRect} - Native object with left / x, top / y, width, height, right, bottom.

**Throws:**  
- `Error`: If the underlying element has been removed 

**Examples:**
```js
const div = $n('<div></div>');
div.setStyle({
	display: 'inline-block',
	position: 'fixed',
	top: '110px',
	left: '130px',
	width: '100px',
	height: '150px',
	backgroundColor: 'blue'
});

//console.log(div.getBox());
console.log(JSON.stringify(div.getBox()));
//DOMRect {"x":0,"y":0,"width":0,"height":0,"top":0,"right":0,"bottom":0,"left":0}
//because it is not yet connected to DOM 

$1('body').append(div);
console.log(div.getBox());
console.log(JSON.stringify(div.getBox()));
//DOMRect {"x":130,"y":110,"width":100,"height":150,"top":110,"right":230,
//"bottom":260,"left":130}
```

[↑TOC](#table-of-contents)

### `.getOuterBox()`
**Available on:** `InDom`

Returns a DOMRect that expands the underlying element’s bounding box by its margins.

**Returns:**  
{DOMRect} - Native object with left / x, top / y, width, height, right, bottom.

**Throws:**  
- `Error`: If the underlying element has been removed 

**Examples:**
```js
const div = $n('<div></div>');
div.setStyle({
	'display': 'inline-block',
	'position': 'fixed',
	'top': '110px',
	'left': '130px',
	'width': '100px',
	'height': '150px',
	'background-color': 'blue',
	'margin': '10px 20px'
});

$1('body').append(div);

console.log(div.getBox());
//DOMRect {"top":120,"left":150,"right":250,"bottom":270,"width":100,"height":150}

const outerBox = div.getOuterBox();
console.log(outerBox);
//DOMRect {"x":130,"y":110,"width":140,"height":170,"top":110,"right":270,
//"bottom":280,"left":130}

const div2 = $n('<div></div>');
div2.setStyle({
	'display': 'inline-block',
	'position': 'fixed',
	'top': outerBox.top + 'px',
	'left': outerBox.left + 'px',
	'width': outerBox.width + 'px',
	'height': outerBox.height + 'px',
	'background-color': 'red'
});
$1('body').append(div2);
// red div2 overlay: exactly covers the margin box of the div blue element
```

[↑TOC](#table-of-contents)

### `.getRelativeBox()`
**Available on:** `InDom`

Returns a DOMRect with coordinates relative to the underlying element’s offset parent(borders of the parent are excluded).

**Returns:**  
{DOMRect} - Native object with left / x, top / y, width, height, right, bottom.

**Throws:**  
- `Error`: If the underlying element has been removed 

**Examples:**
```js
const div = $n('<div></div>');
div.setStyle({
	display: 'inline-block',
	position: 'fixed',
	top: '110px',
	left: '130px',
	width: '100px',
	height: '150px',
	backgroundColor: 'blue'
});
$1('body').append(div);
console.log(JSON.stringify(div.getBox()));
//DOMRect {"x":130,"y":110,"width":100,"height":150,"top":110,"right":230,
//"bottom":260,"left":130}

const innerDiv = $n('<div></div>');
innerDiv.setStyle({
	display: 'inline-block',
	position: 'absolute',
	top: '10px',
	left: '30px',
	width: '30px',
	height: '50px',
	backgroundColor: 'red'
});
div.append(innerDiv);
console.log(JSON.stringify(innerDiv.getRelativeBox()));
//DOMRect {"x":30,"y":10,"width":30,"height":50,"top":10,"right":60,
//"bottom":60,"left":30}
// red innerDiv: positioned inside blue div; coords are relative to blue’s padding box
```

[↑TOC](#table-of-contents)

### `.addClass(...names)`
**Available on:** `InDom`, `InDomArray`

Adds one or more CSS classes to the underlying element(s).

**Parameters:**  
- `names` {...string} names - Class name(s) to add (variadic)

**Returns:**  
{InDom|InDomArray} - `this` for chaining

**Throws:**  
- `Error`: If the underlying element(s) has been removed 

**Examples:**
```js
// add 'clicked' class to any .example>div that gets clicked
const divs = $a('.example>div');
divs.onClick(n => n.addClass('clicked'));

// button: counts how many are currently clicked, then resets them
const sumResetClicked = $n('<div>Sum and reset clicked</div>');
$1('body').append(sumResetClicked);

sumResetClicked.onClick(() => {
  let clicked = 0;
  divs.each(n => {
    if (n.hasClass('clicked')) { // test state
      clicked++;
      n.removeClass('clicked'); // reset state
    }
  });
  console.log('clicked:' + clicked);
});
```

[↑TOC](#table-of-contents)

### `.hasClass(name)`
**Available on:** `InDom`

 Tests whether the underlying element has the given CSS class.

**Parameters:**  
- `name` {string} - Class name to test.

**Returns:**  
{boolean} - `true` if the class is present, `false` otherwise.

**Throws:**  
- `Error`: If the underlying element has been removed 

**Examples:**
see [addClass()](#addclass-names)

[↑TOC](#table-of-contents)

### `.removeClass(...names)`
**Available on:** `InDom`, `InDomArray`

Removes one or more CSS classes from the underlying element(s).

**Parameters:**  
- `names` {...string} names - Class name(s) to remove (variadic)

**Returns:**  
{InDom|InDomArray} - `this` for chaining

**Throws:**  
- `Error`: If the underlying element(s) has been removed 

**Examples:**
see [addClass()](#addclass-names)

[↑TOC](#table-of-contents)

### `.setStyle(property | map, value?)`
**Available on:** `InDom`, `InDomArray`

Sets one or more CSS properties to the underlying element(s). Dash-case names are auto-converted to camel-case.

**Parameters:**  
- `property` {string} - Property name (dash-case or camel-case)  
- `value` {string} - Value to assign (when first arg is a string)  
- `map` {Object} - Object map `{ prop: value, ... }` for bulk assignment  

**Returns:**  
{InDom|InDomArray} - `this` for chaining  

**Throws:**  
- `Error` - If the underlying element(s) has been removed  
- `TypeError` - If a single argument is not an object 

**Examples:**
```js
const div = $n('<div></div>');
$1('body').append(div);

// single property
div.setStyle('background-color', 'blue');
console.log(div.getStyle('background-color'));
// rgb(0, 0, 255)

// bulk assignment + multi-read
div.setStyle({ width: '100px', height: '50px', fontSize: '16px' });
console.log(div.getStyle('width', 'height', 'font-size'));
// {width: '100px', height: '50px', 'font-size': '16px'}

// apply to collection
$a('.example>div').setStyle({ color: 'blue', 'font-size': '15px' });

// inspect every computed property
const styles = $1('.example>div').getStyle();
console.log([styles.color, styles.fontSize]);
// ['rgb(0, 0, 255)', '15px']
```

[↑TOC](#table-of-contents)

### `.getStyle(...properties?)`
**Available on:** `InDom`

 Gets computed CSS value(s) for the underlying element.

**Parameters:**  
- `properties` {...string} [optional] - Zero or more CSS property names (dash-case).

**Returns:**  
{CSSStyleDeclaration|string|Object<string,string>} 
- When no arguments are supplied the full CSSStyleDeclaration is returned.
- When exactly one property is supplied its computed value is returned as a string.
- When two or more properties are supplied an object map { prop: value } is returned.

**Throws:**  
- `Error`: If the underlying element has been removed 

**Examples:**
see [setStyle()](#setstyle-property-map-value)

[↑TOC](#table-of-contents)


[↑TOC](#table-of-contents)


[↑TOC](#table-of-contents)


[↑TOC](#table-of-contents)


[↑TOC](#table-of-contents)


[↑TOC](#table-of-contents)


[↑TOC](#table-of-contents)



