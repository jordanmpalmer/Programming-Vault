In the first tutorial, we got a brief overview of SASL, and attached to it was a tiny [url="http://forums.x-plane.org/index.php?app=core&module=attach&section=attach&attach_id=124145"]orientation demo file[/url], consisting of an RV3 fitted with a bare-bones SASL system that added only one feature: flaps automatically retract at 80 Kts IAS. In this tutorial, we'll dissect that demo file, and try to understand its components, make some changes to the parameters, and lay some fundamentals to build our knowledge on.  
  
Let's take a look at the components of the plane we fitted with a basic SASL function.   
  
## The SASL plug-in.
___________________  
Located in your plane's "Plugins" directory, the SASL folder contains the Mac, Windows, and Linux compiled binaries (mac.xpl, win.xpl, lin.xpl) which read in the lua scripts and pass the data on to X-Plane's plug-in system. The SASL folder also contains some sub-folders, which simply include some resources used by SASL. For instance, if you are working on a 2d panel with click zones, you'll find that a SASL-specific cursor shows up when you hover over mouse click regions. It may be a hand, or a double-right or double-left arrow. These assets (along with several other ones) are stored in the "Data>components" subfolder. If you want, you can change these assets, to suit your needs, but in general, you don't need to touch these files.  
  
## The "avionics.lua" file.
______________________  
This file contains several important bits of information. Let's take them in blocks:  
  
a. The first line will usually contain a line that indicates what panel size you're using. SASL wants to know the dimensions of the canvas, so that the assets that you end up drawing will have a frame of reference to be placed onto. Most likely, this line will be 
```lua
size = { 1024, 1024 }
```
or 
```lua
size = { 2048, 2048 } 
```
  
(Technically, for our demo it wasn't really necessary to include that first line, since we are not drawing any instruments on any 2D canvas.)  
  
b. Dataref and Custom Dataref zone. If you are creating your own custom datarefs, this would be done immediately after the 2a region. We'll get into custom datarefs a little later; all you need to know for now, is that if a dataref does not exist in X-Plane, you can create your own in this script.  
  
c. some "local" variables that you may need to define for the rest of your avionics.lua file to draw from.  
  
d. Reset messages. If you want certain functions to have a certain value when your plane is unloaded, you would define it here.   
  
e. Your components. Basically, these are simply the titles of the Lua scripts you'd have sitting in your "Custom Avionics" folder, without the extension. This segment in its most simple form is defined by the word  

```lua
components={  
  
}  
```
  
and the titles of your different Lua scripts would go between those brackets. You may have a whole bunch of scripts in your Custom Avionics folder, so every script that you wish for X-Plane to make use of, needs to be entered here in the components segment, with the following format:  

```lua
components={  
your_script_name { },  
}  
```
  
In our example file, we see that the "flaps.lua" file is referenced here in the "components" area of the "avionics.lua" file. If you were to delete the line of code that references the "flaps.lua" file, your "flaps.lua" instructions would not be calculated in X-Plane.  
  
If you now pull out your handy-dandy SASL reference manual PDF (available as an attachment in Tutorial 1), you can see on page 5, that there are some added parameters you can make use of, to further define the individual components you are calling in. You only need to enter those parameters that you wish to make use of to define your component. Again, we'll talk about those later, as their use comes up in our example files. If you only wish to reference a Lua script in the "Custom Avionics" folder, and the Lua script does nothing more than crunch some numbers with some datarefs (i.e., no animated textures, no drawn instruments, no pop-up windows, etc.), then the above format will suffice. That's all we'll be using for the next little while.  
  
  
## The "Custom Avionics" folder.
_____________________________  
This is basically where your scripts and assets would be kept. So, right now, we have the "flaps.lua" file sitting here, and it contains some instructions, which the "avionics.lua" file acknowledges and conveys to the SASL plug-in, which uses these scripts to assemble a set of instructions for X-Plane to execute.  
  
Now, each script can be anywhere between a couple of lines (like in our example here) to a 100,000 lines or even more. It is up to you to decide how "fragmented" you wish to keep things. On the one hand, it's possible to pack your entire plane's logic into ONE .lua file... but on the other hand, you might find it a lot more overviewable to have one .lua script for each additional feature you wish to take control over in your plane. We've seen now that the "flaps with speed" function can be expressed in a single .lua script. Now we could either keep adding similar features to the plane, adding them to that "flaps.lua" file, or we could create a new .lua file, and define the new functions there. It's really up to you. In the end, it's a matter of keeping things organized in a way that you understand and feel comfortable working with. A larger number of smaller .lua files is probably more recommendable than one really big .lua file, also because that makes it a lot easier to narrow down problems, if you start running in to them.  
  
Each new .lua file that gets added to the "Custom Avionics" folder needs to be referenced in the "avionics.lua" file in the plane's top directory. So, some planes you'd look at, and find that their "Custom Avionics" folder contains the following .lua scripts:  
  
"logic.lua"  
"sounds.lua"  
"electrical.lua"  
"checklist.lua"  
"cameras.lua"  
  
And you would then find that in the "avionics.lua" file, you have these scripts referenced as follows:  

```lua
components={  
  
logic { };  
sounds { };  
electrical { };  
checklist { };  
cameras { };  
  
}  
```  
## Content of .lua scripts in the "Custom Avionics" folder
  
Now, let's take a look at the actual content of the "flaps.lua" file, which is used in the demo file available for download from the first tutorial. Here we'll find several components:  
  
a. Segment where datarefs are called in. If you want to read or write a dataref in your .lua script, you have to call it in. You can call in more datarefs than you'll end up using, but you can't use datarefs in the script that you haven't called in or defined. What you're basically doing is, you're telling the plug-in system, "THIS is the dataref I want to use, and THIS is what I want to call it in my formulas." You're also specifying whether you'll be working with integers or floating point numbers. So: in our example we have two datarefs:  
 
```lua
defineProperty("airspeed", globalPropertyf("sim/cockpit2/gauges/indicators/airspeed_kts_pilot"))  
defineProperty("flaps", globalPropertyf("sim/cockpit2/controls/flap_ratio"))
```
  
So: defineProperty() is the basic function in Lua that will define a property you wish to use later in the script.  
Inside the brackets, you want to fill in which property you want to define how:  
  
First, the name you wish to use for this function (this can be any ol' name, but it's a good idea to give it a label you'll be able to easily recognize when you're working with it below). The name goes in quotes.  
  
Next, the comma, which states that whatever comes after that will be treated as the "contents" of the label.   
  
In our case, the content is a global property, it's a floating point dataref (therefore the small "f" at the end), and then we open another parentheses and put the dataref in quotes inside. (If it were an integer, we'd see an "i" at the end, instead of an "f".)  
  
