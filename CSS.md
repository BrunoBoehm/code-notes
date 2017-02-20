## Selectors
CSS syntax is `selector { property: value; }`.

You can do inline CSS, internal (embedded), or external CSS.

```html
<link rel="stylesheet" type="text/css" href="style.css">
```

Compound selectors enable to select multiple elements at once
```css
h1, h2, #box {
	font-family: Arial;
}
```
Descendant selector (space) selects all elements that are inside all descendants of the selector
```css
#nav li {...}
```

Child selectors (>) specifies to only select direct children (not grand children)
```css
#nav > li {...}
```

Sibling selectors (+) selects only the first identified element that appear directly after the matching selector
```css
h3 + p {...}
```

Preceded selector (~) selects all matched elements preceding after the selector. 
```css
.styleafter ~ h2 {...}
```

Universal selector  styles anything that has not been previously styled by anything else
```css
* {...}
```



Attribute selectors select elements with an attribute matching the value condition
```css
img[alt="cat"] {...}
```

```css
/* starts with, eg <a href="http://site.com">...</a> */
a[href^="http"] {
	color: red;
}

/* ends with, eg <p class="available dog">...</p> */
p[class$="dog"] {
	color: red; 
}

/* Anywhere within, eg <img src="heart/jpg" alt="I love you"> */
img[alt*="love"] {
	color: red;
}

/* Space separated, eg <p class="available dog shots">...</p> */
p[class~="dog"] {...}

/* Dash separated, eg <p class="available-dogs-shots">...</p> */
p[class|="dogs"] {...}
```

Pseudo class selector select eements based on a particular even or state
```css
a:link, a:visited, a:hover, a:active {...}

p:first-child, p:last-child {...}
```

## Authority and inheritance rules
The most specific selector wins: inline > id > class > element
When equal authority, the last style applies (lower in the code).

## Importing a web font
```css
@font-face {
	font-family: 'Skolar';
	src: url(../fonts.Skolar.webfont);
}
```

## Border size
By default the width and height of an element is calculated like
- actual width = width + padding + border
- actual height = height + padding + border

When you set the width/height of an element, the element often appear bigger than you have set (because the element's border and padding are added to the element's specified width/height).

The CSS3 box-sizing property allows us to include the padding and border in an element's total width and height.

If you set `box-sizing: border-box;` on an element padding and border are included in the width and height.

Applying this to all elements is safe and wise:
```css
* {
	-webkit-box-sizing: border-box;  /* safari */
	-moz-box-sizing: border-box; 	 /* firefox */
	box-sizing: border-box;
}
```

## display types: inline, block, inline-block, table-cell
`display: ;` property can change the default

### display: inline
`img`, `span`, `a` display inline by default
- appear side by side 
- doesn't accept width
- doesn't accept margin-top & margin-bottom

### display: block
`h`, `p`, `div` display block by default.
- take the full width of the page
- allow width to be specified
- allow margin-top & margin-bottom, that collapse

### display: inline-block
- appear side by side 
- allow width to be specified
- allow margin-top & margin-bottom

### display: table-cell
- no margin on the side
- allow width to be specified

Inline-block could be the way to go for creating a column structure on a page, but the blocks are not aligned vertically.
The best practice is to use float and display:block.

## Float
Every time you  float, the elements below/after will try to pull up.
In situations we don't want, we can use the `clear: both;` property.

```css
img {
	display: block;
	float: left;
	margin: 0 20px 10px 0;
	/* display block enables to have margin-top and margin-bottom, as img displays inline by default and  */
}
```

### Clearfix
When you float all elements inside a parent element, the parent loses track of how tall it should be.

With `clearfix` we are able to solve the solution and make the parent as tall as necessary to contain its floating children. You apply it to the parent container element.

```css
.clearfix:after {
	content: ".";
	display: block;
	clear: both;
	visibility: hidden;
	height: 0;
	line-height: 0;
}
```

### Multiple columns
To make multiple columns inside of a wrapper, you use float with fluid % widths.

