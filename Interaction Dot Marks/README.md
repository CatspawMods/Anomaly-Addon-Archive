# **Interaction Dot Marks**
A blatant imitation of the "interaction dot" HUD markers in Stalker 2, and some of its other UI niceties such as floating prompts and secondary interactions.

Except not quite as cool, because Anomaly is jank. Hopefully this makes it a little less jank.

Also implements a "Secondary Interact" keybind with contextual actions such as unloading a weapon without having to pick it up, or taking everything from a stash without opening the loot window.

Requires **MCM 1.7.0** and the **20250306 modded exes** by demonized, or (better yet) the latest of both:
* [Demonized modded exes (Github)](https://github.com/themrdemonized/xray-monolith)
* [Mod Configuration Menu (Moddb)](https://www.moddb.com/mods/stalker-anomaly/addons/anomaly-mod-configuration-menu)
> [!CAUTION]
> If your exes are older than that, a lot of the addon probably won't work very well--if at all. You should update your modded exes **ASAP**.
>
> This addon *will not work at all* on vanilla Anomaly, and may crash spectacularly if you try.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)


# Personalization
Everyone likes different things. "Play your own way" is axiomatic to me.

If your issue is that you don't like the way something looks or works, start by checking out the MCM menu under **DotMarks**. It's packed with customization options that will let you selectively disable or tweak just about everything in the addon. And you'll need the **Advanced** menu there if you're going to report an issue.

Everything in DotMarks is designed to be customizable. Nearly every individual part of the interaction UI can be enabled, disabled, or moved around. Every animation, every icon, every sound, every feature, every color. It's your UI--make it suit your preferences.

Just about any setting that would actually be useful to most users is available to you in MCM. However, there are also a large number of "under the hood" settings in the `configs\scripts\dotmarks_defaults.ltx` file--most of which are not in MCM. 
> [!CAUTION]
> For many of them, there's a very good reason for that! Don't touch anything below the line where it says not to. Everything customizable is above that line and well-commented, and most of what is useful to anyone but me is already in MCM.

> [!NOTE]
> If a setting is in MCM, don't try editing it in the config file--those values are treated as defaults, and your changes will get overwritten when MCM loads its settings.

In the list of MCM settings that follow, the "Config setting" lines refer to that setting's variable name under the `[default_settings]` section in the above file, and the "Default" is its default value defined there.

# MCM Menu: General Settings
## Show item quick info card
* Config setting: `show_item_card`
* Default: `true`

If enabled, a small display will be shown beside a pickup item's primary interaction prompt, showing its weight and value and (optionally, with *Utjan's Item UI Improvements* addon) indicator dots for the condition of weapon/armor parts.
## Show device battery percentage
* Config setting: `show_item_charge`
* Default: `true`

If enabled, the battery charge of devices that use power (or the batteries themselves) will be appended to the name of the item.
## Show quantity for multi-use items
* Config setting: `show_multi_uses`
* Default: `true`

If enabled, the number of uses in a multi-use item will be displayed beside the prompt when more than one remains.
## Show icon for favorites/junk on the item card
* Config setting: `sp_flag_favejunk`
* Default: `true`

If enabled, items marked as Favorites or Junk with *Sorting Plus* will display the appropriate icon on the item card.
> [!IMPORTANT]
> Requires the *Sorting Plus* addon.
## Show gear condition percentage
* Config setting: `show_item_condition`
* Default: `true`
 
If enbabled, the condition of weapons, outfits and headgear will be appended to the name of the item.
> [!NOTE]
> This option is disabled by default in GAMMA, which does not use item condition.
## Show condition/charge level in color
* Config setting: `show_condition_color`
* Default: `true`

If enabled, the color of the condition/battery text will change based on the value, using the same colors as the base game.
> [!NOTE]
> This feature may have a very minor performance cost on low-end machines. You can try disabling it for a tiny gain, if any, when there are many nearby pickup items that use condition or battery.
## Show total weight in containers
* Config setting: `show_stash_weight`
* Default: `false`

