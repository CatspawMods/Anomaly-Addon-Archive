# CIRCULAR PROGRESS INDICATOR PLUGIN
A customizable circular-style progress indicator that works much like any other that you've seen in any other game. It does nothing whatsoever on its own--this is a tool for modders.

[Circular Progress Indicator image](https://i.imgur.com/pyO1ese.png)

The XRengine's default `InitProgressBar` method is strictly linear, though it can be oriented to increment in any of the four cardinal directions. What it cannot do is curve.

This is about the simplest possible implementation of a circular indicator. It has a few stylistic limitations owing to its simplicity, but if what you want is to have a simple solid-color circular indicator, this will do the job. It measures progress from 0 to 1; any other range should be normalized before passing.

# TERMS
* Give credit where due
* Don't distribute altered copies with the same filenames

# REQUIREMENTS
## Modded Exes
	* This requirement is not optional. If you're not using modded exes--or a modpack that is based upon them, like GAMMA--you should be. Get them here: 
	[Modded Exes on Github](https://github.com/themrdemonized/xray-monolith)

## The following files, unless you pass arguments that specify your own replacements for the textures and UI XML:

* `configs\ui\ui_circular_progress.xml`
* `configs\ui\textures_descr\ui_circular_progress_texd.xml`
* `textures\catsy\ui_circular_progress.dds`


# USAGE
`local my_new_ind = ui_circular_progress.init_new_indicator(flags, ind_id, anchor)`

Or you can invoke the class directly if you know what you're doing, the syntax is the same.

All parameters are optional, depending on what you're trying to do. By default the indicator is white, 32px in size, and is anchored on the upper left corner of the screen.

Since that location is not super-useful, you'll probably want to pass some arguments in the `flags` table to move it around, anchor it to something else, or customize it in other ways. See below for a list of options.

The `ind_id` argument is an optional unique identifier for this instance of the indicator. It will be generated if not passed.

Passing any other static as the anchor will have it treat that static as its parent, instead of being anchored on the main screen canvas.

Once instantiated, you can use the following class methods:

	`SetColor(ARGB_value)`
		Changes the indicator color from its default of white. Must be a valid ARGB value.

	`SetProgressPos(progress)`
		Where progress is a number from 0 to 1. Omitting this will set progress back to its default of zero.

	`SetSize(width, height)`
		Changes the size of the indicator. Omitting a parameter will reset the indicator to its defined size.

Valid attributes for the `flags` table (all optional):

	`xml`
		A valid CScriptXMLInit object that has already parsed a UI XML file, ready to use.

	`xmlfile`
		Alternatively, a filepath to parse for the indicator's UI structure.

		If neither of these are passed, the indicator will use (and requires) its own UI file--which, if so, must be distributed unaltered along with the script.

	`xmlnode` (default "indicator")
		The name of the XML node that should be used from the parsed UI file.

	`width` (default 32)
		The width of the indicator. Its (ridiculous and probably unnecessary) maximum size is 128x128, the native size of the source texture. It is made this large in order to
		ensure it does not look pixelated at high resolutions.

	`height` (default 32)
		The height of the indicator. It not a good idea to make it different than the width--it'll most likely look like shit--but you can if you have a use case for it.

	`base_texture` (default ui_catsy_circprog_black)
		The texture name to use for the background of the indicator. By default this is a black ring--and a blacked-out version of the indicator should disappear when drawn on top of it.

	`left_texture` (ui_catsy_circprog_left)
		Texture name to use for the left half of the progress indicator. This must be a square texture with the right half of the indicator missing--not just cropped to the left half of one.

	`right_texture` (ui_catsy_circprog_right)
		Texture name to use for the right half, same rules--this needs to be a square texture with the left half of the indicator missing, not just cropped to the right half.

		A blacked-out version of this is also used as a mask for the right half of the indicator when it's not needed (when progress is < 0.5).

		If it's not clear to you what this means and you still want to customize the texture, go look at the default textures in textures\catsy\ui_circular_progress.dds

	`progress_argb`
		The `GetARGB` for a color to apply to the indicator texture using `SetTextureColor`. The default indicator is a simple white ring.

		If you plan on using this attribute with a custom indicator texture, it's best to use a white or monochromatic texture.

	`offset_x`, `offset_y`
		Optional screen offsets to use. These are relative to whatever its anchor point/parent is.
	
	`initial_value`
		Optionally specify a starting value for the indicator. This can be changed at any time after init, using the `SetProgressPos` method.

# HOW IT WORKS
If you want to replace the textures or mess around with the code, it'll help to know how it all gets used.

The short version: the left and right halves of the indicator get rotated out as progress rises to give the illusion of one line. The right half (blacked out) is also used as a mask for the real right half to hide behind.

In more detail:
1. The left and right halves of the indicator, when both are shown in their starting positions, form a complete circle.
2. The left half of the indicator is hidden when progress is under the halfway point (a value of 0.5).
3. A copy of the right half--turned black, and drawn on top of it--is then used as a mask for the real right half to hide behind.
4. The real right half then gets rotated around the center to show progress, seeming to grow from nothing as it appears out from under the mask layer on the right.
5. When progress reaches 0.5, the left and right halves are both on top of each other, on the left side. The left half is then set visible, swapped in to hold position on the left half that now remains the same.
6. The right half then continues to rotate around its center to show progress, appearing to extend from the end of the left half as if they were a single continuous line until finally becoming a circle at a progress of 1, with the right half back where it started.

The main takeaway from this is to **avoid texturing the left and right halves in a way that will break the illusion** as the texture rotates. Fine details or regular markings are veryt likely to look bad--a solid color or a tube gradient is probably best.

If you intend to use `SetColor` to change the color of the indicator dynamically, the base texture should be white or shades of grey.

As well, because a blacked-out version of the right half is used to hide the real right half, you'll need to make sure that the background (base_texture) is black behind the ring.
