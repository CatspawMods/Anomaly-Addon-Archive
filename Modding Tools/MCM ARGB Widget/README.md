# MCM ARGB Input Widget
Turns any MCM `Input` element with a string value into an ARGB widget that takes the resulting user-selected ARGB values, and parses them into a comma-separated string in the format `255,255,255,255`, which is then cached via MCM to the string value of the `Input` element.
> [!TIP]
> An MCM menu can have any number of these widgets--they work just like any other MCM setting control.

---

## EXTERNAL DEPENDENCIES
  * Anomaly **1.5.3** (vanilla or modded)
  * MCM **1.7.0** or higher
> [!WARNING]
> The script will do absolutely nothing without the `ui_hook_functor` feature added in MCM 1.7.0, and MCM 1.7.0 requires Anomaly 1.5.3 or higher.

---

## REQUIRED FILES
  * `scripts\ui_mcm_argb_input.script`
  * `configs\ui\ui_argb_control.xml`
  * `configs\ui\textures_descr\ui_argb_control_texd.xml`
  * `textures\catsy\ui_argb_control.dds`
> [!NOTE]
> The script is always required, but you can optionally pass custom XML and texture definitions using the attributes below--replacing all or part of the other three files with your own content.

---

## USAGE
The basic implementation is an MCM element taking the following form, used within an MCM menu like you would any other `Input` element:
```lua
{ id = "my_argb_ctrl", type = "input", val = 0, def = "255,255,255,255",
	ui_hook_functor = { ui_mcm_argb_input.init_mcm_argb_control }
}
```
Where `id` is a valid MCM option name of your choice.

And that's it--as long as the script and any dependencies are present, it will do the rest of the work for you.
> [!IMPORTANT]
> The `Input` element must have a string value--that is, it must have the `val = 0` attribute.

---

# Optional Arguments
Of course if you want to customize further, you have plenty of options for doing so.

**All of the following arguments are optional, and should be added to the same `Input` element, just like the attributes of any other MCM element:**
###  `xml = <xml_object>`
Pass your own XML object with pre-parsed file.
###  `xmlfile = <string>`
Pass your own XML file to be parsed.
> [!IMPORANT]
> If you pass your own `xml` or `xmlfile` arguments, the XML content you pass must have the same node structure as in `ui_argb_control.xml` unless you override the defaults using the `xmlnode_` attributes below.
###  `xmlnode_dialog = <string>`
Use this string instead of `box` for the XML node of the base dialog container (no visible texture).
> [!IMPORTANT]
> The `xmlnode_dialog` element must be large enough to be fully underneath all of its input control elements, or their callbacks may not function.
###  `xmlnode_chrome = <string>`
Use this string instead of `argb_chrome` for the XML node of the UI frame texture container.
###  `xmlnode_inputbox = <string>`
Use this string instead of `argb_chrome:input` for XML node of the `InputBox` UI containers.
###  `xmlnode_textbox = <string>`
Use this string instead of `textbox` for XML node of the text containers that show the channel value while the control is inactive.
###  `xmlnode_clricon = <string>`
Use this string instead of `argb_chrome:clr_icon_` for the XML node of the color icons.
> [!IMPORTANT]
> There must be four of these--each of which gets the string "a", "r", "g", or "b" suffixed to it as appropriate.
###  `xmlnode_preview = <string>`
Use this string instead of `box` for XML node of the image container used when setting up icon previews.
###  `chrome_tex = <string>`
Texture to use for the dialog frame.
###  `dialog_w = <number>`
Width of the ARGB control dialog and UI frame.
###  `dialog_h = <number>`
Height of the ARGB control dialog and UI frame.
###  `dialog_x = <number>`
Horizontal screen coordinate of the dialog, relative to its parent element.
###  `dialog_y = <number>`
Vertical screen coordinate of the dialog, relative to its parent element.
###  `colorblind = <boolean>`
If true, use an alternative color scheme for the default "accept" and "cancel" buttons, where the green check will instead be blue, and the red "X" will instead be orange.
> [!NOTE]
> There isn't a built-in function that lets the control switch between normal and colorblind modes at will--but you could create a button or function of your own that reinits the ARGB control.
###  `no_color_icons = <boolean>`
If true, disables the icons representing each color channel.
###  `text_labels = <boolean>`
If true, adds "A", "R", "G", or "B" as appropriate to label each of the color channels.
> [!TIP]
> These can be used alone or in combination with the color icons, in which case they will function as a text overlay on top of the default icons. Recommended for use with colorblind mode.
###  `btn_cancel_tex = <string>`
Texture to use for the "cancel" button.
> [!IMPORTANT]
> This texture and the two that follow must be "button" textures with EHT (Enable, Highlight, Touch) variants. The Disable variant is not used by this control.
> 
> In other words, if you pass `btn_cancel_tex = "foo_bar"`, then there must exist textures "foo_bar_e", "foo_bar_h", and "foo_bar_t".
###  `btn_accept_tex = <string>`
Texture to use for the "accept" button.
###  `btn_edit_tex = <string>`
Texture to use for the "open/edit" button.
###  `preview_func = <function>`
Pointer to a function that should return a UI static with a texture, which the script should treat as the "preview" static instead of initializing a new one--changing the color of its texture in response to user input.
> [!IMPORTANT]
> This must be an actual function or a pointer to one, not a "script.functor" string name.
###  `preview_tex = <string>`
Texture to initialize as a preview for the user's current color choices, if you want this script to handle setting it up.
> [!TIP]
> This works best if you use an actual mock-up or icon that looks like the one you're customizing in-game.
###  `preview_w = <number>`
Width of the preview texture (default 24).
###  `preview_h = <number>`
Height of the preview texture (default 24).
###  `preview_x = <number>`
Horizontal position of the preview image.
###  `preview_y = <number>`
Vertical position of the preview image.
###  `anchor_on_dialog = <boolean>`
If true, preview will be anchored on the ARGB dialog, and will be hidden when the control dialog is inactive - otherwise the preview will always be displayed and its coordinates are relative to the upper-left of the text caption for the MCM setting.
###  `ui_snd_path = <string>`
String path to a sound effect that will play when a button is clicked. If omitted, clicks are silent.
###  `preset_args = <table>`
Any table of key-value pairs, which will be loaded into the widget as if they were attributes specified above.