Totals up the weight of all items within marked containers, appending this information to the prompt. If a searched container is empty, it will say that instead.
> [!WARNING]
> This feature is disabled by default because it is known to have a performance cost on some machines. If you experience stutter or FPS loss when there are many stashes, dead stalkers, or other containers nearby, try disabling this setting.
## Mouse wheel cycles focused pickup
* Config setting: `wheel_cycles_pickups`
* Default: `true`

If enabled, when there is more than one nearby item that can be picked up, scrolling the mouse wheel will cycle focus through the nearby items instead of its usual function. By default, only the focused item will receive interaction.
> [!NOTE]
> If FDDA Redone's multi-pickup option is disabled, DotMarks will try to respect this by only allowing the focused item to be picked up--but on rare occasions that isn't possible.
## Main drop shadow transparency
* Config setting: `interact_drop_alpha`
* Default: `true`

Sets the alpha (transparency) channel for the main drop shadows underneath the interaction prompts or other UI elements.
> [!TIP]
Set to zero if you want to disable the main drop shadows entirely.
# MCM Menu: Secondary Interact
The Secondary Interact provides context-sensitive actions on a second prompt just below the Primary. If you have FDDA or FDDA Redone installed, and there are animations associated with those actions, they will play out just as if you'd taken the item and then immediately used it.

Available actions include:
* Eat, drink, smoke, or otherwise use any consumable without having to pick it up as a separate action
* Take everything from a stash or body at once--and if that stash is a backpack, pick it up, too
* Unload and take the ammunition from any weapon
* Remove and take the battery from any device
## Keybind, Modifier, and Input Mode for Secondary Interactions
* Config settings: `bind_sec_interact`, `modk_sec_interact`, `imod_sec_interact`
* Allowed Modifiers: Shift, Control, Alt
* Allowed Input Modes: Simple Press, Long Press, Double Tap

These three separate settings define the key combination used to activate the Secondary Interact.

By default it is set to a long-press equivalent of your Primary Interact--in other words, your "use" key. But if you're like me and you absolutely *hate* long-press or double-tap actions, don't worry--you can set it to whatever you like.
> [!NOTE]
> The durations for Long Press and Double Tap actions are set for all addons in `MCM > MCM Keybinds`

> [!WARNING]
> While having the Secondary Interact as a variant of the same key used for the Primary is convenient, this requires the script to keep close track of--and sometimes block--the pressing of the "use" key. On very rare occasions this can interfere with normal pickup item interactions. If you have issues like that, try setting the Secondary Interact to a different key.
## Long Press Delay
* Config setting: `long_press_delay`
* Default: `1000`

When a long-press action is completed, inputs from that same key will be ignored for this many milliseconds. This is necessary to prevent the same key from incorrectly registering as a simple press just after completing the previous action--a person cannot lift their finger off the key instantaneously.
> [!TIP]
> If you find that your primary interact isn't registering when trying to quickly pick up multiple items, you can try lowering this value. It must be between 50 and 2000.
# MCM Menu: Icon Settings
The default "dot marker" is a small white dot with a thin black outline. When a marker has **focus**--that is, its Primary Interaction prompt is shown--the icon changes to an alternate *focus icon*. By default, this is a smaller white dot with a ring around it.

However, many different types of objects have their own contextual focus icons to indicate their purpose or the nature of the interaction. In this menu you can individually enable or disable those icons to suit your preferences, or customize their size and color.

The default color for all icons is ARGB 255,255,255,255--where the "A" stands for Alpha, or transparency, while the "RGB" values are the Red, Green, and Blue color channels, in that order.
> [!TIP]
> When the RGB values are all 255, the color is white--and a texture will have its default color. Setting all values to 0 will turn it completely black.

All of the ARGB settings function the same: clicking the **button with the gear icon** will open the ARGB control, and the fields will become editable. Changing any value will show a preview of the chosen color on the focus icon itself. Clicking the **checkmark button** will send your changes to MCM, which will take effect once you commit MCM's settings. Clicking the **X-shaped icon** will cancel all changes in progress.
> [!TIP]
> If you really make a mess of things, don't worry--look in the lower left of MCM for a button that lets you reset settings to default.
## Interaction Dot Marker size
* Config setting: `dot_marker_size`
* Default: `1.0`

Scales the size of all markers between 0 and 2.0, which acts as a multiplier to their default size.
> [!TIP]
> You can set this to zero in order to hide all markers. It will have essentially the same effect as enabling `hide_interaction_dots`.

