# **Catspaw's Utility Scripts**

There are a bunch of functions that I use across my addons, and over time they've added up to a fair bit of duplicated code that I have to update in multiple places anytime they get revised.

This is an attempt at centralizing them, and unifying some functions that I'd like to be more consistent across my addons (like color and text tokenization).

If you use any of my addons, I recommend that you have the latest version of the utility scripts from this folder, and *always* load it last, after any of my other addons. That way no matter which version of my addons you're using, or what order you load them in, you'll always have the most up-to-date version of the utility scripts.

**For most of my addons, you only need the "Catspaw Core Utility Scripts" part of the package.** The "Talk to any Stalker monkeypatch" is safe and useful, as is "No floating item inv icons"--but the others are intended as modding tools, and should only be enabled when an addon requires them.

The "Tutorial Monitor" in particular should only be used if you know exactly what you're doing--it will currently have the side effect of removing many of the interaction prompts in the game, because it's designed for an addon that replaces them (DotMarks).

Feel free to lift code from any of these scripts, or even distribute copies of them in your own addon, but in the interest of avoiding any possibility of conflicts:

##	**PLEASE DO NOT MAKE CHANGES TO ANY OF THESE SCRIPTS THEMSELVES.**

Seriously. Just copy the code into a file with a different name if you want to mess with it. I don't mind, I just don't want mod conflicts--and neither do you.
