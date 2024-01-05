
I've decided to write a tutorial how to draw things in aircraft displays in 3D cockpits, since this is something that a lot of people are having trouble to find out how it works and it is not very clear from the existing documentation.

Before I start I would to say that:

- I'm not a programer, nor a SASL specialist. What I know comes from my experience using SASL.
- What I will describe below is my way, not an "official" one. If someone has a better way, please be free to jump in and post it. 
- This tutorial is not for one to start with. Read Dan's other tutorials to understand the basics.
- If you haven't downloaded already, get my Avanti ([http://forums.x-plane.org/index.php?app=downloads&showfile=18239](http://forums.x-plane.org/index.php?app=downloads&showfile=18239)), I use heavily the draw on many scripts. Many things below are from the Avanti so it is good to have it to follow along this post.

What I will describe here is how to draw text, but is about the same for drawing lines, images etc.

## In what space the text is going to be drawn?

The space to draw the text is the: **_YourAircraft/cockpit3d/-PANELS-/Panel_Airliner.png_** image. My way is to create an image 2048x2048 pixels and painted with green color and the areas that will be for the displays black. That way it is easier to map them on the 3d application. (In Avanti though it is transparent and black since I have removed the green layer at the end).

## How I can put the text inside a display space?

It is obvious that we need some coordinates for that! Planemaker is a good space to look...but there is a problem! Generally in X-Plane the coordinate count starts from the lower left corner (0,0) to the right and up. But if you go to planemaker and check it you'll see that starts at (0,-256)!!!! I really would like to have some explanation on this! So, when you get coordinates from Planemaker you have to add 256 to the height to get them right. Another problem with planemaker (and I said that's a good place to look!) is that you need to add an instrument to read the coords. I usually put a generic_trigger, sized to the smallest possible and then I move it to the corners of the displays' spaces to get the coords.

On the other hand you can use an image editing program (Photoshop, Gimp etc), but those usually the (0,0) point is at the top left, so you have to "translate" the height. If a corner is (at Photoshop) at (0, 500) then you must do this: 2048-500=1548. So the coords you need are (0, 1548).

## Next step...SASL. Now what?

In avionics.lua file the first lines should be these: 

```lua
--We declare here the image size we are using
size={2048,2048}  

--I don't know why, but without those lines you cannot get the coords to work right
panelWindth3d = 2048  
panelHeight3d = 2048  
```

Below in that file, where the components are declared we need this (declare a space for our PFD):

```lua
components{
	-- the lower/left corner of the display is at (5,300)
	PFD {position={5,300,400,640} }, 
}   -- width 400px and height 640px 
```

With that we've done with avionics.lua file (for that matter!).

## Ok...but...when are going to draw some staff?

Well, now!

In Custom Avionics folder, we create a PFD.lua file. As above, the components in avionics.lua are (mostly) the files in Custom Avionics. So if we have a component named "PFD" then SASL will look in Custom Avionics for a file named "PFD.lua". (Some might say that components can be other staff too, but what we look here is the relationship with files).

In PFD.lua we start the file with:

```lua
size={400,640} 
```

That's the size of our space in the image, declared in avionics.lua and now is our local coordinate system. That means that now we draw inside that component starting from (0,0) cords, although in the image we actually draw from (5,300). So, that 400x640 is our local (for PFD.lua file) space to draw things.

Let's say that we want to have at the display the altitude we are selecting to be hold by the autopilot. Next lines should be:

```lua
--We get the altitude to hold
defineProperty("alt_hold", globalPropertyf("sim/cockpit2/autopilot/altitude_dial_ft"))   
--We get the altiltude we are now
defineProperty("altitude", globalPropertyf("sim/cockpit2/gauges/indicators/altitude_ft_pilot")) 
--We get the brightness level so our text will be dimmed with the brightness switch [0] 
defineProperty("brightness", globalPropertyf("sim/cockpit2/electrical/instrument_brightness_ratio[0]")) 
```

Now we need some fonts! Check the "SASL Fonts Tutorial" from FlyingJackal for more info.
[[SASL Fonts]]

Let's say we have our fonts, an Arial20.fnt. This file with the Arial20_0.png should be in Custom Avionics folder.

local font1=loadFont('Arial20.fnt') -- from now on (in this script) we use the font1 to tell what font we want to use. Of course we can use multiple fonts and name them as we like 

Time to draw!

```lua
function draw() 

	drawAll(components)
	 
	drawText(font1, 200, 300, string.format("%d", get(alt_hold)), 1, 1, 1, brightness)

end 
```

This code will draw with Arial 20 font, at 200,300 coords of our local space the altitude to hold value, but in integer format (the "%d"), in white color (1,1,1) and bright as we have set the brightness switch [0].

If you like some more fancy staff let's do this: 

```lua
function draw()  
	drawAll(components)
	 
	local alt=get(altitude)
	
	-- If we are 500 ft below the target altitude of 500 ft above
	if alt <= get(alt_hold) - 500 or alt >= get(alt_hold) + 500 then  
		--Draw the text in red color since we are away from our target altitude
		drawText(font1, 200, 300, string.format("%d", get(alt_hold)), 1,0,0) 
	else
		--Draw the text in green color since we are capturing the target altitude
		drawText(font1, 200, 300, string.format("%d", get(alt_hold)), 0,1,0) 
	end
end
```

Nice, right?

That's the basics. The possibilities are endless. You can, for example make the text move around if in place of a coord (height) put a function and you can create something like a text displaying the VVI and going up and down with the needle!

Any comments, questions, additions are welcomed!