Now we can use the word "airspeed" in the formulas below, and SASL will know that we're talking about the dataref used in X-Plane for airspeed.   
  
Datarefs can pretty much always be read in, but only writeable datarefs can be altered. So:  
  
"sim/cockpit2/gauges/indicators/airspeed_kts_pilot" is a read-only dataref, as it is used for an indicator gauge in the cockpit. But  
"sim/cockpit2/controls/flap_ratio" is a writeable dataref.  
  
When you look up datarefs, you can use[url="http://www.xsquawkbox.net/xpsdk/docs/DataRefs.html"] this link[/url]to look them up. Use the browser's search feature "Ctrl + F" on a PC or "Cmd + F" on a Mac, to look for your desired dataref.   
  
Then you can also make some assertions about the dataref you're about to use. You can find out if they're read-only or writeable, you can find out if they're floating point (with a decimal) or integers (whole numbers), booleans (True or False) or bitfields, if they're part of an array, you can look up what units that dataref will be calculated in, and you'll get a brief description of that dataref.  
  
It's important for you to define here whether you're going to be using integers or floating point values. Normally, if the dataref has float values, use  
  
```lua
globalPropertyf
```
  
If the dataref has integer values, use  
  
```lua
globalPropertyi  
```
  
If the dataref is part of an array, you MUST define which index of the array you're using. Indicate that with square brackets after the dataref.  
  
  
b. the "Function update" section. Basically, as soon as you create a "Function update" section in your script, whatever you put in there gets calculated once per frame. This is really important. It's the difference between setting a parameter at aircraft load only, vs. creating a calculation that is called up every single frame.  
  
In our example, we have a simple "if-else" statement, a basic programming building block. We will learn the formatting of if-else statements by osmosis, but for now it's important to note a few things:  
  
```lua
defineProperty("airspeed", globalPropertyf("sim/cockpit2/gauges/indicators/airspeed_kts_pilot"))  
defineProperty("flaps", globalPropertyf("sim/cockpit2/controls/flap_ratio"))  
  
function update()  
  
	if get(airspeed) > 80 then  
		set(flaps, 0)  
	end  
  
end
```
  
