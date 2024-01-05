
So some of you may know how to do this, but i am guessing that many do not. So i will share with you what i know and try and make it simple. Also, its a good idea for me to put all this information in one place for quick reference later on.
  
So, you may have noticed some developers referencing "fnt" files in their scripts. These files are used to output a specific font with text to the screen, for instance on a pop up panel like I use. Its a little tricky however to wrap your head around these without a base on how to even make these files. I will explain how to make the files, and then how to use them.  
  
To create our fnt files first we need a program called bitmap Font Generator. Its free and can be [url="http://www.angelcode.com/products/bmfont/"]found here.[/url] Not sure if there is a MAC version, perhaps someone can link to that for us as well. Once installed open up the program and you will be presented with a window similar to the one below.  
  
The first step in making a font is to pick which font you would like to use. For instance, in this tutorial I will be using an Arial font, of 20 pixels size with a stretched height of 120% and Bold. It is important you know what you want before hand.  
  
Go to the Options menu, and open up Font Settings, or press the F key.  
You will see the following window pop up.  
  
Here Select your font from the list of fonts. Here I picked Arial.  
Pick your font size, here 20 px and click Match Char Height. Clicking this will make the font height be exactly 20 pixels. If you don't you may get undesired results.  
Select other settings as needed, as Bold or Italics and height.  
Select Font smoothing for nice looking fonts, and then Press OK.  
  
You will see your Font now in the bitmap font generator window.  
  
You will now need to select the characters you want to use. There is a Selected character indicator on the bottom left of the window which indicates how many you have selected. Clicking and dragging over the various character squares will highlight the characters. Generally I will only select the top 95 characters which will be good enough for most things.  
  
Next, Go to Options and open the Export Options menu or press T.  
This will open up the following window.  
  
There are a few important items to get right in this window so that the font will be able to be drawn by SASL.  
1) Set the Bit Depth to 32.  
2) Set the texture Width and Height to something acceptable. Larger font sizes will need a larger texture size to fit. Use the visualize window to see if your work fits.  
3) In the presets pull down select "White text with alpha"  
Next set the texture to PNG as these work best.  
  
Now you are ready to export your font to a fnt file. go to the options menu and select "Save bitmap font as.."  
Now select a name of your font. Generally I like to name them something I will remember, like Arial20.fnt  
  
When you save it you will get two files, one is the font file (fnt) and the other is your png file. Both of these are needed for SASL to recognize and load the font.  
  
Now that we have the font, i will show you how to use it.  
In your lua file you will need to first setup the script to load the font.  
  
the line will look something like this:  
```lua
local Arial20 = loadFont('Arial20.fnt')
```
It should go somewhere at the top of your script where you would assign other variables.  
  
Now we can use it in our Draw() function near the bottom of our script.  
  
```cpp
function draw()  
	drawAll(components)  
	drawText(Arial20, 30, 132, get(SomeDataref), 0.10,0.10,0.10)  
end  
```

The drawtext function is used to tell SASL to draw your don't on the screen. First it needs your font, which is Arial, then a location, in this case 20,132. The location is something you will have to play with in sim by reloading your script after each change. Then you tell is what to draw on the screen. It can be a dataref, or scring, or a variation of both or multiple of both.  
  
You could for instance have something like this.  

```cpp
get(FuelTotalLBSShow) .. " lbs (" .. math.ceil((get(dref_TotalFuel)/24600)*100) .. "%)"  
```

The syntax here is a Variable can be followed by a "String" with a .. in between them.  
  
The last three digits are the color as a ratio from 0 to 1 of red green and blue.  
  
I hope this has helped you understand something you had not before. Now go and make some awesome things!  
  
If any of you have different or better ways of doing this please let me/us know. I am sure there may be more then one way to do this. This just happens to be the way I do it.