```css
.wrapper {
  width: 960px;
  margin: 0 auto;
}

.col-1 {
  float: left;
  width: 32%;
}

.col-2 {
  float: left;
  width: 66%;
}

[class*="col-"] {
  margin-left: 2%;
}

.first {
  margin-left: 0%;
}
```


## Positioning

### Centering
We use `margin: 0 auto;` to center a div horizontally on a page.

### Relative position
Using relative ("relative to where it was positioned") you are able to set the properties of `top`, `left`, `bottom` and `right`.

### Absolute position
You are able to set the properties of `top`, `left`, `bottom` and `right`. But here instead of a position relative to where it was positioned, it moves relatively to the parent container (if relative positioning of the parent) or the browser window.


### Fixed position
Similar to absolute, but the element will stay in place when the browser scrolls (like a sticky menu)

## Z-index
By default z-index is 0, the higher the number the closer to the viewer.


## Responsive
Twitter has a mobile website, but it takes a lot of maintenance.
- "Desktop down" is made to work on desktop first, and then you hide stuff. "graceful degradation" means removing features as it gets smaller.
- "Mobile up" is first designed for mobile. "Progressive enhancement" means adding features as you get more space, with more columns.

A good deciding factor is where your users will be more (desktop/mobile)

You set up break-points/ thresholds.

### CSS Media queries
```css
@media [not|only] type [and] (expr) [,] … {
    rules
}
```

Devices type we use often are:
- all
- print
- screen

Logical keywords
- and
- not
- , (or)
- only

```css
p { color: red; }

@media only screen and (min-width: 200px) and (max-width: 800px) {
	p { color: green; }
}
```

### Responsive media
Types, medias, layouts can be responsive and allo them to scale up or down and not overflow.

```css
/* containing parent element needs to be at a determined fluid width */
section {
	width: 33.333%;
}

img, table, form, input, video, audio {
	width: 100%;
	max-width: 100%;
}
```

### Responsive font type size
Best practice is to fix a `body { font-size: 100%; }` and then work in `em`. At each break-point you can change the `body` % to make all texts bigger or smaller.

```css
body { font-size: 100%; }
h1 { font-size: 2.5em; }
p { font-size: 1em; }
 
@media only screen and (max-width: 400px) {
  body { font-size: 130%; }  
} 
``

```css
article p {
  column-count: 1; /* This is the default so we don't really need to specify */
}
 
@media only screen and (min-width: 480px) {
  article p {
	  -moz-column-rule: 1px dotted #ccc;
	  -webkit-column-rule: 1px dotted #ccc;
	  column-rule: 1px dotted #ccc;

	  -moz-column-gap: 1.5em;
	  -webkit-column-gap: 1.5em;
	  column-gap: 1.5em;

	  -moz-column-count: 3;
	  -webkit-column-count: 3;
	  column-count: 3;    
  }
}
```
### Responsive layout
Example of a desktop down layout where we define desktop style first.
On a small display we destroy the column structure by having the columns be `width: 100%; float: none;`.

```css
.wrapper { 
  width: 960px; 
}
 
@media only screen and (max-width: 980px) {
  .wrapper { width: 90%; }
  .col-1 {
  	width: 100%;
  	margin: 0;
  	padding: 0;
  	float: none;
  }
}
```

### dealing with viewport
Prevents devices from zooming in or out and instead rely on the media query.

```css
<meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0">
```

## Twitter Bootstrap

### containers

To have a centered container
```css
<div class="container">
</div>
```

To have a container that fits the whole width of the page
```css
<div class="container-fluid">
</div>
```
### rows
Bootstrap uses row to create vertical separation, they stack below each other
```css
<div class="container">
	<div class="row">
	</div>
	<div class="row">
	</div>
	<div class="row">
	</div>