> [!NOTE]
> Markers are scaled by distance, so in-game they usually will appear slightly smaller than they do here.
## Customize color for the normal dot marker
* Config setting: `argb_dot_normal`
## Enable special icons for task objectives
* Config setting: `enable_icon_tasks`
* Default: `true`

If enabled, objects that are the target of the current active task will use a high-resolution task marker icon instead of the standard dot markers. The marker will be gold or white depending on whether or not it is a story task, just as in vanilla.
> [!NOTE]
> Like every other focus icon on this page, you won't see these special icons if you've hidden the dot markers entirely--these are just alternate textures that get swapped in based on the marker's focus state.
## Enable special icons for service NPCs
* Config setting: `enable_icon_services`
* Default: `true`

Certain NPCs provide services to the player: Traders, Technicians, Medics, Guides, and Faction Leaders. While the default focus icon for most stalkers is a speech bubble, these *Service NPCs* each get their own special icon. Disabling this setting will cause them to use the normal dot marker instead.
## Customize color for the Trader icon
* Config setting: `argb_service_trader`
## Customize color for the Guide icon
* Config setting: `argb_service_guide`
## Customize color for the Leader icon
* Config setting: `argb_service_leader`
## Customize color for the Technician icon
* Config setting: `argb_service_mech`
## Customize color for the Medic icon
* Config setting: `argb_service_medic`
## Object focus icons
The rest of the **Icon Settings** menu consists of pairs of settings for each icon: a checkbox for whether it is enabled, and an ARGB control for customizing its color. All of them default to enabled.

The following special icons are available here:
## NPCs with dialogue
* Config settings: `enable_icon_talk`, `argb_icon_talk`
> [!TIP]
> By default, you won't see markers at all for stalkers who can't engage in conversation. If you'd rather still see dot markers for them, turn off the `hide_mute_stalkers` setting in the Advanced menu.
## Stashes
* Config settings: `enable_icon_stash`, `argb_icon_stash`
## Doors
* Config settings: `enable_icon_door`, `argb_icon_door`
## Prompt to skin mutant
* Config settings: `enable_icon_butcher`, `argb_icon_butcher`
## Breakable boxes
* Config settings: `enable_icon_box`, `argb_icon_box`
## Explosive objects
* Config settings: `enable_icon_boom`, `argb_icon_boom`
> [!NOTE]
> Breakable boxes and explosive objects do not have interaction prompts, and so do not receive focus--their special icon replaces the dot marker at all times.

# MCM Menu: Object Settings
The settings in this menu control whether or not markers are added to each different type of object that DotMarks recognizes. If any are distracting or less useful to you, turning off their setting will hide their visible markers entirely.

All of these are enabled by default with the exception of **Breakable Boxes** and **Explosive Objects**.
> [!TIP]
> As with the other two methods of hiding markers, objects with hidden markers will still show interaction prompts normally, which is what you will almost always want--but if you prefer otherwise, you can turn off the `hidden_show_prompts` setting.

There is a checkbox for each of the following object classes:
## Ammo 
* Config setting: `scan_ammo`

Ammunition of any kind, with the exception of explosives.
## Artefacts
* Config setting: `scan_artefacts`

Any artefact, whether or not it in a container.
## Attachments
* Config setting: `scan_attachments`

Weapon and Outfit attachments.
## Bodies
* Config setting: `scan_bodies`

Dead bodies that contain something--whether mutant or stalker. Mutant corpses that have decayed or been looted cannot be interacted with, and thus will not have a marker.
> [!TIP]
> If you use my *Milspec PDA* addon, the `bodies_use_mpda_rules` setting will cause body markers to use Milspec PDA's visibility rules--meaning that you'll only see a marker if your PDA allows you to see them on your map.
## Breakable boxes
* Config setting: `scan_boxes`

Any breakable box that has a chance to contain loot, whether wood or metal.
## Consumables
* Config setting: `scan_consumables`

Food, drink, drugs, or anything else that gets consumed when used.
## Devices
* Config setting: `scan_devices`

Equippable devices that take batteries.
## Doors
* Config setting: `scan_doors`

