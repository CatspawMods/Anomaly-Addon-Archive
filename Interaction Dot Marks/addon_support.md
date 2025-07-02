![DotMarks top banner](https://i.imgur.com/OXAK0xS.png)

# Addon Support

## Do you need to read this?
If you're the author of an addon for Stalker: Anomaly which adds or changes objects in the game, then yeah--you probably should. You'll probably need to know what's in here if you have to make a patch for Interaction Dot Marks compatibility.

Fortunately, the chances are pretty good that you won't have to do anything at all! DotMarks is designed for maximum compatibility, and uses both direct lookups and heuristic methods to identify objects--and the best way to automatically handle them. 

The criteria IDM uses to identify objects are mostly the same ones that the game itself does, and it is constantly monitoring the `UIStaticQuickHelp` container to which the system writes most interaction prompt text.

Therefore, if your changes or new items have attributes assigned to them that are consistent with other like objects, they'll probably work right out of the box--so to speak.

But even some vanilla items require special handling, and some addons will as well. That's why DotMarks provides a suite of callbacks that can be used to exercise total control over how your objects are handled, or even whether they are at all.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## How DotMarks finds and identifies objects

But perhaps that's getting ahead of ourselves--in order to make effective use of these tools, you should probably understand how DotMarks works--and how to make it do exactly what you want.

### The main Actor Update loop

On every actor update, `ui_hud_dotmarks.actor_on_update` does the following:

1. Call `do_garbage_collection()` every loop
2. Call `update_hint_filters()` every loop
3. Check tutorial states every `tut_check_interval`
4. Call `update_nearby_pickups()` every loop, and stop there if `locked_id` is set
5. Call `update_targeted_object()` every loop
6. Call `scan_nearby_entities()` with the `near_scan_radius` every `near_scan_interval`
7. Call `update_mark_vis_ignores()` every `early_scan_interval`
8. Call `scan_nearby_entities()` with the `early_scan_radius` every `early_scan_interval`

Most of these are not going to be useful externally, but the calls to `scan_nearby_entities` are where the detection and analysis logic happens.

> [!NOTE]
> The `near_scan_radius` is the player's immediate vicinity, within which they can see markers. The `early_scan_radius` is added to the `near_scan_radius`, and that value is the distance beyond which active markers are destroyed. Both scans function identically apart from their intervals and radii.

### Object identification 1: `scan_nearby_object` validation checks

Each time `scan_nearby_entities` is called, it iterates through all objects within the passed radius, ultimately calling `scan_nearby_object` on each.

The `scan_nearby_object` function is primarily a validation and optimization step. After basic existence checks, it subjects the object to a series of tests for the following, in order:

1. Check whether its ID is in the `scanned` table. If any object ID in this table, no further action will occur.
2. Check whether its name has changed. If it has, it assumes the ID has been recycled, and nulls out `scanned[id]`.
3. Checks whether its `mark_id` is already instantiated.
> [!NOTE]
> The `mark_id` is always `dotmark_id`, where `id` is the object's numeric ID (e.g. `dotmark_2486`).
4. Initializes the `args` table by calling `get_args_for_valid_objects()`.

### Object identification 2: `get_args_for_valid_objects`

This step is where nearly every piece of information is staged that will be used to generate the object's marker. When `get_args_for_valid_objects` is called with the object's ID, it first initializes the `args` table with the following:

```lua
{
 	logged_type = "something else",
 	-- Used for debug logging -- it just has to be a non-null string
 	name 		= cfg.names_by_section[sec]
 	-- A direct lookup table that matches section names with localization strings
 	kind 		= kind
 	-- The object's "kind" attribute
 	section 	= sec
 	-- obj:section()
 	clsid 		= cls
 	-- obj:clsid()
 	ret_value 	= true
 	-- Used for callback control
}
```
This initial table of args is then sent, along with the game object, to the callback that you--as an addon author--are most likely to need.

The values passed in the above table of args are meant for use in quickly identifying the object based on its fundamental attributes. The script will expect that table to get populated with everything it needs to know about the object. That might be a little or a lot.

### Object identification 3: send and process the `dotmarks_on_new_object_scan` callback

Many potential addon compatibility issues--such as, for example, being detected as the wrong type of object--don't need a callback. They can be most easily solved by adding a section name or similar attribute to a table in `dotmarks_defaults.ltx`.

But if you need to take manual steps or discretionary action during the object identification process, then you'll probably need to make use of this callback. 

### Object identification 4: lookups and iteration
But that's getting ahead of ourselves--let's put a pin in the callback for now, and assume that no action is taken there. At that point, DotMarks performs three different checks, in this order:

1. Check the Section Lookup

The script checks whether `obj:section()` is a key contained within the `cfg.section_lookup` table, which--like all other settings--gets loaded from the DotMarks config file.

Section names are one of the most specific criteria possible, short of identifying a specific instance of an object--they're generally the quickest and most reliable way to firmly lock in an item's classification.

2. Check the Kind Lookup

Just like the first step, except this check's the object's `kind` attribute, which can be gotten with `SYS_GetParam(0, obj:section(), "kind")`.

![NOTE]
> An object's "kind" is arbitrary and can be somewhat inconsistently set--it's meant as a heuristic data point for identifying new, unknown items. If you know the item's section, it's better to use that.

3. Iterate through manual tests

This is overwhelmingly the slower method--but it is the only way to properly identify many things.

This final step iterates through each of the item classes in DotMarks--by default, 20 in all--and tests the object one by one against each of them, as quickly and simply as possible. For example, the subordinate function for testing Stashes looks like this:
```lua
Stashes = (
    function(obj) return obj and IsInvbox(nil, obj:clsid()) end
    -- Containers that the actor can interact with to stash or remove items
)
```

### Object identification 5: generating the marker attributes

Once an item is conclusively identified as belonging to a particular item class, the script generates a list of attributes that describe how the marker should behave, and adds them to whatever is already in the `args` table. For a placed backpack stash, those new args would look like this:
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
#### logged_type
This is purely for debug logging. It can't be nil, but it can be any descriptive display name you want.
#### is_container
This flag lets DotMarks quickly determine which markers should be handled as containers.
####
Separate from the above, this flag distinguishes those containers that are placed backpack stashes--which have a different secondary action to pick them up along with everything in them.
#### has_items
A state flag that should be initially set based on the container's contents (or lack), and will be updated by DotMarks afterwards.
#### texture
This is the texture that will be used for the marker. Setting this to `cfg.marker_dotmark` will use the standard dot marker, but it can be any texture you want.
#### hint_filters
Hint filters are a DotMarks feature that I'll discuss more later. For now, just know that they're a list of localization strings that DotMarks will watch for and try to erase from the `UIStaticQuickhelp` container.
#### active_swap
The active swap is a subtable of args that will get swapped in whenever the marker has focus, and back out for the original values. It is primarily used to change the marker texture.

And that's it--once the script gets those args back, it creates the marker and begins setting up any necessary primary and secondary prompts. More on that later.

But here's the best part: *you probably won't have to do any of that yourself*. DotMarks is pretty good about identifying most items, because it begins with the same classification logic that the vanilla game--and addons like *Sorting Plus*--already use.

### Example item: a new kind of placed stash

For this example, let's assume that the item you've added is a new kind of deployable stash. It's supposed to work just like any other item that uses the backpack code: you can place it anywhere, and pick it up by taking everything from it.

In all likelihood, you don't even need to do anything at all, because this DotMarks identifies an item as a Stash by checking `IsInvbox(nil, obj:clsid())`--which is of course going to return `true` for any object parented to `inventory_box`.

And that probably means zero work for you. DotMarks will detect that it's an `inventory_box`, and automatically set up all the args just the way they need to be.

But let's say the autodetection doesn't work properly, and your item is getting detected as a member of--for example--the `Tools` item class instead of `Stashes`. How to solve that?

#### Handling custom items: DLTX the Section Lookup to force item classification
The easiest way is to add it to the section lookup with DLTX. Just create a new file in `configs\scripts`, and call it `mod_dotmarks_defaults_myname`. Then, assuming the section name of your item is `my_weird_backpack`, put this in the file:
```ini
![section_lookup]
my_weird_backpack = Stashes
```
And from then on, DotMarks will always treat that item as a Stash, and ignore all other checks.

#### Handling custom items: DLTX the Name Lookup to force a specific name string
Okay, so now it's being detected as a stash, but the item is showing the incorrect name on the prompt. It's supposed to be the localization of `st_my_item_name`, but instead it's showing just the literal string "st_my_item_name", or perhaps an object name like `my_weird_backpack24869`.

This can be solved very easily by adding this block to the `mod_dotmarks_defaults_myname` file you created:
```ini
![names_by_section]
my_weird_backpack = st_my_item_name
```
From then on, DotMarks will always use that localization string.
#### Handling custom items: the `dotmarks_on_new_object_scan` callback
Except that still misses one important detail, doesn't it? The fact that your new stash is a deployable item, like a backpack stash. That means that not only will its prompt text be wrong, it won't have the correct secondary action.

That's because sometimes just patching the name or item class and using the default args for the rest isn't enough. DotMarks detects a backpack stash by checking whether its section name is `inv_backpack`--and that's obviously the vanilla item, not yours. If you want your Stash object to be treated like a backpack, you need to manually add that `is_backpack = true` attribute to the list of args.

And the simplest way would be to use the `dotmarks_on_new_object_scan` callback that we briefly talked about above. You register it exactly as you would any other callback in the game:
```lua
function on_game_start()
	RegisterScriptCallback("dotmarks_on_new_object_scan", my_addon_handler_function)
end
```
Using the callback is going to skip the autodetection steps--so if we don't want to have to set up all the args ourselves, we need to tell it what the item's classification is.

![NOTE]
> It's not strictly necessary to set the `item_class` if you're setting all of the args yourself--however, if you don't pass an `item_class`, then you must pass a `texture` instead so that DotMarks knows what icon to use for the marker. Additionally, the item's class is used to control whether the marker is visible or hidden based on the per-class settings in MCM.

Passing back the item class will ensure that your backpack is being correctly detected as a Stash object, but now you need to flag is as a backpack. To do that, you'd put the following in your handler function:

```lua
function my_addon_handler_function(id, args)
	if args and args.section == "my_weird_backpack" then
		args.item_class 	= "Stashes"
		args.is_backpack 	= true
	end		
end
```
And that's it! That's all DotMarks needs in order to know to handle your new backpack item properly. It will pull the default args for Stashes, then add your flag to them.

#### Handling custom items: using hint filters
In DotMarks, a "hint filter" is a marker customization that is used to recognize when the game is trying to display an interaction prompt that is associated with that marker--and suppress the vanilla text display.

The hint filter logic itself is completely handled by DotMarks, so all you need to do is tell it which localization strings you're "subscribing" to. For any ordinary pickup item that displays the default interaction prompts, it will be this:
```lua
args.hint_filters    = {
	["inventory_item_use"] = "inventory_item_use"
},
```
For stashes, you need at least one of these three:

```lua
args.hint_filters = {
    ["inventory_box_use"]       = "inventory_box_use",
    ["actor_inventory_box_use"] = "actor_inventory_box_use",
    ["st_search_treasure"]      = "st_search_treasure",
}
```
But if you've customized the hint text on your stash, or it has alternate (e.g. locked) states that for which it needs to display different text, then you'll need to tell DotMarks what string to subscribe to. And as you might've guessed, it's just this simple as adding it to your callback like this:
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
#### Further customization: focus icons
A **focus icon** is an alternate texture that is swapped in whenever a marker has focus, and thus becomes active--that is, it is the player's primary target, the one with which any interaction would take place.

The marker system does this using the `active_swap` arg--which contains a subtable of args that are swapped when focus is gain, and reverted back to the original values when it is lost. In the orignal vanilla stash example above, it shows the line that would be used for the default stash icon:
```lua
active_swap 	= dup_table(dotmarks_main.cfg.swap_stash)
```
> ![WARNING]
> Don't directly reference the swap tables in the main config--make a copy of the table. Otherwise you risk altering the way that swap works for others.

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
