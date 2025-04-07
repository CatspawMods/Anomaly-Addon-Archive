# MCM ARGB Input Widget
Turns any MCM `Input` element with a string value into an ARGB widget that takes the resulting user-selected ARGB values, and parses them into a comma-separated string in the format `255,255,255,255`, which is then cached via MCM to the string value of the `Input` element.

## REQUIREMENTS
  * Anomaly **1.5.3**
  * MCM **1.7.0** or higher -- this script will do absolutely nothing without the ui_hook_functor feature added in that version
  * `configs\ui\ui_argb_control.xml` (or your own replacement UI file, passed via the `xmlfile` attribute)
  * `configs\ui\textures_descr\ui_argb_control_texd.xml` (or your own texture definitions)
  * `textures\catsy\ui_argb_control.dds` (or your own texture file)

The basic implementation is an MCM element taking the following form:
```
{ id = "my_argb_ctrl", type = "input", val = 0, def = "255,255,255,255",
  ui_hook_functor = { ui_mcm_argb_input.init_mcm_argb_control }
}
```
Where `id` is a valid MCM option name of your choice.

An MCM menu can have any number of these widgets--they work just like any other MCM setting control.

## FUNCTOR USAGE
Invoke by adding the following ui_hook_functor to any MCM `Input` element that has a string value (`val = 0`):
```
ui_hook_functor = { ui_mcm_argb_input.init_mcm_argb_control }
```
# Optional Arguments
**All of the following arguments are optional, and should be added to the same `Input` element like any other MCM element attributes:**

  `xml`: Pass your own XML object with pre-parsed file.

  `xmlfile`: Pass your own XML file to be parsed.

  `xmlnode_dialog`: Use this string instead of `box` for the XML node of the base dialog container (no visible texture). Must be large enough to be underneath all input elements so that their callbacks function.

  `xmlnode_chrome`: Use this string instead of `argb_chrome` for the XML node of the UI frame texture container.

  `xmlnode_inputbox`: Use this string instead of `argb_chrome:input` for XML node of the `InputBox` UI containers.

  `xmlnode_textbox`: Use this string instead of `textbox` for XML node of the text containers that show the channel value while the control is inactive.

  `xmlnode_clricon`: Use this string instead of `argb_chrome:clr_icon_` for the XML node of the color icons, each of which gets the string "a", "r", "g", or "b" suffixed to it as appropriate.

  `xmlnode_preview`: Use this string instead of `box` for XML node of the image container used when setting up icon previews.

  `chrome_tex`: Texture to use for the dialog frame.

  `dialog_w`: Width of the ARGB control dialog and UI frame.
    
  `dialog_h`: Height of the ARGB control dialog and UI frame.

  `dialog_x`: Horizontal screen coordinate of the dialog, relative to its parent element.
    
  `dialog_y`: Vertical screen coordinate of the dialog, relative to its parent element.

  `colorblind`: Use an alternative color scheme for the default "accept" and "cancel" buttons, where the green check will instead be blue, and the red "X" will instead be orange.

  `no_color_icons`: Disable the icons representing each color channel.

  `text_labels`: Add "A/R/G/B" text to label each of the color channels. Can be used as an overlay on top of the default color icons. Recommended for use with colorblind mode.
    
  `btn_cancel_tex`: Texture to use for the "cancel" button.

  `btn_accept_tex`: Texture to use for the "accept" button.

  `btn_edit_tex`: Texture to use for the "open/edit" button.

  `preview_func`: Pointer to a function that should return a UI static with a texture, which the script should treat as the "preview" static instead of initializing a new one--changing the color of its texture in response to user input.

  `preview_tex`: Texture to initialize as a preview for the user's current color choices, if you want this script to handle setting it up.

  `preview_w`: Width of the preview texture (default 24).
  
  `preview_h`: Height of the preview texture (default 24).

  `preview_x`: Horizontal position of the preview image.
    
  `preview_y`: Vertical position of the preview image.

  `anchor_on_dialog`: If true, preview will be anchored on the ARGB dialog, and will be hidden when the control dialog is inactive - otherwise the preview will always be displayed and its coordinates are relative to the upper-left of the text caption for the MCM setting.

  `ui_snd_path`: String path to a sound effect that will play when a button is clicked. If omitted, clicks are silent.

  `preset_args`: Any table of key-value pairs, which will be loaded into the widget as if they were attributes specified above.

If you pass your xml or xmlfile arguments, the XML content you pass must have the same node structure as in `ui_argb_control.xml` unless you override the defaults using the `xmlnode_` attributes above.

## Color Blindness
To set up a control that uses colorblind-friendly buttons and text labels, the default Anomaly menu click sound effect, and an automatic preview using the secondary task icon:

```lua
{ id = "example_argb_ctrl", type = "input", val = 0, def = "255,255,255,255",
				ui_hook_functor = { ui_mcm_argb_input.init_mcm_argb_control },
				ui_snd_path = "ui_menu_click",
				preview_tex = "ui_inGame2_PDA_icon_Secondary_mission",
				preview_x 	= 256,
				preview_y 	= 3,
    colorblind = true,
    text_labels = true
}
```

## Presets for multiple elements
If you're setting up multiple options with more than one attribute, consider using the `preset_args` attribute, which takes a table of attributes like so:
```lua
local my_preset = {
				ui_snd_path = "ui_menu_click",
				preview_tex = "ui_inGame2_PDA_icon_Secondary_mission",
				preview_x 	= 256,
				preview_y 	= 3,
    colorblind = true,
    text_labels = true
}

function on_mcm_load()
    	op = { id = "example_menu", sh = true, gr = {
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
