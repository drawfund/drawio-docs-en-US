## Create and edit complex custom shapes

https://desk.draw.io/support/solutions/articles/16000052874-create-and-edit-complex-custom-shapes

draw.io has a large library of pre-built shapes, 

but it also lets you embed your own raster and SVG images in your diagrams. 

While this provides you with a lot of flexibility, it doesn’t let you style raster or SVG images, except to change the colours in embedded SVG images. 

Because SVGs and raster images aren’t native shapes, they don’t contain the necessary information about which shapes on which to draw shadows, apply line widths, and so on.

## Basic custom shapes

You can create your own custom shapes in diagrams.net by describing their geometry, connection points and styles in an XML format. The basic diagrams.net shapes use XML. Select *Arrange > Insert > Shape* from the diagrams.net menu to open the Edit Shape dialog where you can see the XML structure of the shape.

### **<shape>**

The outer element is <shape>, that has the following attributes:

- name - string, required. This name uniquely identifies the shape. Not currently used in diagrams.net.
- w,h - optional decimal view bounds. This defines your co-ordinate system for the graphics operations in the shape. The default is 100,100.
- aspect - optional string with the value of either "variable", the default, or "fixed". *Fixed* always renders the shape with the aspect ratio defined by the ratio w/h. *Variable* causes the ratio to match that of the geometry of the current vertex.
- strokewidth - optional string containing either an integer or the string "inherit". *Inherit* indicates that the *strokewidth* of the cell will change both when you scale and when you resize the shape. This numeric value defines the multiplier that is applied to the width. The default is "1".

```xml
<shape h="100" w="100" aspect="variable" strokewidth="inherit">
  <connections>
    <constraint x="0" y="0" perimeter="1" />
    <constraint x="0.5" y="0" perimeter="1" />
    <constraint x="1" y="0" perimeter="1" />
    <constraint x="0" y="0.5" perimeter="1" />
    <constraint x="1" y="0.5" perimeter="1" />
    <constraint x="0" y="1" perimeter="1" />
    <constraint x="0.5" y="1" perimeter="1" />
    <constraint x="1" y="1" perimeter="1" />
  </connections>
  <background>
    <rect x="0" y="0" w="100" h="100" />
  </background>
  <foreground>
    <fillstroke />
    <ellipse x="0" y="0" w="100" h="100" />
    <stroke />
  </foreground>
</shape>
```

### **<connections>**

If you want to define specific fixed connection points on your custom shape, use the <connections> element. Each <constraint> element within the connections element defines a fixed connection point on the shape. 



Constraints have the following attributes:

- perimeter - required, with the values 1 or 0. A value of 0 sets the connection point where specified by x,y. A value of 1 extrapolates the position of the connection point from the center of the shape, through x,y, to the point of intersection with the perimeter of the shape.
- x,y - the position of the fixed point relative to the bounds of the shape. These are automatically adjusted if perimeter=1. (0,0) is top left, (0.5,0.5) is the center, (1,0.5) is the center of the right hand edge of the bounds, etc. Use values less than 0 or greater than 1 to position the fixed point outside of the shape.
- name - optional string. A unique identifier for the port on the shape.

### <background> and <foreground>

The paths used to draw the shape are split into two elements, <foreground> and <background>. If a shadow is defined, this is derived from the <background> element. Generally, the *background* of the shape is the line that traces the outside of the shape, but this may not always be the case.



The <background> element can only contain one <path> <rect> <roundrect> or <ellipse> element (or none). It can not contain any *fill* *stroke* *image* *text* or *include-shape*.



Note that the state, styling and drawing in the mxGraph shapes used in diagrams.net is very close in design to that of HTML 5 canvas. Use these suggested [HTML 5 tutorials](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial) for a high-level introduction to the concepts used.

### **State**

*Foreground* and *background* elements are rendered following the concept of state. In addition to the state save/load operation, there are two other types of operations: style and draw. An applied style changes the current state.

- <save/> - saves the current state (current style).
- <restore/> - retrieves (loads) the last saved state from the state stack.

### **Drawing**

Most of the drawing (the lines inside the shape) is contained within a <path> element. The graphic primitives used by mxGraph in diagrams.net are very similar to that of HTML 5 Canvas.

- <move> - to attributes, required decimals (x,y).
- <line> - to attributes, required decimals (x,y).
- <quad> - to required decimals (x2,y2) via control point, required decimals (x1,y1).
- <curve> - to required decimals (x3,y3), via control points required decimals (x1,y1) and (x2,y2).
- <arc> - a copy of the SVG arc command and doesn't follow the HTML Canvas signatures. The [SVG specification documentation](http://www.w3.org/TR/SVG/paths.html#PathDataEllipticalArcCommands) gives the best description of its behavior. The attributes are named identically, all decimals, and all required.
- <close> - ends the current subpath and causes an automatic straight line to be drawn from the current point to the initial point of the current subpath.

### **Complex drawing**

In addition to the graphics primitive operations described above, there are non-primitive operations. Use these to more easily draw some of the basic shapes:

- <rect> - attributes "x", "y", "w", "h", all required decimals.
- <roundrect> - attributes "x","y", "w", "h", all required decimals. Also "arcsize" an optional decimal attribute defining how large the corner curves are.
- <ellipse> - attributes "x", "y", "w", "h", all required decimals.

Note that these 3 shapes and all *paths* must be followed by either a *fill*, *stroke*, or *fillstroke* to render them.

### **<text>**

*Text* elements have the following attributes:

- str - the text string to display, required.
- x and y - the decimal location (x,y) of the text element, required.
- align - the horizontal alignment of the text element, either "left", "center" or "right". Optional, default is "left".
- valign - the vertical alignment of the text element, either "top", "middle" or "bottom". Optional, default is "top".
- localized - 0 or 1. If 1 then the "str" actually contains a key used to fetch the value out of mxResources. Optional, default is 0, currently unused in diagrams.net.
- vertical - 0 or 1. If 1 then the label is rendered vertically (rotated by 90 degrees). Optional, default is 0.
- rotation - angle in degrees (0 to 360). The angle to rotate the text by. Optional, default is 0.
- align-shape - 0 or 1. If 0 then the rotation of the shape is ignored when setting the text rotation. Optional, default is 1.
- placeholders - 0 or 1. If 1 then placeholders of the form %name% will be replaced with their values. Optional, default is 0.

### **<image>**

*Image* elements can either be external URLs, or data URIs, [where supported](http://en.wikipedia.org/wiki/Data_URI_scheme) (not supported in IE 7-). Attributes are:

- src - required string. Either a data URI or URL.
- x,y - required decimals. The (x,y) position of the image.
- w,h - required decimals. The width and height of the image.
- flipH, flipV = optional 0 or 1. Used to flip the image along the horizontal/vertical axis. Default is 0 for both.