Interactable doors that could be opened, whether or not they are currently locked. This includes small furnishing doors like cabinets and lockers.
## Explosive Objects
* Config setting: `scan_explosives`

Any static physics object that will explode when damaged or triggered. This includes volatile canisters and barrels, but also IEDs and other placed explosives.
> [!NOTE]
> The detection of this item type is based on whether the object, by section, has a `blast` attribute with a value. Some pickup items (e.g. the jerrycan) with a `blast` value are special cases classified instead as **Tools**.
## Grenades
* Config setting: `scan_grenades`

Hand grenade pickup items.
## Headgear
* Config setting: `scan_headgear`

Helmets and other items to equip in the head slot.
> [!TIP]
> If you have *Utjan's Item UI Improvements* addon, and their setting for showing part dots on armor is enabled, the pickup prompt for headgear items will also show those dots on the item card.
## Misc Loot
* Config setting: `scan_misc`

Miscellaneous objects that can be picked up, but which defy classification in any other category.
> [!NOTE]
> The kinds of items covered under this umbrella are far too many to list here, but include things like faction patches, junk items, and various bits of crafting and repair components.
## Outfits
* Config setting: `scan_outfits`

Armor, jackets, and other items to equip in the body slot.
> [!TIP]
> If you have *Utjan's Item UI Improvements* addon, and their setting for showing part dots on armor is enabled, the pickup prompt for headgear items will also show those dots on the item card.
## Quest Items
* Config setting: `scan_quest`

In addition to any object flagged by the game as a Quest Item--inventory items which can't be dropped or traded--this category also includes a small handful of buttons and other interactive quest objects, most of which are located in the X-Labs.
## NPCs
* Config setting: `scan_stalkers`

Any living stalker. NPCs from a faction that is currently an enemy of the actor will not show a visible marker, and dead stalkers are handled by the **Bodies** category.
> [!NOTE]
> The determination is based on the actor's current faction, not their true faction--meaning it will change in response to disguises.
## Stashes
* Config setting: `scan_stashes`

Any container classified by the game as an "inventory box", which includes virtually all stashes.
> [!NOTE]
> The built-in stash in the Placeable Workbench from *Hideout Furniture* is ignored, as it is accessed only through the workbench.
## Tools
* Config setting: `scan_tools`

Repair, crafting, and maintenance tools of various kinds.
> [!NOTE]
> Most placeable items from the *Hideout Furniture* addon will be in this category while they are inventory items, but will be reclassified once placed.
## Weapons
* Config setting: `scan_weapons`

Any equippable weapon, whether melee or ranged.
## Workbenches
* Config setting: `scan_workbenches`

Interactive workbenches, most of which are owned by a Technician.
> [!NOTE]
> This includes the Placeable Workbench from the *Hideout Furniture* addon, once it has been placed. The HF version will show a secondary interaction prompt for opening its control menu.
# MCM Menu: Other Addons
DotMarks is designed to work hand-in-hand with a variety of popular addons, and many of those integrations have options which you can adjust in this menu.

Because DotMarks detects objects based on many of the same fundamental attributes the game itself uses to classify items, it will support many other addons right out of the box, as long as they define their own items in a way consistent with the vanilla definitions.
> [!NOTE]
> But there are always going to be exceptions and conflicts sooner or later. If that happens, see the Troubleshooting section.
## Dead stalkers use PAW faction patches
* Config setting: `bodies_use_paw_patches`
* Default: `false`
If my *Personal Adjustable Waypoint* addon is installed, enabling this option will cause dead stalkers to show their faction patch icon instead of the usual dot markers.
## Body markers obey Milspec PDA rules
* Config setting: `bodies_use_mpda_rules`
* Default: `false`
If my *Milspec PDA* addon is installed, enabling this option will cause bodies to be marked--or not--depending on whether the actor's PDA has the ability to see them on the map.
> [!TIP]
> If corpse markers are disappearing, or not appearing when you expect them to, check whether this setting is on--as well as Milspec PDA's own MCM settings, which are setting the terms for visibility.
## Show interaction prompt during FDDA animations
* Config setting: `show_prompt_during_anim`
* Default: `false`

