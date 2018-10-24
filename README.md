**This repo is no longer under active development.  See fxToolTip.js**
# <a id="top">foxToolTip.js</a>

---
## Installation
``` html
npm install fox.tooltip.js

-or-

npm install -g fox.tooltip.js   //install globally
```

---
##Overview
foxToolTip.js is a small, simple library designed to show tooltips on hover over any DOM element.  It uses a combination of JavaScript and css styles to provide flexibility and ease of implementation.  It has no dependencies and should work on any modern browser (i.e., not Internet Explorer 8.0 and earlier).

foxToolTip.js will automatically size and position tootips [default behavior] to ensure they are displayed in the viewport. Tooltips can be positioned relative to their target DOM element [default behavior] or track the cursor.

Tooltips can be styled in a wide variety of ways, and can contain any valid html (text, tables, images, svg, etc.).

foxToolTip.js supports method chaining and has special code to detect and integrate with [d3.js](https://d3js.org/).

Find a simple demo foxToolTip.js at my [foxToolTip.js demo bl.ock](http://bl.ocks.org/MichaelRFox/8725b438cf26be9287ece9a7b90624d0/).

* [Usage](#usage)
* [How it Works](#how-it-works)
* [The foxToolTip object](#The-foxToolTip-object)
* [Working with D3.js](#working-with-D3)
* [The Tooltip Content](#tooltip-content)
* [License](#license)

---
## <a id="usage">Usage</a>
foxToolTip.js exposes two methods: create and remove.

### <a id ="foxToolTip.create">foxToolTip.create (elementId, content)</a>
* **elementId** (string) - the unique id of your target DOM element
* **content** (string) - any valid html
* **returns** - a [foxToolTip object](#The-foxToolTip-object)
* **description** - The create method is the only method you need to invoke to instantiate a tooltip with all of the predefined defaults.  It returns a [foxToolTip object](#The-foxToolTip-object) which you may save as a variable to dynamically alter its options, or you may use method chaining to set desired options at the time the tooltip is created.  If you attempt to create a tooltip on a target element that already has a tooltip associated with it, the .create() method will delete the old tooltip before returning the new [foxToolTip object](#The-foxToolTip-object).

### <a id ="foxToolTip.remove">foxToolTip.remove (elementId)</a> 
* **elementId** (string) - the unique id of the target DOM element whose tooltip you want to remove.
* **returns** - null
* **description** - removes all of the event listeners and the associated [foxToolTip object](#The-foxToolTip-object) object from the stack.  If this is the only [foxToolTip object](#The-foxToolTip-object) on the stack, the .remove() method will remove all of the [foxToolTip object](#The-foxToolTip-object) css rules from the stylesheet and set the library back to its un-instantiated state.  Generally, this method is not required, since foxToolTip.js automatically detects the removal of target elements, and performs corresponding tooltip removal automatically.

[:house: top](#top)

---
## <a id="how-it-works">How it Works</a>
When the [foxToolTip.create()](#foxToolTip.create) method is first called it appends three class styles to the document's stylesheet:

* .foxToolTip - contains all of the rules that style the tooltip
* .foxToolTip::after - contains all of the rules that style the tooltip arrow
* .foxToolTipTarget - contains one rule that styles the cursor of the target element

The [foxToolTip.create()](#foxToolTip.create) method also appends two div elements to the document body element:

* The first div element is created with the class name "foxToolTip" and is the container for all tooltips in the document
* The second div element is permanently hidden and is used internally to size objects

*note*:  The classes and div elements are created only once and shared by all of the tooltips

Lastly, the [foxToolTip.create()](#foxToolTip.create) method adds event listeners (mouseover, mouseout, and mousemove) for each [foxToolTip object](#The-foxToolTip-object) which call internal foxToolTip.js methods: mouseOver(), mouseOut(), and mouseMove() which act as dispatchers for showing, hiding, and moving tooltips.  The [foxToolTip.create()](#foxToolTip.create) method also adds an onresize event listener to the window element to keep track of the document viewport size.

Upon each call of the [foxToolTip.create()](#foxToolTip.create) method, a unique [foxToolTip object](#The-foxToolTip-object) is created which contains all of the options necesary to style, position, and hide the tooltip.  foxToolTip.js maintains an internal stack of all of the [foxToolTip objects](#The-foxToolTip-object).  When the cursor hovers over a target element, foxToolTip.js determines which [foxToolTip object](#The-foxToolTip-object) is associated with that element, and alters the .foxToolTip, .foxToolTip::after, and .foxToolTipTarget style rules according to the options set for that tooltip.  This allows for unique tooltips to be created, removed, and dynamically styled.  

[:house: top](#top)

---
## <a id ="The-foxToolTip-object">The foxToolTip object</a>
Upon creation, the [foxToolTip.create()](#foxToolTip.create) method returns a [foxToolTip object](#The-foxToolTip-object) which exposes methods you can use to change the default behavior.

* [.content](#.content)
* [.orientation](#.orientation)
* [.preferredOrientation](#.preferredOrientation)
* [.autoPosition](#.autoPosition)
* [.autoSize](#.autoSize)
* [.mousePoint](#.mousePoint)
* [.trackMouse](#.trackMouse)
* [.cursor](#.cursor)
* [.font](#.font)
* [.foregroundColor](#.foregroundColor)
* [.backgroundColor](#.backgroundColor)
* [.backgroundOpacity](#.backgroundOpacity)
* [.padding](#.padding)
* [.borderRadius](#.borderRadius)
* [.boxShadow](#.boxShadow)
* [.transitionVisible](#.transitionVisible)
* [.transitionHidden](#.transitionHidden)
* [.arrowSize](#.arrowSize)
* [.width](#.width)
* [.minWidth](#.minWidth)
* [.remove](#.remove)

[:house: top](#top)

### <a id=".content">.content (content)</a>
* **content** (string) - any valid html
* **default** - ""
* **returns** - if the content argument is passed, the .content() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .content() method is called with no arguments, the .content() method returns the current content string.
* **description** - although the content of the tooltip is set with the [foxToolTip.create()](#foxToolTip.create) method, you may use the .content() method on the [foxToolTip object](#The-foxToolTip-object) to dynamically alter the content of the tooltip at runtime.  The content argument can be any valid html (text, tables, images, svg, etc.).  See [Tooltip Content](#tooltip-content) for more detail.

``` javascript
var myTooltip = foxToolTip.create("myElement", "Loreiem Ipsum");
	.
	.
	.
myTooltip.content("Ut enim ad minim veniam");
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".orientation">.orientation (orientation)</a>
* **orientation** (string) - "right" | "left" | "top" | "bottom"
* **default** - ""
* **returns** - if the orientation argument is passed, the .orientation() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .orientation() method is called with no arguments, the .orientation() method returns the current orientation setting.
* **description** - You may set the location of the tooltip relative to the target element (or cursor if the [.mousePoint](#.mousePoint) method is called with true as the argument).  If the orientation is set, autopositioning is disabled (see the [.autoPosition](#.autoPosition) method).

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
  .orientation("left");
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".preferredOrientation">.preferredOrientation (orientation)</a>
* **orientation** (string)  - "right" | "left" | "top" | "bottom" | "none"
* **default** -  "right"
* **returns** - if the orientation argument is passed, the .preferredOrientation() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .preferredOrientationt() method is called with no arguments, the .preferredOrientation() method returns the current preferredOrientation setting.
* **description** - The .preferredOrientation() method is designed to work with autopositioning.  If there is sufficient space to display the tooltip at the preferred orientation, then regardless of which position is optimum, the tooltip will be displayed at the preferred orientation.  If there is insufficient room to display the tooltip at the preferred orientation, then autopositioning takes over and displays the tooltip at the optimum position.

``` javascript
var myTooltip = foxToolTip.create("myElement", "Loreiem Ipsum")
	.preferredOrientation("left");
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".autoPosition">.autoPosition (autoPostion)</a>
* **autoposition** (boolean) - true | false
* **default** - true
* **returns** - if the autoPosition argument is passed, the .autoPosition() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .autoPosition() method is called with no arguments, the .autoPosition() method returns the current autoPosition setting.
* **description** - If set to true, the .autoPosition() method enables autopositioning of the tooltip.  Autopositioning evaluates the available screen space on all four sides of the target element (or cursor if the [.mousePoint](#.mousePoint) method is called with true as the argument) and the height and width of the tooltip and positions the tooltip at the orientation with the most room.  If the preferred orientation is not set to "none", and that location has sufficient room to display the tooltip, then the preferred location is used.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.autoPosition(false);
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".autoSize">.autoSize (autoSize)</a>
* **autoSize** (boolean) - true | false
* **default** -  true
* **returns** - if the autoSize argument is passed, the .autoSize() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .autoSize() method is called with no arguments, the .autoSize() method returns the current autoSize setting.
* **description** - If set to true, the .autoSize() method enables autosizing of the tooltip.  Autosizing evaluates the content of the tooltip and attempts to scale the width and height of the tooltip to conform to the aspect ratio of the document viewport.  Autosizing to the document viewport aspect ratio is intended to present an appealing appearance and maximize the probability that the tooltip will be displayable on the screen.  If the content includes non-text elements like tables or images, it may be necessary to set the minimum width of the tooltip using the [.minWidth](#.minWidth) method.  *Note*: autosizing does not actually set the tooltip height.  It sets the width, so that when the tooltip div element auto-scales the height for overflow, the desired aspect ratio is achieved.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.autoSize(false);
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".mousePoint">.mousePoint(mousePoint)</a>
* **mousePoint** (boolean) - true | false
* **default** - false
* **returns** - if the mousePoint argument is passed, the .mousePoint() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .mousePoint() method is called with no arguments, the .mousePoint() method returns the current mousePoint setting.
* **description** - if set to true, the .mousePoint() method causes the tooltip to be displayed relative to the cursor position instead of the target element.  If set to false the tooltip will be displayed relative to the target element.  In this case, the tooltip will attempt to center relative to one of the four sides of the target element (depending on the autoposition setting).  If, due to document viewport limitations, the tooltip can't center itself, it will adjust the tooltip left, right, up, or down as appropriate, but offset the tooltip pointer to keep it centered on the element.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.mousePoint(true);
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".trackMouse">.trackMouse (trackMouse)</a>
* **trackMouse** (boolean) - true | false
* **default** - false
* **returns** - if the trackMouse argument is passed, the .trackMouse() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .trackMouse() method is called with no arguments, the .trackMouse() method returns the current trackMouse setting.
* **description** - if set to true, the .trackMouse() method causes the tooltip to follow the cursor as it moves over the target element. If autopositioning is set to true, the tooltip will continue to adjust it orientation to ensure that it can be displayed within the document viewport.  The tooltip will also continuously offset the tooltip pointer, as required, to keep it centered relative to the cursor. When true is passed to the .trackMouse() method, the mousePoint option to will automatically be set to true as well.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.trackMouse(true);
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".cursor">.cursor (cursor)</a>
* **cursor** (string) - "" | “alias” | “all-scroll” | “auto” | “cell” | “context-menu” | “col-resize” | “copy” | “crosshair” | “default” | “e-resize” | “ew-resize” | “help” | “move” | “n-resize” | “ne-resize” | “nesw-resize” | “ns-resize” | “nw-resize” | “nwse-resize” | “no-drop” | “none” | “not-allowed” | “pointer” | “progress” | “row-resize” | “s-resize” | “se-resize” | “sw-resize” | “text” | “URL” | “vertical-text” | “w-resize” | “wait” | “zoom-in” | “zoom-out” | “initial” | “inherit”
* **default** - "help"
* **returns** - if the cursor argument is passed, the .cursor() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .cursor() method is called with no arguments, the .cursor() method returns the current cursor setting.
* **description** - The .cursor() method allows programmtic control of the cursor appearance as the mouse hovers over the target element.  If an empty string ("") is passed to the .cursor() method, the cursor displayed while hovering over the target element defaults to whatever style (inline, css, etc.) was otherwise defined.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.cursor("pointer");
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".font">.font(family, size)</a>
* **family** (string) - any valid css font-family (e.g., "tahoma, sans-serif")
* **size** (string | number) - any valid css font-size (e.g., "1em" | "16px").  If a number is passed, it is assumed to be in pixels.
* defaults - "tahoma", 16
* **returns** - if the family and size arguments are passed, the .font() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .font() method is called with no arguments, the .font() method returns the current font setting as an object (e.g., {family:"tahoma, sans-serf", size:16).
* **description** - Setting the font family and size via the .font() method, sets the font-family and font-size options contained within the [foxToolTip object](#The-foxToolTip-object) which is injected into the .foxToolTip style's ruleset upon hover over the target element. These options set defaults for text within the tooltip, but can be customized through inline or css selector styles, contained in the content that is passed to the [foxToolTip.create()](#foxToolTipCreate) method.

In the following example the phrase, "Lorem ipsum" will be displayed with the H1 style, and the span element will be displayed with the .myClass style.  The remaining text will be displayed with the family and size set with the .font() method.

```html
<style>
	H1 {
		.font-size: 1.25em;
		.font-weight: bold;
	}
	.myClass {
		.font-style: italic;
	}
</style>
<body>
	<p id="myElement">This is the target element</p>
	<script type = "text/javascript" src="foxToolTip.js"></script>
	<script>
		var content = "<H1>Lorem ipsum</H1><p>Ut enim ad minim veniam, <span class="myClass">quis nostrud</span> exercitation</p>";
		var myTooltip = foxToolTip.create("myElement", content)
			.font("verdana, sans-serf", "12px")
	</script>
</body>
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".foregroundColor">.foregroundColor (color)</a>
* **color** (string) - any valid css color such as hex, rgb, or a named css color
* **default** - "beige"
* **returns** - if the color argument is passed, the .foregroundColor() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .foregroundColor() method is called with no arguments, the .foregroundColor() method returns the current foregroundColor setting.
* **description** - Setting the tooltip foregroundColor via the .foregroundColor() method, sets the foregroundColor option contained within the [foxToolTip object](#The-foxToolTip-object) which is injected into the .foxToolTip style's ruleset upon hover over the target element. The forgroundColor option sets defaults for text color within the tooltip, but can be customized through inline or css selector styles.

In the following example the phrase, "Lorem ipsum" will be displayed with the H1 style, and the span element will be displayed with the .myClass style.  The remaining text will be displayed with the foregroundColor set with the .foregroundColor() method.

```html
<style>
	H1 {
		.font-size: 1.25em;
		.font-weight: bold;
		.color: black;
	}
	.myClass {
		.font-style: italic;
		.color: black
	}
</style>
<body>
	<p id="myElement">This is the target element</p>
	<script type = "text/javascript" src="foxToolTip.js"></script>
	<script>
		var content = "<H1>Lorem ipsum</H1><p>Ut enim ad minim veniam, <span class="myClass">quis nostrud</span> exercitation</p>";
		var myTooltip = foxToolTip.create("myElement", content)
			.foregroundColor("lightBlue")
	</script>
</body>
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)
          
### <a id=".backgroundColor">.backgroundColor(color) </a>
* **color** (string) - any valid css color such as hex, rgb, or a named css color
* **default** - "midnightBlue"
* **returns** - if the color argument is passed, the .backgroundColor() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .backgroundColor() method is called with no arguments, the .backgroundColor() method returns the current backgroundColor setting.
* **description** - Setting the tooltip backgroundColor via the .backgroundColor() method, sets the backgroundColor option contained within the [foxToolTip object](#The-foxToolTip-object) which is injected into the .foxToolTip style's ruleset upon hover over the target element. The backgroundColor option sets the background color of the tooltip.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.backgroundColor("rgb(20, 40,128)");
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".backgroundOpacity">.backgroundOpacity(opacity)</a>
* **opacity** (number) - [0...1]
* **default** - 1
* **returns** - if the opacity argument is passed, the .backgroundOpacity() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .backgroundOpacity() method is called with no arguments, the .backgroundOpacity() method returns the current backgroundOpacity setting.
* **description** - Setting the tooltip backgroundOpacity via the .backgroundOpacity() method, sets the backgroundOpacity option contained within the [foxToolTip object](#The-foxToolTip-object) which is injected into the .foxToolTip style's ruleset upon hover over the target element. The backgroundOpacity option sets the background opacity of the tooltip.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.backgroundOpacity(0.5);
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".padding">.padding (padding)</a>
* **padding** (string) - a string representing 1 to 4 any valid css sizes (e.g., "1em" | "16px"). If no unit of measurement is given it is assumed to be in pixels.
* **default** - "5px 10px"
* **returns** - if the argument(s) are passed, the .padding() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .padding() method is called with no arguments, the .padding() method returns the current padding setting formatted as a single css shorthand string (e.g. "5px 10px").
* **description** - Setting the tooltip padding via the .padding() method, sets the padding option contained within the [foxToolTip object](#The-foxToolTip-object) which is injected into the .foxToolTip style's ruleset upon hover over the target element. The padding option establishes the padding for the tooltip.  If the padding argument contains only one measurement, uniform padding is applied to all sides of the tooltip.  If the padding argument contains two measurements, the first measure is applied to the top and bottom, and the second to the left and right.  If the padding argument contains three measurements, the first is applied to the top, the second to the left and right, and the third to the bottom.  If the padding argument contains four measurements, they are applied to the top, left, bottom, right respectively.  

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.padding("2em 5px 1em 5px");
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".borderRadius">.borderRadius (borderRadius)</a>
* **borderRadius** (string | number) - any valid css size (e.g., "1em" | "16px") except percentage.  If a number is passed, it is assumed to be in pixels.
* **default** - "12px"
* **returns** - if the borderRadius argument is passed, the .borderRadius() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .borderRadius() method is called with no arguments, the .borderRadius() method returns the current borderRadius setting.
* **description** - Setting the borderRadius via the .borderRadius() method, sets the borderRadius option contained within the [foxToolTip object](#The-foxToolTip-object) which is injected into the .foxToolTip style's ruleset upon hover over the target element. The borderRadius option sets a uniform borderRadius for the tooltip corners.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.borderRadius("1em");
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".boxShadow">.boxShadow (size, color, opacity)</a>
* **size** (string | number) - any valid css size (e.g., "1em" | "16px")  except percentage, or "none".  If a number is passed, it is assumed to be in pixels.  If "none" is passed as a single argument, no boxShadow is shown
* **color** (string) - any valid css color such as hex, rgb, or a named css color
* **opacity** (number) - [0...1]
* **defaults** - size: "8px", color: "rgb(0,0,0)", opacity: 0.5;
* **returns** - if arguments are passed, the .boxShadow() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .boxShadow() method is called with no arguments, the .boxShadow() method returns the current boxShadow setting as a css box-shadow shorthand string (e.g. "8px 8px 8px 0 rgba(0,0,0,0.5)").
* **description** - Setting the boxShadow size, color, and opacity via the .boxShadow() method, sets the boxShadow option contained within the [foxToolTip object](#The-foxToolTip-object) which is injected into the .foxToolTip style's ruleset upon hover over the target element. The boxShadow option sets a box shadow offsets to the lower right of the tooltip.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.boxShadow("1em", "#0000FF", 0.5);
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".transitionVisible">.transitionVisible (delay, duration)</a>
* **delay** (number) - any valid number in seconds
* **duration** (number) - any valid number in seconds
* **defaults** - delay: 0, duration: 0.4;
* **returns** - if arguments are passed, the .transitionVisible() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .transitionVisible() method is called with no arguments, the .transitionVisible() method returns the current transitionVisible setting as a css transition shorthand string (e.g. "opacity 0.4s 0s").
* **description** - Setting the transitionVisible delay and duration via the .transitionVisible() method, sets the transitionVisible option contained within the [foxToolTip object](#The-foxToolTip-object) which is injected into the .foxToolTip style's ruleset upon hover over the target element. The transitionVisible option sets the transition effect upon hover over the target element.

*note*:  Internet Explorer 9 does not support CSS transitions.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.transitionVisible(0, 0.6);
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".transitionHidden">.transitionHidden (delay, duration)</a>
* **delay** (number) - any valid number in seconds
* **duration** (number) - any valid number in seconds
* **defaults** - delay: 0, duration: 0.4;
* **returns** - if arguments are passed, the .transitionHidden() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .transitionHidden() method is called with no arguments, the .transitionHidden() method returns the current transitionHidden setting as a css transition shorthand string (e.g. "opacity 0.4s 0s").
* **description** - Setting the transitionHidden delay and duration via the .transitionHidden() method, sets the transitionHidden option contained within the [foxToolTip object](#The-foxToolTip-object) which is injected into the .foxToolTip style's ruleset upon target element mouseout . The transitionHidden option sets the transition effect upon target element mouseout.

*note*:  Internet Explorer 9 does not support CSS transitions.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.transitionHidden(0, 0.2);
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".arrowSize">.arrowSize (size)</a>
* **arrowSize** (string | number) - any valid css size (e.g., "1em" | "16px").  If a number is passed, it is assumed to be in pixels.
* **default** - "12px"
* **returns** - if the size argument is passed, the .arrowSize() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .arrowSize() method is called with no arguments, the .arrowSize() method returns the current arrowSize setting.
* **description** - Setting the arrowSize via the .arrowSize() method, sets the arrowSize option contained within the [foxToolTip object](#The-foxToolTip-object) which is injected into the .foxToolTip style's ruleset upon hover over the target element. The arrowSize option sets the size of the tooltip's arrow.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.arrowSize("1.2em");
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".width">.width (width)</a>
* **width** (string | number) - any valid css size (e.g., "1em" | "16px").  If a number is passed, it is assumed to be in pixels.
* **default** - "auto"
* **returns** - if the width argument is passed, the .width() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .width() method is called with no arguments, the .width() method returns the current width setting.
* **description** - Setting the width via the .width() method, sets the width option contained within the [foxToolTip object](#The-foxToolTip-object) which is injected into the .foxToolTip style's ruleset upon hover over the target element. The width option sets the width of the tooltip upon hover over the target element.  However if the [.autoSize()](#.autoSize) method is set to true, the width setting will be ignored.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.width("500px");
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".minWidth">.minWidth (minWidth)</a>
* **minWidth** (string | number) - any valid css size (e.g., "1em" | "16px").  If a number is passed, it is assumed to be in pixels.
* **default** - "80px"
* **returns** - if the minWidth argument is passed, the .minWidth() method returns the [foxToolTip object](#The-foxToolTip-object).  If the .minWidth() method is called with no arguments, the .minWidth() method returns the current minWidth setting.
* **description** - Setting the minWidth via the .minWidth() method, sets the minWidth option contained within the [foxToolTip object](#The-foxToolTip-object) which is injected into the .foxToolTip style's ruleset upon hover over the target element. The minWidth option sets the minimum width of the tooltip upon hover over the target element.  The .minWidth() method is useful to ensure that non-text content elements such as tables are correctly displayed in the tooltip when using autosizing.

``` javascript
foxToolTip.create("myElement", "Loreiem Ipsum")
	.minWidth("100px");
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

### <a id=".remove">.remove () </a>
* **arguments** - none
* **defaults** - none
* **returns** - null
* **description** - similar to the [foxToolTip.remove()](#foxToolTip.remove) method, the .remove() method of a [foxToolTip object](#The-foxToolTip-object) removes all of the event listeners and the associated [foxToolTip object](#The-foxToolTip-object) object from the stack.  If this is the only [foxToolTip object](#The-foxToolTip-object) on the stack, the .remove() method will remove all of the [foxToolTip object](#The-foxToolTip-object) css rules from the stylesheet and set the library back to its un-instantiated state.  Generally, this method is not required, since foxToolTip.js automatically detects the removal of target elements, and performs corresponsing tooltip removal automatically.

```javascript
var myToolTip = foxToolTip.create("myElement", "Loreiem Ipsum");
	.
	.
	.
myToolTip.remove();
```

[:arrow_up: The foxToolTip object](#The-foxToolTip-object)

----
## <a id="working-with-D3">Working with D3.js</a>

[d3.js](https://d3js.org/) has a few peculiarities (especially with respect to D3 selectors, that foxToolTip.js accommodates.  In typical D3 coding, one generally won't save a unique var for each tooltip, as D3's chaining methods are generally used to create svg and other objects. This is easily accommodated by using the .each() method to create tooltips.  Note that each element that will have a tooltip associated with it must have a unique id. The following code illustrates one convenient way to do this:

```javascript
var svg = d3.select("body").append("svg").attr("id", "svg")
	.attr("width", width + margin.right + margin.left)
	.attr("height", height + margin.top + margin.bottom);
	
var svgGroup = svg.append("g")
	.attr("transform", "translate(" + margin.left + "," + margin.top + ")");

var node = svgGroup.selectAll("g.node")
	.data(nodes);	

var nodeEnter = node.enter().append("g")
	.attr("class", "node");

nodeEnter.append("circle")
	.attr("r", 1e-6)
	.style("fill", "lightSteelBlue")
	.style("stroke", "lightSteelBlue")
	//This is where we assign a unique id
	.attr("id", function(d, i) { return "node" + i })
	//This is where we create a tooltip for each node
	.each (function (d) {	
			var content = "<p>" + d.toolTipText + "</p>";
			foxToolTip.create("node" + d.id, content)
				.backgroundColor("midnightBlue")
				.backgroundOpacity(0.9)
				.foregroundColor("linen")
				.transitionVisible(0.5, 0.4)
				.transitionHidden(0, 0.4)
				.padding("0px 10px")
				.borderRadius(16)
				.arrowSize(16);
	};
```

You can find a demo of integrating foxToolTip.js with D3.js at my [D3.js demo bl.ock](http://bl.ocks.org/MichaelRFox/59cdc1c3478fb0362448cf87fdab30d0/).

[:house: top](#top)

---
## <a id="tooltip-content">Tooltip Content</a>

Content for tooltips in foxToolTip.js can contain any valid html, not just text.  The content can be styled using css selector styles or in-lined within the html itself.  Here are a few examples:

### Tables

When using tables and autoSizing in foxToolTip.js, it may be necessary to set the minimum width using the [.minWidth()](#.minWidth) method on the [foxToolTip object](#The-foxToolTip-object).  

```javascript
var table = "<table id = 'table' width="100%"><tr><th>id</th><th>name</th><th>address</th></tr><tr><td>1</td><td>Bill Smith</td><td>17 Cherry Lane</td></tr><tr><td>2</td><td>Ed Walker</td><td>427 Oak Lane</td></tr></table>"
var content = "<p class='centeredHeading'>Address</p>" + table;
var myToolTip = foxToolTip.create("myElement", content)
	.minWidth("300px");
```

### Images
Embedding images is possible by using the image tag.  To support autosizing, you should include the height and width attributes in the image tag, and set the minimum width using the [.minWidth()](#.minWidth) method on the [foxToolTip object](#The-foxToolTip-object).

```javascript
var content = "<p class='centeredHeading'>Company Logo</p><img src=logo.png alt='logo' width='300px' height='300px'></img>";

foxToolTip.create("myElement", content)
	.minWidth("300px")
	.arrowSize("0.5em")
	.cursor("pointer")
	.padding("5px", "1em", "2em");
```

You may also embed inline svg if your browser supports it.  To support autosizing, it may be necessary to set the minimum width using the [.minWidth()](#.minWidth) method on the [foxToolTip object](#The-foxToolTip-object).

```javascript
var svg = "<svg width='250' height='250'><rect x='50' y='50' rx='20' ry='20' width='150' height='150' style='fill:red;stroke:black;stroke-width:5' /></svg>"

foxToolTip.create("myElement", svg)
	.minWidth(250)
	.mousePoint(true)
	.trackMouse(true);
```

[:house: top](#top)

# <a id='license'>License</a>

**The MIT License (MIT)**

Copyright (c) 2016 Michael R. Fox

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

[:house: top](#top)