</div>
```

### columns
12 is the total number of colums.
The class name `col-`is followed by either `xs`, `sm`, `md`pr `lg`. These indicate the sizes: extra-small, small, medium or large, which effect the CSS media query break points. In other words the sizes define how many pixels the columns start to stack vertically instead of horizontally.
Unless you use `col-xs-...` the columns will stack vertically at some point.

You can use `-offset`class to shift columns over to the right.

You can change the conventional order of the columns by using push and pull classes. Push will push columns to the right, pull will pull columns from the right to the left.

Columns can be nested inside one another by inserting a row inside a column and then inserting additional columns inside of the inner row.

## CSS useful rules

### CSS transition
Experiment on [coderun](http://codepen.io/impressivewebs/pen/zqpEg)
```css
div {
	-webkit-transition: background-color 2s ease-out;
	-moz-transition: background-color 2s ease-out;
	-o-transition: background-color 2s ease-out;
	transition: background-color 2s ease-out;
} 

div:hover {
	background-color: red;
}
```

### Box shadow

```css
.shadow {
  box-shadow: 0 3px 10px #333;
}
```

### Menu
Inside of the `header` section 
```html
<header>
	<div id="navbar">
	  <div class="wrapper">
	    <nav>
	      <a href="index.html" class="selected">About</a>
	      <a href="new-properties.html">New Properties</a>
	      <a href="real-estate-listings.html">Listings</a>
	      <a href="market-report.html">Market Report</a>
	      <a href="contact.html">Contact</a>
	      <a href="http://hud.gov" target="_blank">H.U.D.</a>
	      <a class="menu-icon">&#9776;</a>
	    </nav>
	  </div><!-- .wrapper -->
	</div><!-- #navbar -->
</header>
```

In the CSS
```css
header {
  margin: 0 0 30px;
}

#navbar {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 2;
  width: 100%;
  border-bottom: 1px solid #ccc;
  text-align: center;
}

.wrapper {
  width: 960px;
  margin: 0 auto;
}

#navbar nav {
  border-left: 1px solid #ccc;
}

#navbar nav a {
  display: inline-block;
  width: 16.6666667%;
  margin: 0;
  padding: 22px 0;
  text-transform: uppercase;
  color: black;
  text-align: center;
  border-right: 1px solid #ccc;
  text-decoration: none;
  font: 1em "Trebuchet MS", Arial, sans-serif;
}

#navbar nav a.selected {
  background: #333;
  color: white;
}

#navbar nav a:hover {
  color: white;
  background: rgb(0,0,0); /* Old browsers */
  background: -moz-linear-gradient(top,  rgba(0,0,0,1) 0%, rgba(71,71,71,1) 28%, rgba(81,81,81,1) 35%, rgba(71,71,71,1) 72%, rgba(43,43,43,1) 87%, rgba(28,28,28,1) 91%, rgba(0,0,0,1) 100%); /* FF3.6+ */
  background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,rgba(0,0,0,1)), color-stop(28%,rgba(71,71,71,1)), color-stop(35%,rgba(81,81,81,1)), color-stop(72%,rgba(71,71,71,1)), color-stop(87%,rgba(43,43,43,1)), color-stop(91%,rgba(28,28,28,1)), color-stop(100%,rgba(0,0,0,1))); /* Chrome,Safari4+ */
  background: -webkit-linear-gradient(top,  rgba(0,0,0,1) 0%,rgba(71,71,71,1) 28%,rgba(81,81,81,1) 35%,rgba(71,71,71,1) 72%,rgba(43,43,43,1) 87%,rgba(28,28,28,1) 91%,rgba(0,0,0,1) 100%); /* Chrome10+,Safari5.1+ */
  background: -o-linear-gradient(top,  rgba(0,0,0,1) 0%,rgba(71,71,71,1) 28%,rgba(81,81,81,1) 35%,rgba(71,71,71,1) 72%,rgba(43,43,43,1) 87%,rgba(28,28,28,1) 91%,rgba(0,0,0,1) 100%); /* Opera 11.10+ */
  background: -ms-linear-gradient(top,  rgba(0,0,0,1) 0%,rgba(71,71,71,1) 28%,rgba(81,81,81,1) 35%,rgba(71,71,71,1) 72%,rgba(43,43,43,1) 87%,rgba(28,28,28,1) 91%,rgba(0,0,0,1) 100%); /* IE10+ */
  background: linear-gradient(to bottom,  rgba(0,0,0,1) 0%,rgba(71,71,71,1) 28%,rgba(81,81,81,1) 35%,rgba(71,71,71,1) 72%,rgba(43,43,43,1) 87%,rgba(28,28,28,1) 91%,rgba(0,0,0,1) 100%); /* W3C */
}
```

We can add a media query to make sure the menu will transform to a hamburger button when the viewer gets smaller, thanks to the `<a class="menu-icon">&#9776;</a>` element in the menu.
```css
@media only screen and (max-width: 700px) {
  #navbar nav a {
    display: none;
  }

  #navbar nav a.menu-icon {
    display: block;
    float: right;
    font-size: 1.5em;
    padding: 10px 20px;
    width: auto;
  }
}
```