If enabled, the interaction prompt will be shown during FDDA animations.
> [!NOTE]
> If you're using FDDA Redone, there isn't much point to enabling this--see below.
## Interaction prompt disabled during FDDA animations
* Config setting: `prompt_busy_during_anim`
* Default: `true`

If enabled, the interaction prompt will not accept input during FDDA animations.
> [!NOTE]
> This only really affects the original FDDA, since FDDA Redone blocks the use key during animations.
## Item secondary usage delay
* Config setting: `item_use_delay`
* Default: `0.5`

When using a secondary action on a pickup item, this is the delay in seconds between the pickup event and the queueing of the secondary action. This is necessary in order to allow FDDA's pickup animation time to begin.
> [!NOTE]
> This setting has no effect without FDDA or FDDA Redone installed.
## Take-all secondary action triggers Autoloot
* Config setting: `takeall_does_autoloot`
* Default: `true`

If enabled along with iTheon's *Auto Looter*, the secondary action for containers will trigger the Auto Looter function instead of Take all. Once a container has been autolooted, the secondary action for that container will change to "Take all".
## Override Auto Looter keybind
* Config setting: `hijack_autoloot_keybind`
* Default: `true`

If both of these Auto Looter options are enabled, DotMarks will attempt to take control of the Auto Looter key bind, and will disable the usual one. This not only simplifies the integration of the two addons, but allows you to unbind the Auto Looter bind and free up the key for something else.
> [!NOTE]
> When this option is enabled, you can't trigger the Auto Looter manually using its keybind--the function is moved to the DotMarks secondary action. However, most players will never need to Autoloot the same container twice.
## Hide items marked as junk in Sorting Plus
* Config setting: `sp_hide_junk_items`
* Default: `true`

If the *Sorting Plus* addon is installed, and an item has been marked as "junk", then a marker will not be shown for it.
> [!TIP]
> Just like any other hidden marker, you can still interact with and pick up the item.
## Sorting Plus icon X and Y offsets
* Config settings: `sp_icon_pos_x`, `sp_icon_pos_y`
* Default: `10, 1`

Fine-tunes the position of the Sorting Plus icon on the Item Card.
## Part condition dots X and Y offsets
* Config settings: `parts_dot_pos_x`, `parts_dot_pos_y`
* Default: `-7, -4`

Fine-tunes the position of the part condition dots on the Item Card.
## Increase perception radius based on Skill System
* Config setting: `use_skill_system`
* Default: `true`

If Haruka's *Skill System* addon (or one of its derivatives) is installed, the `near_scan_radius` will be slightly increased for each level in a specified skill.
## Scan radius multiplier per skill level
* Config setting: `haru_skill_coef`
* Default: `0.2`

If *Skill System* integration is enabled, this setting controls the multiplier that is applied to the `near_scan_radius` per skill level, and effectively increases the player's "perception" range--the distance at which dot markers begin to appear.

At **Scavenging** level 15 with the default multiplier of 0.2 and radius of 4m, the final near scan radius would be 7 meters.
> [!WARNING]
> A very high near scan radius can have a performance impact, especially in dense areas with many objects.
## Interval between Skill System updates
* Config setting: `skill_upd_interval`
* Default: `29711`

Time in milliseconds between updates. You'll probably never have a need to change this.
> [!NOTE]
> Skill level-ups occur very rarely, so the skill system update does not need to happen often--you may go entire play sessions without a level-up, and even the default value of ~30s is arguably overkill.
## Name of the skill used for this check
* Config setting: `haru_skill_name`
* Default: `scavenging`

The internal name of the skill from *Skill System* that is used as the basis for the integration.
> [!WARNING]
> Bad things may happen if you typo this, and most of you have no need to tinker with it.
# MCM Menu: Advanced Settings
.

.
> [!WARNING]
> The Advanced menu is very lengthy, and documenting it properly is going to take some time. So consider this your "work in progress" warning for this section.

.

.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# Troubleshooting
I've tried to set up safety checks that display a message to the player a few moments after they first load into the game if a dependency is missing. But in case that fails, if something isn't working right, there's a good chance that you need to properly update something.

Here are the most common issues people have, and how to solve them:

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Can't pick up anything that isn't centered in the crosshair, or `actor_on_update_pickup` error seen in log

