![Other Addons banner](https://i.imgur.com/hdvQEwj.png)

# Addon Support

## Do you need to read this?
If you're the author of an addon for Stalker: Anomaly or GAMMA, and it adds or changes objects in the game, then yeah--you probably should. You'll need to know what's in here if you have to make a patch for *Interaction Dot Marks* compatibility.

Fortunately, the chances are pretty good that you won't have to do anything at all! DotMarks is designed for maximum compatibility, and uses both direct lookups and heuristic methods to identify objects--and the best way to automatically handle them. 

The criteria IDM uses to identify objects are mostly the same ones that the game itself does, and it is constantly monitoring the `UIStaticQuickHelp` container to which the system writes most interaction prompt text.

Therefore, if your changes or new items have attributes assigned to them that are consistent with other like objects, they'll probably work right out of the box--so to speak.

But even some vanilla items require special handling, and some addons will as well. That's why DotMarks provides a suite of callbacks that can be used to exercise total control over how your objects are handled, or even whether they are at all.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

----
# How DotMarks finds and identifies objects

But perhaps that's getting ahead of ourselves. In order to make effective use of these tools, you should probably understand how DotMarks works--and how to make it do exactly what you want. So let's take a step-by-step look at what it does during its main update loop.

> [!NOTE]
> This is the main update loop for DotMarks itself. The marker objects instantiated from `utils_catspaw_hudmarks` each have their own separate update loops, which are driven by the attributes that get set during marker creation and the object's current state.

## The main Actor Update loop

On every actor update, `ui_hud_dotmarks.actor_on_update` does the following:

### 1. Call `do_garbage_collection()` every time
> [!NOTE]
> This is used to clean up unneeded state flags and other temporary values.

### 2. Call `update_hint_filters()` every time
> [!NOTE]
> It is during this step that the `on_quickhelp_text_update` callback gets sent.

### 3. Check tutorial states every `tut_check_interval`
> [!NOTE]
> This is similar to the garbage collection step, used to clean up any tutorial prompts that should no longer be active.
### 4. Call `update_nearby_pickups()` every time, and STOP THERE if `locked_id` is not nil
> [!TIP]
> The `ui_hud_dotmarks.locked_id` variable contains the ID of the currently-locked object. The locked object is forced to have focus, and is the only one with which interaction will be permitted.
>
> This is typically set when the player scrolls their mouse wheel to cycle through nearby pickups. If it is, the main update loop will end here.
### 5. Call `update_targeted_object()` every loop
> [!IMPORTANT]
> as long as `ui_hud_dotmarks.locked_id` is set, this part of the loop *will never happen*, and thus the actor's target will never be updated. If you set `locked_id` to anything for any reason, you must clear it when you are done.

> [!NOTE] 
> This step is also one of the main pieces that is critically dependent on having current modded exes. Without the `db.actor:get_actor_object_looking_at()` method and the `actor_on_update_pickup` callback, DotMarks can only detect an object when it is precisely centered under the crosshair.
### 6. Call `scan_nearby_entities()` with the `near_scan_radius` every `near_scan_interval`
> [!NOTE]
> The `near_scan_radius` is the player's immediate vicinity, within which they can detect markers and marker visibility begins to fade in. This value can be expanded by the Scavenger skill if *Skill System* and its integration are enabled.

### 7. Call `update_mark_vis_ignores()` every `early_scan_interval`
> [!NOTE]
> You'll never need to touch this function, but you should be aware of its purpose: to check whether an NPC who isn't within talk range is capable of dialogue. This is necessary because the vanilla game always considers a stalker to be "mute" until you are close enough to actually interact with them--so until you are, DotMarks doesn't know to show their icon.

### 8. Call `scan_nearby_entities()` with the `early_scan_radius` every `early_scan_interval`
> [!NOTE]
> The `early_scan_radius` is added to the `near_scan_radius`, resulting in the radius for this scan--also called the "far" scan. That resulting value is also the distance beyond which active markers are destroyed, and the maximum distance at which marker initialization will begin. Both near and early scans function identically apart from their intervals and radii.

Most of these steps are not going to be useful for anyone to interfere with externally (really, *please* don't), but the calls in steps (6) and (8) to `scan_nearby_entities` are where the detection and analysis logic happens.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

----
## Object classification 1: `scan_nearby_object` validation checks
Every time `scan_nearby_entities` is called, it iterates through all objects within the passed radius, ultimately calling `scan_nearby_object` on each.

The `scan_nearby_object` function is primarily a validation and optimization step. After basic existence checks, it subjects the object to a series of tests for the following, in order:

----
### 1. Check whether its ID has already been scanned
If any object's ID is present in the `scanned` table, no further action will occur and the function call will exit immediately. This is done so that iterating through already-scanned objects is as fast as possible.
> [!WARNING]
> Don't mess with this. If you want to prevent a particular ID from ever being processed, add it to `cfg.id_blacklist`, or check for it during the `dotmarks_on_new_object_scan` callback.

> [!TIP]
> If you need to force an object to be re-scanned or re-classified, call `ui_hud_dotmarks.unregister_scanned_entity(id)` to terminate the marker and clear all data associated with it. It will be automatically re-scanned on the next update, but you can force it to do so immediately by then calling `ui_hud_dotmarks.scan_nearby_object(id)`.
>
> This is exactly what DotMarks itself does when a stalker dies, in order to convert them into a body marker.

----
### 2. Check whether its name has changed
If it has, DotMarks assumes the ID has been recycled, nulls out `scanned[id]`, then proceeds on to the next check.

----
### 3. Check whether the item is blacklisted
I mentioned above the ID blacklist, but there are also `cfg.clsid_blacklist`, `cfg.section_blacklist`, and `cfg.kind_blacklist`. They can also be added to their respective section names in `dotmarks_defaults` using DLTX. 
> [!NOTE]
> I don't recommend adding anything to the kind blacklist; it's there only as future-proofing, so that a bad "kind" value from a misbehaving/conflicting addon could be filtered out if ever necessary.

----
### 4. Checks whether its `mark_id` is already instantiated
An already-scanned object should've been trapped before now by the check for `scanned[id]`, but it's always possible that someone manually instantiated a marker. This step avoids duplicating or overwriting it.
> [!NOTE]
> The `mark_id` is always `dotmark_<id>`, where `<id>` is the object's numeric ID (e.g. `dotmark_2486`). It is generated automatically during marker init.

----
### 5. Initializes the `args` table by calling `get_args_for_valid_objects()`.
This step is where nearly every piece of information necessary to generate the object's marker gets set up.

----
## Object classification 2: `get_args_for_valid_objects`
When `get_args_for_valid_objects` is called with the object's ID, it first initializes a local `args` table with the following:

```lua
{
 	logged_type	= "something else",
 		-- <string> Used for debug logging -- unimportant, but don't set it to nil
 	name 		= cfg.names_by_section[obj:section()] or utils_catspaw_text.bestname(id) --[[
 		<string> The text name of the object, as it will be shown on prompts
 		cfg.names_by_section is a direct lookup table that matches section names with 
 		localization strings

 		If a section name isn't in this table, it sends the ID to my utility script for naming --]]
 	kind 		= SYS_GetParam(0, obj:section(), "kind")
 		-- <string> The object's "kind" attribute
 	section 	= obj:section()
 		-- <string> the section name of the object
 	clsid 		= obj:clsid()
 		-- <number> The object's clsid 
 	ret_value 	= true
 		-- <boolean> Used for callback control - set false to abort all further action
}
```
This initial table of args is then sent, along with the game object, to the callback that addon authors are most likely to need.

The values passed in the above table of args are meant for use in quickly identifying the object based on its fundamental attributes. The script will expect that table to get populated with everything it needs to know about the object. That might be a little or a lot.

----
## Object classification 3: send and process the `dotmarks_on_new_object_scan` callback

Many potential addon compatibility issues--such as, for example, being detected as the wrong type of object--don't need a callback. They can be most easily solved by adding a section name or similar attribute to a table in `dotmarks_defaults.ltx`.

But if you need to take manual steps or discretionary action during the object classification process, then you'll probably need to make use of this callback. 

----
## Object classification 4: lookups and iteration
But let's put a pin in the callback for now, and assume that no action is taken there. At that point, DotMarks performs three different checks, in this order:

----
### 1. Check the Section Lookup

The script checks whether `obj:section()` is a key contained within the `cfg.section_lookup` table, which--like all other settings--gets loaded from the DotMarks config file.

Section names are one of the most specific criteria possible, short of identifying a specific instance of an object--they're generally the quickest and most reliable way to firmly lock in an item's classification.

----
### 2. Check the Kind Lookup

Just like the first step, except this check's the object's `kind` attribute, which can be gotten with `SYS_GetParam(0, obj:section(), "kind")`.

> [!NOTE]
> An object's "kind" is arbitrary and non-unique, and can be somewhat inconsistently set--it's meant as a heuristic data point for identifying new, unknown items. Don't use it here. If you know the item's section--and if you're the author, you ought to!--it's better to use that.

----
### 3. Iterate through manual tests

This is overwhelmingly the slower method--but it is the only way to properly identify many things.

This final step iterates through each of the item classes in DotMarks--by default, 20 in all--and tests the object one by one against each of them, as quickly and simply as possible. For example, the subordinate function used to test for Stashes looks like this:
```lua
Stashes = (
    function(obj) return obj and IsInvbox(nil, obj:clsid()) end
    -- Containers that the actor can interact with to stash or remove items
)
```

----
## Object classification 5: generating the marker attributes

Once an item is conclusively identified as belonging to a particular item class, the script generates a list of attributes that describe how the marker should behave, and adds them to whatever is already in the `args` table. For a placed backpack stash, those new args would look something like this:
```lua
    logged_type     = "a stash"
    is_container    = true
    is_backpack     = true
    name            = "My Cordon stash"
    has_items       = true
    texture         = dotmarks_main.cfg.marker_dotmark
    hint_filters   	= {
        ["inventory_box_use"]       = "inventory_box_use",
        ["actor_inventory_box_use"] = "actor_inventory_box_use",
        ["st_search_treasure"]      = "st_search_treasure",
    }
    active_swap 	= dup_table(dotmarks_main.cfg.swap_stash)
```
Let's talk about each of them briefly.

----
### logged_type <string>
This is purely for debug logging. It can't be nil, but it can be any descriptive display name you want.

----
### is_container <boolean>
This flag lets DotMarks quickly determine which markers should be handled as containers, without having to check its clsid or query an external lookup like `IsInvBox`.

----
### is_backpack <boolean>
Separate from the above, this flag distinguishes those containers that are placed backpack stashes--which have a different secondary action to pick them up along with everything in them.

----
### has_items <boolean>
A state flag that should be initially set based on the container's contents (or lack of same), and will be automatically updated by DotMarks afterwards whenever the contents change.

----
### texture <string>
This is the texture that will be used for the marker. Setting this to `cfg.marker_dotmark` will use the standard dot marker, but it can be any texture you want.

----
### hint_filters
Hint filters are an internal DotMarks feature that I'll discuss more later. For now, just know that they're a list of localization strings that DotMarks will watch for and try to erase from the `UIStaticQuickhelp` container.

----
### active_swap
The active swap is a subtable of alternate args that will get swapped in whenever the marker has focus, and back out for the original values. It is primarily used to change the marker texture.

----
And that's it--once the script gets those args back, it adds the following to the table:
```lua
args.id         = id
-- The ID of the game object
args.mark_id    = m_id
-- The generated mark_id, e.g. "dotmark_24869"
args.preset     = "dotmark"
-- Instructs utils_catspaw_hudmarks to use this named preset for marker functionality
-- This should never be set to anything else
args.ret_value 	= true
-- Callback control - set to false to cancel marker creation
```
It then sends the current table of args to the `dotmarks_on_before_marker_init` callback, and aborts out if `args.ret_value` isn't true.
> [!TIP]
> This callback can be used as a final sanity check before marker creation, to ensure that none of the values have been changed to anything else by other logic in the script, or another addon.

----
## Object classification 6: generate the marker 
Once it receives the item's table of args, DotMarks will perform one final check: whether that table contains a `texture` attribute.

If DotMarks doesn't find a texture, then it assumes that the object was not recognized and takes no further action. If you null out `args.texture`, it will have the same result as setting `args.ret_value` to false.

It then passes the current args table over to the `setup_marker_for_object()` function to have the marker and prompts generated.
> [!TIP]
> If you want to have no marker texture at all, you can set the texture name to `""`. This will still instantiate the marker, but no icon will be shown for it.

But like I've said before: *you probably won't have to do any of that yourself*. DotMarks is pretty good about identifying most items, because it begins with the same classification logic that the vanilla game--and addons like *Sorting Plus*--already use.

----
## Object classification 7: generate the interaction prompts
The final step that DotMarks must perform is to check whether the marker requires a primary interaction prompt, or if it has both primary and secondary actions--and what they are.

As with other things, it's unlikely you'll need to interfere with this process. If if you need your primary or secondary actions to do something different (for example, triggering a specific function instead of user interaction), or if the auto-generated prompt text isn't what you want, you can use a callback to customize that.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

----
# New callbacks added by Interaction Dot Marks
DotMarks adds a comprehensive suite of callbacks that ought to cover just about any point in its operations that you might want to influence. These are all thoroughly documented in [`dotmarks_callbacks.script`](https://github.com/CatspawMods/Anomaly-Addon-Archive/blob/main/Interaction%20Dot%20Marks/packing/Main/gamedata/scripts/dotmarks_callbacks.script), but we'll briefly cover each here.

----
## `dotmarks_on_init_complete`
```lua
--- @param script_version string
--- @param release_date number
RegisterScriptCallback("dotmarks_on_init_complete")
```
Get notified once DotMarks has completed its initialization, at which point its config file is populated and ready to hook into.

Sent once immediately at the end of DotMarks initialization, which takes place during `on_game_start`. 

----
## `on_quickhelp_text_update`
```lua
--- @param raw_text string
--- @param loc_string string
--- @param trimmed_text string
--- @param flags table
RegisterScriptCallback("on_quickhelp_text_update")
```
Check and manage the contents of `UIStaticQuickHelp`, which is used by the game engine to display most text interaction prompts.

> [!CAUTION] 
> This is sent on every actor update. Do **NOT** put heavy logic in this check.

----
## `dotmarks_on_new_object_scan`
```lua
--- @param id number
--- @param flags table
RegisterScriptCallback("dotmarks_on_new_object_scan")
```
Take action when an object is first scanned, and abort/override its classification or other attributes.

The one callback that most addon authors are likely to need, if they need any at all.

----
## `dotmarks_on_before_marker_init`
```lua
--- @param args table
RegisterScriptCallback("dotmarks_on_before_marker_init")
```
Take action just before a marker is initialized, and either cancel its creation based on its table of args, or adjust the final attributes chosen for the object.

----
## `dotmarks_on_marker_init`
```lua
--- @param args table
--- @param mark utils_catspaw_hudmarks.UIHUDMarker
RegisterScriptCallback("dotmarks_on_marker_init")
```
Take action just after marker initialization, but before the interaction prompts are initialized.

----
## `dotmarks_on_setup_pri_interact`
```lua
--- @param args table
--- @param mark utils_catspaw_hudmarks.UIHUDMarker
RegisterScriptCallback("dotmarks_on_setup_pri_interact")
```
Take action during setup of a marker's primary interact prompt, allowing you to alter its attributes or cancel its creation.

----
## `dotmarks_on_setup_sec_interact`
```lua
--- @param args table
--- @param mark utils_catspaw_hudmarks.UIHUDMarker
RegisterScriptCallback("dotmarks_on_setup_sec_interact")
```
Take action during setup of a marker's secondary interact prompt, allowing you to alter its attributes or cancel its creation.

----
## `dotmarks_on_before_prompt_visibility_change`
```lua
--- @param mark utils_catspaw_hudmarks.UIHUDMarker
--- @param prompt ui_hud_dotmarks.InteractPrompt
--- @param state boolean
--- @param flags table
RegisterScriptCallback("dotmarks_on_before_prompt_visibility_change")
```
Take action just before any interaction prompt changes its visibility state, and optionally cancel that change.

----
## `dotmarks_on_prompt_visibility_change`
```lua
--- @param mark utils_catspaw_hudmarks.UIHUDMarker
--- @param prompt ui_hud_dotmarks.InteractPrompt
--- @param state boolean
RegisterScriptCallback("dotmarks_on_prompt_visibility_change")
```
Take action just after any interaction prompt changes its visibility state, but after the prompt has finished updating itself.

----
## `dotmarks_on_before_secondary_action`
```lua
--- @param id number
--- @param mark utils_catspaw_hudmarks.UIHUDMarker
--- @param prompt ui_hud_dotmarks.InteractPrompt
--- @param functor function
--- @param flags table
RegisterScriptCallback("dotmarks_on_before_secondary_action")
```
Take action when the player triggers a prompt's secondary action, and make changes or choose whether to allow it to continue.

----
## `dotmarks_on_manual_interact_release`
```lua
--- @param id number
--- @param mark utils_catspaw_hudmarks.UIHUDMarker
--- @param flags table
RegisterScriptCallback("dotmarks_on_manual_interact_release")
```
Take action when the player releases the interact key after it has been suppressed--usually when the secondary interact is a long-press of the primary interact keybind.

You're unlikely to ever need this. It is mostly necessary for special handling of interactions with  non-pickup objects that have their own scripted interact logic, such as Glowsticks or Hideout Furniture. If you're adding a new item that uses the Hideout Furniture framework, though, you've probably got some work to do.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

----
# Example custom item: a new kind of placed stash
Now let's put all of that into practice.

For this example, let's assume that your mod adds some new kind of deployable stash. It's supposed to work just like any other item that uses the backpack code: you can place it anywhere, and pick it up by taking everything from it.

In all likelihood, you don't even need to do anything at all for an item like this, because this DotMarks identifies an item as a Stash by checking `IsInvbox(nil, obj:clsid())`--which is of course going to return `true` for any object parented to `inventory_box`.

And that probably means zero work for you. DotMarks will detect that it's an `inventory_box`, and automatically set up all the args just the way they need to be.

## Handling custom items: DLTX the Section Lookup to force item classification
But let's say the autodetection doesn't work properly, and your item is getting detected as a member of--for example--the `Tools` item class instead of `Stashes`. Even some vanilla items fail automatic classification and required special handling. How to solve that for your addon?

The easiest way is to add it to the section lookup with DLTX. Just create a new file in `configs\scripts`, and call it `mod_dotmarks_defaults_myname` (obviously replacing `myname` with something unique to your mod). Then, assuming the section name of your item is `my_weird_backpack`, put this in the file:
```ini
![section_lookup]
my_weird_backpack = Stashes
```
And from then on, DotMarks will always treat that item as a Stash, and ignore all other checks.

----
## Handling custom items: DLTX the Name Lookup to force a specific name string
Okay, so now it's being detected as a stash, but the item is showing the incorrect name on the prompt. It's supposed to be the localization of `st_my_item_name`, but instead it's showing "Unknown stash", or just the literal string "st_my_item_name".

This can be solved very easily by adding this block to the `mod_dotmarks_defaults_myname` file you created:
```ini
![names_by_section]
my_weird_backpack = st_my_item_name
```
From then on, DotMarks will always use that localization string.

----
## Handling custom items: the `dotmarks_on_new_object_scan` callback
Except that still misses one important detail, doesn't it? The fact that your new stash is a deployable item, like a backpack stash. That means that not only will its prompt text be wrong, it won't have the correct secondary action.

That's because sometimes just patching the name or item class and using the default args for the rest isn't enough. DotMarks detects a backpack stash by checking whether its section name is `inv_backpack`--and that's obviously the vanilla item, not yours. If you want your Stash object to be treated like a backpack, you need to manually add that `is_backpack = true` attribute to the list of args.

And the simplest way would be to use the `dotmarks_on_new_object_scan` callback that we briefly talked about above. You register it exactly as you would any other callback in the game:
```lua
function on_game_start()
	RegisterScriptCallback("dotmarks_on_new_object_scan", my_addon_handler_function)
end
```
Using the callback is going to skip the autodetection steps--so if we don't want to have to set up all the args ourselves, we need to first tell it what the item's classification is.

>[!NOTE]
> It's not strictly necessary to set the `item_class` if you're setting all of the args yourself--however, if you don't pass an `item_class`, then you must pass a `texture` instead so that DotMarks knows what icon to use for the marker. Additionally, the item's class (if any) is used to control whether the marker is visible or hidden based on the per-class settings in MCM.

Passing back the item class will ensure that your backpack is being correctly detected as a Stash object, but now you need to flag it as a backpack. To do that, you'd put the following in your handler function:

```lua
function my_addon_handler_function(id, args)
	if args and args.section == "my_weird_backpack" then
		args.item_class 	= "Stashes"
		args.is_backpack 	= true
	end		
end
```
And that ought to be it! That's all DotMarks needs in order to know to handle your new backpack item properly. It will pull the default args for Stashes, then add your flag to them.

----
## Handling custom items: using hint filters
In DotMarks, a **hint filter** is a marker customization that is used to recognize when the game is trying to display an interaction prompt that is associated with that marker--and suppress the vanilla text display.

The hint filter logic itself is completely handled by DotMarks, so all you need to do is tell it which localization strings you're "subscribing" to. For any ordinary pickup item that displays the default interaction prompts, it will be this:
```lua
args.hint_filters    = {
	["inventory_item_use"] = "inventory_item_use"
},
```
For stashes, you should have at least one of these three, if not all:

```lua
args.hint_filters = {
    ["inventory_box_use"]       = "inventory_box_use",
    ["actor_inventory_box_use"] = "actor_inventory_box_use",
    ["st_search_treasure"]      = "st_search_treasure",
}
```
Normally, those get added automatically according to an item's classification. But if you've customized the hint text on your stash, or it has alternate (e.g. locked) states for which it needs to display different text, then you'll need to tell DotMarks what string to subscribe to.

And as you might've guessed by now, it's just as simple as adding it to your callback like this:
```lua
function my_addon_handler_function(id, args)
	if args and args.section == "my_weird_backpack" then
		args.item_class 	= "Stashes"
		args.is_backpack 	= true
		args.hint_filters  	= {
			["st_my_custom_prompt"] = "st_my_custom_prompt"
		}
	end		
end
```
> [!WARNING]
> There is no limit to the number of hint filters you can add, but remember that these get checked on every actor update. A very large number of them could have a performance cost, and should virtually never be necessary.

----
## Further customization: focus icons
A **focus icon** is an alternate texture that is swapped in whenever a marker has focus, and thus becomes active--that is, it is the player's primary target, the one with which any interaction would take place.

The marker system does this using the `active_swap` attribute--which contains a subtable of args that are applied to the marker when focus is gained, and then reverted back to the original values when focus is lost. In the orignal vanilla stash example above, it shows the line that would be used for the default stash icon:
```lua
active_swap 	= dup_table(dotmarks_main.cfg.swap_stash)
```
You could use DLTX to add a new table name to the `template_tables` section of the config file, then DLTX in a new section with that table name--then use that instead of "swap_stash" above. But it'll be simpler and easier to do it like this:
```lua
function my_addon_handler_function(id, args)
	if args and args.section == "my_weird_backpack" then
		args.item_class 	= "Stashes"
		args.is_backpack 	= true
		args.hint_filters  	= {
			["st_my_custom_prompt"] = "st_my_custom_prompt"
		}
		args.active_swap 	= dup_table(dotmarks_main.cfg.swap_stash)
		args.active_swap.texture = "my_custom_stash_icon"
	end		
end
```
Take a look through the swap sections in the config file for an idea of what args get swapped, and what you can do.
> [!WARNING]
> Don't directly reference the swap tables in the main config--make a copy of the table instead, like shown. Otherwise you risk altering the way that swap works for others.

----
## Further customization: other marker attributes
Many marker attributes are simple flags designed to allow rapid processing of the marker without having to query the object or do other checks during every actor update.

Here are some other useful attributes to be aware of--all of which normally get set automatically as necessary, some of them on every update.

----
### [ hidden ] <boolean>
Flags the marker so that it will not be shown by default, even when it otherwise would be.

----
### [ bone ] <string>
Specifies a particular bone name on which the marker will be centered.
> [!NOTE] 
> DotMarks will try to find a valid default bone name if this isn't set--but if it can't find one, or the object doesn't have bones, it will instead anchor the marker on `obj:center()`, which is the center of the physics shell.
> 
> This is undesirable because the physics shell updates its location infrequently compared to bones, and should never be used for a marker that needs to move.

----
### [ pos_adjust ] <vector3>   
A vector value that will be added to the marker's default pos. Adjust the Y attribute of the pos if you want to raise or lower the height of the marker position.
> [!TIP]
> If this is the only change you need to make, you can instead DLTX the object's section name and vertical adjustment into `pos_adj_by_section` within `dotmarks_defaults.ltx`. See the *Hideout Furniture* patch included with DotMarks for an example of this.

> [!WARNING]
> If you set this vector value via script, take care that you only set the Y value, since X and Z will be relative based on the direction the object is facing.

----
### [ is_npc ] <boolean>
Flags the object as an NPC, which among other things checks each loop whether they're still alive--and if not, destroys their marker and re-scans them as a body marker.

----
### [ is_alive ] <boolean>
Set by the marker's main update loop every time while doing the above check, so that other functions don't have to do it again.

----
### [ is_stalker ] <boolean>
Flags the object as a stalker, which among other things checks their dialogue, wounded, and community states.
> [!TIP]
> A stalker must have both the `is_stalker` and `is_npc` flags--they are used for different things.

----
### [ is_enemy ] <boolean>
On every update, DotMarks flags whether the marker belongs to a stalker whose faction is an enemy of the player's. This check respects disguises.
> [!NOTE]
> A living stalker flagged with `is_enemy` will not have their marker shown, unless they are on the ground wounded and capable of dialogue. The `hidden` attribute will get set on them automatically at the beginning of every update.

----
### [ is_monster ] <boolean>
Flags the object for checking whether it is a mutant corpse that has decayed or been looted. If it has done either, the marker is destroyed.
> [!TIP]
> As with stalkers, mutants must be flagged as both `is_monster` and `is_npc`--the meanings of the flags overlap, but they serve different purposes.

----
### [ los_check_range ] <number>
When the actor is within this many meters of the marker, LOS calculations will not be performed and the marker will be assumed visible.
> [!NOTE]
> This defaults to the `early_scan_radius` + 1 meter. See the `HUD MARKER PRESET PATCH` section of `ui_hud_dotmarks` for how this and other attributes get assigned as part of the "dotmarks" preset.
 
----
### [ vis_ignore_mute ] <boolean>
Should always be set on stalkers the player can talk to. Causes their marker visibility to not be controlled by their "mute" state.

----
### [ no_pri ] <boolean>
If set, this marker will not have a primary interaction prompt initialized upon marker init.

----
### [ no_sec ] <boolean>
If set, this marker will not have a secondary interaction prompt initialized upon marker init.
> [!TIP]
> If you need to, you can force your marker to begin with no interaction prompts, and add them later using the `ui_hud_dotmarks` functions `setup_pri_interact_prompt(mark, args)` or `setup_sec_interact_prompt(mark, args)`.
>
> The args used for prompt setup are slightly different than those used for marker init, so refer to those functions for details on how they work.

----
### [ func_exec_pri ] <function>
Direct pointer to a function, not the function's string name. If present, this will be executed when the primary interact is triggered, passing the object's ID and the marker object as first and second parameters, respectively.

----
### [ func_exec_sec ] <function>
Exactly like `func_exec_pri`, but for the secondary action.

----
### [ func_valid ] <function>
If present, this gets executed by each interaction prompt on every update, sending the marker object and the prompt's args as parameters. If it returns false, the interaction prompt will be destroyed, but the marker will not be touched.

----
### [ refresh_text ] <boolean>
If set, this flag will force any interaction prompts based on this marker to refresh their text on every update. When combined with a hint filter, this is used to make a marker's text automatically respond to live changes in the tooltip text that the game is currently trying to display.
> [!TIP]
> If you only want to set this (or any other attribute) for either the primary or secondary interaction prompt, but not the other, then don't put it in the args prior to marker initialization--instead, set it during either the `dotmarks_on_setup_pri_interact` or `dotmarks_on_setup_sec_interact` callbacks, respectively.

----
### [ no_update_loop ]
If this flag is set, DotMarks will not perform any logic to update the marker during any actor update.
> [!WARNING] 
> This skips nearly all tests, state updates, and sanity-checks that are done for this marker on every loop. This should only ever be used for very specific reasons--for example, the workshop stash attached to Hideout Furniture's homestead workshop is a hidden marker that uses this flag.

----
# Other references
The above is very far from being a comprehensive list of marker attributes and valid args! For more information, see the following:

## Table `dotmarks_main.base_config`
The core table of functions that gets imported into the `dotmarks_main.cfg` table. It isn't meant to be modified, but the various "args" functions it contains for each item class demonstrate how to assign attrbibutes ("args") to marker objects in order to define their appearance and behavior.

## File `configs\scripts\dotmarks_defaults.ltx`
Main config file containing all the default runtime settings for the addon. Several of the sections in it (especially the Service NPC swap definitions) have good examples of other marker features like focus icons.

## Class `utils_catspaw_hudmarks.UIHUDMarker`
The `CUIScriptWnd` class used to display and manage markers on the HUD, and automate routine tasks such as self-detecting LOS and fading in/out by distance.

## Class `ui_hud_dotmarks.InteractPrompt`
The `CUIScriptWnd` class used to display interactive prompts which are attached to an arbitrary point in world or screen space. Usually anchored on a `UIHUDMarker` as its parent element.

## `Addon patches that come with DotMarks`
Check out the patches for *Hideout Furniture*, and the one for the Water Pumps from *Fillable Canteens*. The HF patch is very complex, and more work than any of you will ever have to do--but the one from FC is a great example of using `dotmarks_on_new_object_scan` and DLTX to add a custom focus icon and set the attributes for a custom object.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# Anything else?
I hope not. If you're making an addon complicated enough to require more than that, I'd expect you to be able to sort things out yourself from here. Don't DM me, but you can try reaching out to me in the public support channel for DotMarks on the GAMMA or Anomaly modding Discords. My personal ability to help may be limited by my RL obligations, but don't be afraid to dig into the other script files to figure things out--they should be extremely well-commented.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)