In plain English, what we're saying in that .lua script is the following:  
  
  
[color=#008000]Go fetch me the dataref for airspeed, which is a floating point dataref, and call it "airspeed", so that I can refer to it as such in the rest of this script.  
Go fetch me the dataref for the flaps, which is also a floating point dataref, and call it "flaps", so that I can refer to it as such in the rest of this script.  
  
We'll use these definitions for the next set of instructions, which will take place in X-Plane once every frame.  
  
If the airspeed dataref is above 80  
then set the flaps dataref to 0.  
Implicitly, we're saying, "otherwise, do nothing."  
And then we're saying "end" this instruction.  
  
Also, end the fact that we're doing this every frame.[/color]  
  
  
  
which, when expressed in code, looks like what you see in your demo file's "flaps.lua" script. (Same as above in code box).  
  
  
  
What's important to understand about the functions that are embedded is, that they have a beginning and an end.   
  
[b]Whatever is between the beginning and the end of "function update ()" gets calculated every frame[/b]. So, you can imagine that space being the "container" for one frame. It will read in a dataref value during [i]that particular frame, [/i]run it through the formula and the commands you've given it, and spit it back into the sim to the writeable datarefs [i]in that same frame. [/i]It starts with "function update()" and ends with "end". And during the next frame, it does the whole thing over again. So, if your sim is running at a frame rate of 30 fps, this particular calculation is done 30 times every second.   
  
  
We see that embedded inside this update function is a formula, which also has a beginning and an end. It starts with "If" and ends with "end". So you have two "end"s at the end of this file. The first end closes off the "if" statement, and the second end closes off the "function update ()" line.  
  
Think of these as performing a similar function in code as parentheses perform in language. You open a parentheses and you need to close it again. But inside that parentheses you can open and close another set of brackets. In Lua, things work similar as in English or other languages, but it's not always as straight-forward to recognize as an opening and a closing parentheses that we see in regular language grammar. So it may take some time and practice to train your eye to recognize openings and closings in code. You can embed pretty much as many functions into one another, and embedding is one of the more crucial things to get a grip on when it comes to programming. It's easy to lose track of the levels of embedding, and it's very easy to forget to terminate one function, because of the possibly many layers of embedding.  
  
Many code-friendly text editors (such as TextWrangler) give you the ability to indent. I recommend making use of indentation in order to help you visually identify the level of embedding that you're currently working in. Indentation and spaces have no further effect in Lua. They can be used to help you visualize and organize your code better.  
  
By far the most common error I make is, to forget to close something that I've opened. You'll always get an error if you do that. If you make a mistake, which will be very common, so don't even worry about it, you'll find that your plane still loads, and everything seems fine... but the functions that depend on the plug-in won't work.  
  
Now, it is important at this point to understand the difference between calculations that are supposed to be made once per frame, and those that only need to be made once when the aircraft is loaded.  
  
Let's say, for instance, you want this plane to load up with the flaps fully extended. That's quite simple. This would be placed OUTSIDE of the "function update" segment... so let's put it just above, like so:  
  
```lua
defineProperty("airspeed", globalPropertyf("sim/cockpit2/gauges/indicators/airspeed_kts_pilot"))  
defineProperty("flaps", globalPropertyf("sim/cockpit2/controls/flap_ratio"))  
  
set(flaps, 1)  
  
function update()  
  
	if get(airspeed) > 80 then  
		set(flaps, 0)  
	end  
	  
end  
```
  
  
Try this. Type (or copy) the above into your "flaps.lua" script, save it, and reload the plug-in in X-plane (please refer to Tutorial 1 if you don't know what I mean.)  
  
Now we're telling SASL to LOAD UP the plane with flaps extended... but it's a one-time command. We can override that with the flap keyboard short cut at any time after the airplane has been loaded, provided the plane isn't flying faster than 80 kts. And the instruction set we gave SASL within the "function update" section still comes into effect, just like before. The "set(flaps, 1)" used above does not conflict with the "set(flaps, 0)" we're using inside the "function update" segment, because the first one was a one-time command.  
  
What would happen if we were to place that "set(flaps, 1)" command INSIDE the "function update" segment? Well, let's try it... instead of retracting the flaps when we're faster than 80 kts, we'll just plain keep them extended by force. So, it would look like this:  
  
```lua
defineProperty("airspeed", globalPropertyf("sim/cockpit2/gauges/indicators/airspeed_kts_pilot"))  
defineProperty("flaps", globalPropertyf("sim/cockpit2/controls/flap_ratio"))  
  
function update()  
  
	set(flaps, 1)  
  
end
```
  
Notice how you can no longer use joystick or keyboard shortcuts to control the flap settings. They are being overwritten EVERY SINGLE FRAME by the command "set(flaps, 1)". So, even if you ARE giving a command with the joystick or keyboard to set the flaps to 0, your joystick's command will be overwritten the very next frame by the plug-in.  
  
Very simple. Set flaps to 1. Period. A very dictatorial command, with no "ifs" and "buts". This is as simple as it gets, and it also clarifies the syntax we're working with:  
  
set(property, value)  
  
where "set" is "go write this next bit into the sim" "property" is the dataref you wish to write to, and "value" is the value you wish to give that dataref.  
  
Of course, "get" then is "read this next bit from the sim." Hence, the bit where we go  
  
```lua
get(airspeed)
```
  
But the format is still very similar.  
  
So now you can also see a bit more clearly what we had originally done with the "if-else" statement. We had made the flap-to-0 setting dependent on the airspeed via an "if-else" statement.   
  
So far so good? OK. Let's try doing some more alterations to this code.  
  
What if we want the flaps to have a INVERSE RELATIONSHIP with the airspeed? What if we want the flaps to extend inversely proportionally to the airspeed? Well, first of all, we'd have to take note that the airspeed range is much larger than the flap range. But let's say we'd want the flaps to be fully extended at 0 knots, and fully retracted at 100 knots, with a gradual transition in between. What we'd have to do then, is find a way to replace the "1" in the "set(flaps, 1)" command with a relationship to the airspeed. This is how it would look:  
  

```lua
defineProperty("airspeed", globalPropertyf("sim/cockpit2/gauges/indicators/airspeed_kts_pilot"))  
defineProperty("flaps", globalPropertyf("sim/cockpit2/controls/flap_ratio"))  
  
function update()  
  
	set(flaps, 1-get(airspeed)/100)  
  
end
```
  
  
So, first of all, notice that with this change, just like when we defined the flaps to always be extended, you are no longer able to control the flaps with your joystick. Next, we have told the flaps to be set to one, and we subtract from that 1 the value of 1/100th of the airspeed. This should gradually retract the flaps as we speed up towards 100 kts, and once we're there, the flaps will forcibly remain retracted.  
  
There are several different ways to write the script to achieve the very same end-results. And it is up to you to come up with the best way to keep things tidy and neat and overviewable, not to mention, efficient. You'll see what I'm talking about a little later, when we start really seeing cluttered functions in our scripts.   
  
The above function could also be written like this:  
  
```lua
defineProperty("airspeed", globalPropertyf("sim/cockpit2/gauges/indicators/airspeed_kts_pilot"))  
defineProperty("flaps", globalPropertyf("sim/cockpit2/controls/flap_ratio"))  
  
function update()  
  
	x = get(airspeed)/100  
	  
	set(flaps, 1 - x)  
  
end
```
  
So, you're defining x as having the value of 1/100th of the airspeed dataref's value, and you're plugging that value into the formula just beneath it. Basic algebra, mixed with X-Plane variables and Lua syntax. Isn't this great?
  
or even like this:  
  
```lua
defineProperty("airspeed", globalPropertyf("sim/cockpit2/gauges/indicators/airspeed_kts_pilot"))  
defineProperty("flaps", globalPropertyf("sim/cockpit2/controls/flap_ratio"))  
  
function update()  
  
	x = 1 - get(airspeed)/100  
	  
	set(flaps, x)  
  
end
```
  
It is crucial to write these things along parameters that are in line with Lua's syntax rules. We'll be learning these by osmosis in these tutorials... so for now, just try copying the examples I give, and maybe explore changing only the numbers to see what that does, but don't touch the actual formatting, until we get a better sense for Lua's syntax.  
  
If you've made a typo somewhere, or if your number of closing parentheses don't match the opening parentheses, your code isn't gonna work. In order to keep an eye on what's going wrong if things don't work, open up the "log.txt" file and scroll to the bottom. It will normally give you an error report, and give you an idea of where to start looking if you've made a mistake somewhere in your code.  
  
TextWrangler users (Mac only), if you open up the "log.txt" file in TextWrangler, you can actually see it being updated LIVE. So, keep that file open while you're working on your scripts, since it's a very quick and easy way to see if things were done correctly or not.  
  
This is as far as we'll go with this tutorial. Basically, we've just dissected and defined the various components that we've used to add the function of retracting the flaps when the plane hits 80 kts. Then we've taken some baby steps toward changing the parameters, exploring ways of arriving at different end-results.