# Flex



# Animations

## Slide-in effect for menus
```scss
$small: only screen and (min-width: 600px;);

.nav {
    margin-top: 1.75rem;
    
    .nav-elements {
        @media $small { 
            display: flex;
        }
        
        .branding {
            max-width: 180px;
            margin: 0 auto;
            
            @media $small {
                width: 30%;
                margin: 0;
            }
        }  /*.branding*/
        
        .navbar {
            margin: 0;
            padding: 0;
            margin-top: 2rem;
            line-height: 150%;
            list-style-type: none;
            width: 100%;
            
            @media $small {
                display: flex;
                justify-content: flex-end;
                align-content: flex-start;
                flex-wrap: wrap;
            }
        
            a {
                font-weight: $font-weight-bold;
                color: $color-background;
                text-decoration: none;
                padding: 0 1rem;
                
                &:hover, 
                &:focus {
                    color: $yellow;
                    transition: 1s ease-out;
                }
                
                &:after {
                    content: ' ';
                    display: block;
                    height: 5px;
                    width: 0;
                    margin 0 .5rem;
                    transition: width .3s ease-out, background-color .3s ease-out;
                    /*will be preprocessed by autoprefixer*/
                }
                
                &:hover:after {
                    width: 100%;
                    background: $yellow;
                }
            } /*a*/
        } /*.navbar*/
    } /*.nav-elements*/
} /*.nav*/
```

## Backimage mixin
This image would take the whole viewport height, with a nice gradient.
```scss
@define-mixin backImage
    $image,
    $height: 100vh,
    $grstart: rgba(0,0,0,.2),
    $grend: rgba(0,0,0,.5),
    $bgPos: center center {
        background: linear-gradient( to bottom, $grstart, $grend), url($image);
        background-repeat: no-repeat;
        background-position: $bgPos;
        background-size: cover;
        height: 100vh;
        max-height: $height;
    }
```

## Working with rems
Let's put the default text size to 10px using the 62.5% rule. The [rem](https://snook.ca/archives/html_and_css/font-size-with-rem) unit is relative to the root—or the html—element. That means that we can define a single font size on the html element and define all rem units to be a percentage of that. It is a better approach than using *em* because it is relative to the font-size of the immediate parent, which causes the compounding issue (a list within a list isn't 14px, it's 20px. Go another level deeper and it's 27px).
```css
html { font-size: 62.5%; } 
body { font-size: 14px; font-size: 1.4rem; } /* =14px */
h1   { font-size: 24px; font-size: 2.4rem; } /* =24px */
```
And voila, we now have consistent and predictable sizing in all browsers, and resizable text in the current versions of all major browsers.

We also set [border-box](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing) for the whole document (to calculate the dimensions of an element to include padding and border, but not margin - instead of only content)
```scss
html {
    font-size: 62.5%;
    box-sizing: border-box;
}

*, *:before, *:after {
    box-sizing: inherit;
}
```