---

# EXAMPLES
## Color Blindness
To set up a control that uses colorblind-friendly buttons and text labels, a click sound effect, and an automatic preview using the secondary task icon:

```lua
{ id = "example_argb_ctrl", type = "input", val = 0, def = "255,255,255,255",
	ui_hook_functor = { ui_mcm_argb_input.init_mcm_argb_control },
	ui_snd_path	= "ui_menu_click",
	preview_tex	= "ui_inGame2_PDA_icon_Secondary_mission",
	preview_x 	= 256,
	preview_y 	= 3,
	colorblind	= true,
	text_labels	= true
}
```
> [!TIP]
> You can use `ui_snd_path = "ui_menu_click"` in any ARGB control to give it the default Anomaly menu click sound--it's already a part of the base game.

---

## Presets for multiple elements
If you're setting up multiple options with more than one attribute, consider using the `preset_args` attribute, which takes a table of attributes like so:
```lua
local my_preset = {
	ui_snd_path	= "ui_menu_click",
	preview_tex	= "ui_inGame2_PDA_icon_Secondary_mission",
	preview_x 	= 256,
	preview_y 	= 3,
	colorblind 	= true,
	text_labels	= true
}

function on_mcm_load()
	op = {
		id = "example_menu", sh = true, gr = {
			{ id = "example_control_1", type = "input", val = 0, def = "255,255,255,255",
				ui_hook_functor = { ui_mcm_argb_input.init_mcm_argb_control },
				preset_args = my_preset
			},
			{ id = "example_control_2", type = "input", val = 0, def = "255,255,255,255",
				ui_hook_functor = { ui_mcm_argb_input.init_mcm_argb_control },
				preset_args = my_preset
			}
		}
	}
	return op
end
```
...and so forth.
> [!TIP]
> This very quickly becomes worth doing if your menu has more than a couple of ARGB controls, or they each have more than a few duplicated attributes.

---
