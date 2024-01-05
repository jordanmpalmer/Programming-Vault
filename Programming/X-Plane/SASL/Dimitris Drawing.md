Hey Jordan
what is the size of your Panel.png ?
I sent the screenshots as well to do some explaining
![[SASL-Draw-1.png]]

![[SASL-Draw-2.png]]

so this is a small example the first one is the main.lua and the second one is the drawingExample.lua
what i do is the following:

First, i am letting SASL know that the Panel.png file that i will be drawing on will be 4096x4096

Also i am making sure that the panel drawing, rendering, interactivity and RenderTExtPixelAligned are activated, and that the 3D rendering is not activated, that is because I am drawing ONLY in the 2D environment, that is the "screens" of the plane and the Pop Ups that i might have

After that what i usually do is that i set some global variables for the project name and version so i can easily change them oNLY on 1 place so that i know what version of the package the users are using when sending the logs for the errors and stuff

After that, I am creating my render textures, that is pretty much textures that are being stored in the cache memory so that i can draw some stuff only 1 time and then be able to use them in multiple aspects of the projects if neccesary or applicable, for this example i have included the display render Texture and 1 symbology Render Texture

The numbers at the end is the size of the render texture

after that is the components list that i am letting SASL know that the drawingExample component has a specific position and size in the Panel.png file, that is the position x = 0, y = 0 and the sizes width = 1100 and height = 1100, the numbers are in the order i described it here (position = {x, y, width, height})

After that, moving to the drawingExample

I will start from the draw() function

What i do here is that on the first time the function is being run, i am drawing the symbology symbol, in this case a circle with a cross

SASL will use the OPENGL function to draw the circle and the 2 lines ONLY the first time this is being run, and because we are not making any changes to that texture, the texture will stay on the last drawn state.

After that, in the update function, i am drawing inside the display Texture the texture that we just created and drawn on(the symbology texture), in this way instead of having to draw the whole symbol, SASL i handling that as an image, i.e. just 2 triangles(in this case, it can differ) so you are using less resources this way.

And then at the end of the draw function we are drawing the display texture on the drawingExample component

have a look and let me know of any questions or any missunderstandings :)

i believe that doing it in C++ would be kind of simillar, i just have 0 clue on how to draw on C++ with OPENGL Shaders and do lines and rectangles and Triangles and all these things :P