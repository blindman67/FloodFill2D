##FloodFill2D

FloodFill2D is a simple flood fill set of functions for the canvas 2D API.

It will fill a selected location with the current 2D context fillStyle and supports gradients, patterns, solid colours, current globalAlpha, and current globalCompositeOperation.

Can be used to cut or copy a selection from canvas, (like a magic selection tool). 

Can get the extent of a group of connected pixels.

Can return a mask representing the group of connected pixels.

Many floodFill functions have trouble dealing with anti aliased edges. This package lets you set a toleranceFade value that will create a soft edge at the boundaries. Though not a anti aliased fill it does produce very good fills for line drawings and high contrast images.


##It is easy to use.

There are no dependencies, written in ES5

###To install.

Just include the floodFill2D.js file on your page

```
   <script src = "floodFill2D.js"></script>
```

###To use in your application.

```
   // ctx is the canvas context2D
   //-----------------------------------------------------------------------------------------
   // Fill flat colour
   ctx.fillStyle = "red"; // colour to fill
   floodFill.fill(200, 200, 10, ctx); // fill pixels from position 200,200 with the colour red
                                 // with a tolerance of 10
                                 
                                 
   //-----------------------------------------------------------------------------------------
   // Fill with gradient
   var grad = ctx.createLinearGradient(0, 0, 0, ctx.canvas.height);
   grad.addColorStop(0,"red");
   grad.addColorStop(1,"yellow");
   ctx.fillStyle = grad;
   floodFill.fill(200, 200, 10, ctx); // fill pixels from position 200,200 with the current gradient
                                 // with a tolerance of 10


   //-----------------------------------------------------------------------------------------
   // Fill with alpha = 0.5
   ctx.fillStyle = "red"; // colour to fill
   ctx.globalAlpha = 0.5; // fill with half transparency
   floodFill.fill(200, 200, 10, ctx);   // fill pixels from position 200,200 with the colour red
                                        // with a tolerance of 10
                                 
                                 
   //-----------------------------------------------------------------------------------------
   // Remove pixels
   ctx.fillStyle = "black"; // any colour will do   
   ctx.globalAlpha = 1;   // ensure global alpha is 1
   ctx.globalCompositeOperation = "destination-out";
   floodFill.fill(200, 200, 10, ctx); // removes all filled pixels with a tolerance of 10
   
   
   //-----------------------------------------------------------------------------------------   
   // Fill all pixels with green that are within  range of alpha 255 - tolerance. All other colours are ignored.
   floodFill.setCompareValues(null,null,null,255); // ignore red, green, blue
   ctx.fillStyle = "green";
   floodFill.fill(200,200,200,ctx);   


   //-----------------------------------------------------------------------------------------   
   // Fill all colours except transparent black
   floodFill.setBoundingColor(0,0,0,0); // Fill up to all colours except transparent black
   ctx.fillStyle = "green";
   floodFill.fill(200,200,200,ctx);   
   // Note when alpha is set to 0 the other pixel colours are also set to 0

   
   //-----------------------------------------------------------------------------------------   
   // Fill all but red
   floodFill.setBoundingColor(255,0,0,255); // Fill all but Red
   ctx.fillStyle = "green";
   floodFill.fill(200,200,200,ctx);  

   
   //-----------------------------------------------------------------------------------------   
   // Cut connected pixels from canvas
   var image = floodFill.fill(200,200,200,ctx);  
   // Draw cut image
   ctx.drawImage(image,0,0);

   
   //-----------------------------------------------------------------------------------------   
   // Copy connected pixels from canvas
   var image = floodFill.fill(200,200,200,ctx);  
   // Draw copied image
   ctx.drawImage(image,0,0);

   
```

###floodFill functions description.

Once installed floodFill created an object named floodFill in the global scope. Flood fill has several functions related to filling pixels. (note [] around arguments indicate that they are optional and will revert to the default values if not supplied)

- **floodFill.fill(xPos, yPos, tolerance, context2D, [diagonal], [area], [toleranceFade])**
 Standard fill
- **floodFill.getMask(xPos, yPos, tolerance, context2D, [diagonal], [area], [toleranceFade])**
 Returns an image that has pixels to be filled. The original context2D is not filled, the mask pixels are set to set according to context2D current fill, alpha, and compositeOperation.
- **floodFill.getExtent(xPos, yPos, tolerance, context2D, [diagonal], [area], [toleranceFade])**
 Returns an object containing the extent of the fill. The extent object contains top,left,bottom,right,width,height. If no pixels are filled for what ever reason then null is returned.
- **floodFill.cut(xPos, yPos, tolerance, context2D, [diagonal], [area], [toleranceFade])**
 Returns an image that has pixels from context2D. The pixels are removed from context2D
- **floodFill.copy(xPos, yPos, tolerance, context2D, [diagonal], [area], [toleranceFade])**
 Returns an image that has pixels from context2D. The pixels are NOT removed from context2D
- **floodFill.setCompareValues(R,G,B,A)**
 Sets the colour to compare tolerance against in the next flood fill. This value is reset after each fill. To use the RGBA values agina you must call this function again.
 Any of the values can be set to null. If a channel is null it is ignored when testing tolerance during the fill operation. Note that is all values (R,G,B,A) are null then the function will ignore the call and the next fill will be done as normal.
- **floodFill.setBoundingColor(R,G,B,A)**
 Sets the colour that limits the bounds of the fill. If you wish to fill all colours except transparent then `floodFill.setBoundingColor(0,0,0,0)` to fill up to the colour red then `floodFill.setBoundingColor(255,0,0,255)`
 Note that this is reset after any fill is called. To repeat the fill with the bounding colour you must set it again.
 

###Argument description
 
```
floodFill.fill(xPos, yPos, tolerance, context2D, [diagonal], [area], [toleranceFade]);
```

- **xPos** : X position relative to left of canvas
- **yPos** : Y position relative to top of canvas
- **tolerance** : The difference between the start pixel's @(xPos,yPos) colour and the colour not to fill. A value of 0 will only fill pixels that are the same colour as the start pixel. A value 255 will fill all pixels.
- **context2D** : The 2D context of the canvas to fill.
- **diagonal** : Optional boolean. Defaults false. If true will fill along diagonal pixels (pixels that touch at their corners)
- **area** Optional : object describing the area to fill. eg {x : 100, y : 100, w : 100, h : 100} will only fill pixels in an area from top left at 100,100 and a width 100 and height of 100 pixels. Each of the properties x,y,w,h are optional and if excluded will default to the top left and remaining width and height.
- **toleranceFade** : Optional number Uses the difference between the start colour and tolerance to apply alpha at edges of the fill. Can be used to reduce the jaggedness at edge of fill.

###Notes

The flood fill ignores the current transformation.

Flood fill is not intended for real-time rendering and has a large memory overhead.


   