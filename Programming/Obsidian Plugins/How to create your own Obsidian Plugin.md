https://www.youtube.com/watch?v=XaES2G3PVpg
Short form line count tutorial - https://www.youtube.com/watch?v=AgXa03ZxJ88
Obsidian Series - https://www.youtube.com/watch?v=CtR-d-gyxHg&list=PLIDCb22ZUTBnMCbJa-st4PD5T3Olep078
https://github.com/obsidianmd/obsidian-api
Official developer docs - https://docs.obsidian.md/Home
# Getting things set up

To create your first Obsidian Plugin, you need to have:

- [NodeJS](https://nodejs.org/) installed
- A Code Editor of your choice installed (My personal recommendation would be: [Visual Studio Code](https://code.visualstudio.com/))

If you want to publish your Plugin someday, you also need the following:

- [Git](https://git-scm.com/), a Version Control System
- A [GitHub Account](https://github.com/signup)

You will be using Git to version your Plugin and also to push and pull changes from the remote Repository, which is basically just a “Folder in the Cloud”. In the End, I will tell you more about how to publish the Plugin, so that other people can install it from inside Obsidian.

The next step would be to clone the [official Sample Plugin](https://github.com/obsidianmd/obsidian-sample-plugin) from GitHub.

## With Git and GitHub

If you have a GitHub Account, just click the “Use this Template” Button and follow the Instructions:

![](https://miro.medium.com/v2/resize:fit:1400/1*7IZJciKB7Ci5oOTX9wugpA.png)

![](https://miro.medium.com/v2/resize:fit:1400/1*obUhZtw0Lxu8UElDeVNVZw.png)

Once you have your own Repository set up, just copy the following URL:

![](https://miro.medium.com/v2/resize:fit:1400/1*L5VgMlCQlCXyfiKRIlYuIA.png)

Afterwards, you can clone the Repository locally using the following shell command. Just make sure you are in the correct Directory you want your Plugin to live in.

Now you can finally open the Folder with your Code Editor.

## Without Git and GitHub

If you don’t plan on publishing your Plugin or don’t have a GitHub Account, you can also just download and unpack the Sample Plugin as a ZIP Archive:

![](https://miro.medium.com/v2/resize:fit:1400/1*uTVCB2A_oMXC8G7wM2Qgzg.png)

Now you can finally open the Folder with your Code Editor, too.

## Compiling and running the Plugin

You will see, there are a lot of files already. But don’t worry, only three of them matter at the moment:

- The `main.ts` File, which contains the Code of your Plugin
- The `styles.css` File, which contains a corresponding Style Sheet
- And the `manifest.json` File, which contains important Information, like the Version and Name of your Plugin

If you want to, you can already go ahead and look at these Files, you could for example edit the `manifest.json` to your liking.

Obsidian cannot execute TypeScript files directly, you will need to compile them to JavaScript first. Luckily, everything, including bundling, is already set up correctly in the Sample Plugin, so you don’t have to worry too much about it 🥳.

Most Code Editors, like Visual Studio Code, already have a Terminal Window built in. You can use it to run the following two commands after one another:

The First one will download and cache all dependencies, and the second one will compile your Typescript Files to a single JavaScript File.

Once that’s done, go ahead and copy the `styles.css`, `manifest.json` and `main.js` into a newly created Folder inside your Vault under `<your-vault>/.obsidian/plugins/sample-plugin/`. Now go into Obsidian and enable your Plugin in the Community Plugins Settings:

![](https://miro.medium.com/v2/resize:fit:1400/1*NQO9j-nTKciyfcRwO6Y22Q.png)

You will notice, that most Text turned red. This is because of the following CSS inside the `styles.css` you just copied:

This is just a demo to help you get started, you can delete it without consequences.

# The Sample Plugin

Now that everything is set up, let’s look at the Plugins Code. Note that I stripped the SampleModal and SampleSettingTab at the End of the File.

Line 1: Import premade Functions and Components from Obsidian, so you can use them in your Plugin

Line 3–9: The Settings the Plugin can store and the default Values

Line 11: A class extending “Plugin” is exported. This is always necessary for a Plugin

Line 14: The onload() Function. This is called every time a Plugin is loaded in Obsidian. In the Sample Plugin it adds a lot of Stuff, but that's just to explain how it works. You can ignore most of them.

Line 17: The Settings are loaded into Obsidian. They are stored on the Disk to persist even when Obsidian is closed. You can see the corresponding Function at the bottom (Line 51).

Line 47: The onunload() Function does the same thing as onload(), but when the Plugin is disabled. Note that this isn’t called when Obsidian closes.

There will also be two more Classes. One extends the “Modal” Component and another one which extends “SettingTab”. These are used to create a dedicated Tab in the Settings or to show these nifty pop up Modals.

# Recreating Cycle through Panes

In our Case we don’t need most of this. Our Plugin won’t even have configurable Settings. But what does it need to do? If you are reading this you are most likely using a Browser, to switch between your open Tabs you can press Ctrl + Tab. Cycle through Panes replicates that behavior with Panes in Obsidian. So, what we need to do is the following:

- Add a Command to go to the next Pane, triggered by Ctrl + Tab
- Add a Command to go to the previous Pane, triggered by Ctrl + Shift + Tab

Lets use the following as our Starting Point:

This is almost the bare minimum you will need for any Plugin. Inside the onload() method we will be adding the two Commands. The Obsidian API exposes a method on the Plugin Class to add Commands, if you add them using this official way the User will also be able to change Hotkeys, just like with every other Command! If you want to read up on it, see the official Type Definition [here](https://github.com/obsidianmd/obsidian-api/blob/5af3ef982328a2dd0a2f5242f3c74e2f45e07896/obsidian.d.ts#L2195).

The addCommand() method takes a [Command Object](https://github.com/obsidianmd/obsidian-api/blob/5af3ef982328a2dd0a2f5242f3c74e2f45e07896/obsidian.d.ts#L410) as a parameter, which has fields for a name, which is going to be the display Text, a id, which is basically the internal name, and a callback, which is a function that will be executed every time the command is invoked.  
Instead of using a regular callback, one can use a checkCallback, editorCallback or editorCheckCallback. A regular callback can be invoked from everywhere in Obsidian, while any of the two editorCallback’s only work while a Editor is active, the Editor instance will also be available inside the function as a parameter. The checkCallback’s are used if you need to implement your own Logic on when the Callback can be invoked. As an example, the sample implementation for that only works when there is an active Leaf.  
Other optional fields are mobileOnly, icon (mainly for Obsidian Mobile’s Toolbar) and hotkeys.

Let’s just create the forward cycle first: Add an ID, Name and a checkCallback, for now you can use an empty arrow Function here. If you want to, you can also set default Values for the Hotkeys already, but it is generally advised to not do that to avoid conflicts between Plugins. It should look something like this:

Now that we have that, how do we implement the rest?

In Line 12 we store the currently active leaf inside a variable called “active”. If there is no active Leaf this will be null and thus the callback will return false and not work nor show up in the Command Palette. If the User invoked the Command the passed checking variable will be false and thus our Logic will run.

In Line 15 we first get all Markdown Leaves and right after get the Index of the currently active Leaf.

Now we need to handle an interesting Case. If the active Pane is the last one and the User switches to the next one we want to set the first Pane as active. This is handled by the if statement. If this is not the case we can simply increment the index by one.

Now the only thing thats missing is the same Command, but to go in reverse, from right to left. We can copy almost all of the Code, except that we need to change the id, name and callback a little bit. In the next Gist you will see the complete Code:

The full Plugin

# Publishing a Plugin

Now everything that’s left to do is publish the Plugin. This involves 4 Steps, filling out the manifest.json, pushing the Code to GitHub, creating a Release on GitHub and finally making a Pull Request to the obsidian-releases Repository with your Plugin’s information.

## Filling out the manifest

The Manifest has 8 fields, the _id_ and _name_, which is almost the same as in a Command, the _version_, which needs to follow [Semantic Versioning](https://semver.org/), the _minAppVersion_, which is the minimal Obsidian Version that your Plugin depends on, this might change if Obsidian’s API changes. A _description_ which is used for the search in the Plugin Browser, the _author_ and _authorUrl_, which should be you and your website or GitHub Profile and lastly the _isDesktopOnly_ field, which needs to be set to true if your Plugin interacts with NodeJS or you are are relying on CodeMirror 5.

## Pushing to GitHub

This Step is super easy if you already created a Repository in the first Step of this Article. Just run the following Commands:

## Creating a Release on GitHub

![](https://miro.medium.com/v2/resize:fit:654/1*135gCoRH8GSeOzHR9JX4qA.png)

On the right hand side, click on “Create new Release”. Inside the “Choose a Tag” field add the current version of your Plugin without a v at the front. This needs to match your manifest.json. If you want you can add a description and a Title, but thats optional. What you will need to do, is adding the main.js (run _npm run build_ to compile it) , manifest.json and if you have one styles.css at the bottom. Once you are ready, hit “Publish Release”.

## Adding the Plugin to the Community Plugins

Go to the official Obsdian-Releases Repository and [open the Community Plugins File](https://github.com/obsidianmd/obsidian-releases/blob/master/community-plugins.json). On the top right hit Edit and add your Plugin to the end of the List like so:

![](https://miro.medium.com/v2/resize:fit:820/1*8VzDSAOeyjCGP_KQsf1Jfw.png)

Editing the File

![](https://miro.medium.com/v2/resize:fit:862/1*0BMB4c9k9fJzOfg0bQeqHw.png)

Adding your Plugin to the File

You just need to make sure to not miss any commas and double check the Repository Name. After that you can click on “Propose Changes” and simply follow the Instructions given by the Obsidian Developers. You are done 🥳!