If your version of the modded exes is earlier than `20250306`, you will see this error in your log:

`![axr_main callback_set] callback actor_on_update_pickup doesn't exist!`

If you are seeing that, you **MUST** update your version of the modded exes in order for object targeting to work properly. Otherwise the addon has to fall back on a vanilla targeting method that requires the crosshair to be *exactly* centered on the object, and you may have trouble interacting with some things.

The reason is that new callbacks are defined in a file that is contained in one of the DB files in the modded exes installation. Older versions may have loose versions of the files that are now contained in the DB, which override the new ones and cause things to break.

If you just update the binaries but somehow still have old DB files (or something is overwriting them), then some functions may *appear* to work, but the newly-added callback won't exist--so DotMarks has to rely on finicky, much less accurate methods of detecting the target object. Anything dependent on that callback, such as proper targeting and cycling through nearby pickups with the mouse wheel, won't work well if at all.

I repeat, and let me be completely clear:

### :exclamation: **IF YOU ARE SEEING THAT ERROR IN YOUR LOG ABOUT THE MISSING CALLBACK, YOUR MODDED EXES INSTALLATION IS STILL NOT CURRENT.** :exclamation:
### :exclamation: **EVEN IF YOU HAVE TRIED TO UPDATE THEM, THAT ERROR MEANS YOU MUST REINSTALL THEM FROM SCRATCH ACCORDING TO THE INSTRUCTIONS.** :exclamation:

I'm sorry to be so blunt and shouty, but I've been testing it with users for a few months now, and the vast majority of the bug reports I get come down to the above issue.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Empty icons where key binding names should be; MCM menus are absent, broken or missing content

You **MUST** update your MCM version to 1.7.0 or higher.

Some modpacks have embedded older versions of MCM, and some have even edited their copy of MCM to bypass Anomaly version checks. Don't do that, please. The version should be [somewhere around here](https://github.com/RAX-Anomaly/Anomaly-Mod-Configuration-Menu/blob/b02d0ff127181e698fa2c03fd434badb278211c9/gamedata/scripts/ui_mcm.script#L92) in your copy of `ui_mcm.script`, and it must be `1.7.0` or higher.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Poor performance / FPS loss / stutter
The setting that shows the total weight within stashes and other containers can cause an FPS loss in bases with many containers, or when there are many lootable bodies nearby. You can turn this off (or customize many other things) in the DotMarks MCM menu.

Beyond that, while I am continually working to optimize it, understand that this addon is *completely replacing most of Anomaly's interaction UI*. It needs to keep track of all nearby objects at all times so that it can know when to show interaction prompts. There is going to be a certain minimum amount of overhead involved in doing that much work, meaning that optimization can only go so far.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Auto Looter keybind doesn't work
Go to MCM > DotMarks > Other Addons and disable [hijack_autoloot_keybind](#override-auto-looter-keybind).

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Still having problems?
### 1. GENERATE A CLEAN DEBUG LOG
If you need to report a problem, first do this:
    1. Go to MCM > DotMarks > Advanced
    2. Enable both Debug and Verbose logging
    3. Exit the game completely and restart it
    4. Load a save game where the issue occurs, and reproduce it as simply as possible
    5. Exit the game immediately
### 2. CHECK DEPENDENCIES
Before you post the log anywhere, open it in a text editor and try searching for the string `Checking for dependencies`. You should find a block in the log just below it that looks like this:
```
[DotMarks] [V] config loaded: true
[DotMarks] [V] using_modded_exes: true
[DotMarks] [V] no_callback_old_exes: false
[DotMarks] [V] no_callback_old_dbs: false | found in _G true | callback firing true
[DotMarks] [V] no_lookat_method: false
[DotMarks] [V] bad_mcm_version: false
[DotMarks] [V] no_hud_mark_manager: false
```
At least, it *should* look like that. If any of those true/false values are different, something is wrong and you need to properly reinstall/update the offending component.
### 3. REPORT THE ISSUE
If all of those are correct and something's still busted, please take the short DEBUG LOG from the instructions above--the whole thing--and post it somewhere I can see it, along with as much information as possible about the issue. If the issue is visual, include a screenshot of what looks wrong.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)
