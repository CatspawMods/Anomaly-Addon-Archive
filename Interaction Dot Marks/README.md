# **Interaction Dot Marks ("DotMarks")**
![DotMarks Title](packing/fomod/images/dotmarks-fomod.png)
----
A blatant imitation of the "interaction dot" HUD markers in Stalker 2, and some of its other UI niceties such as floating prompts and secondary interactions.

Except not quite as cool, because Anomaly is jank. Hopefully this makes it a little less jank.

Also implements a "Secondary Interact" keybind with contextual actions such as unloading a weapon without having to pick it up, or taking everything from a stash without opening the loot window.
> [!TIP]
> Having difficulties? Check out the [Troubleshooting](#troubleshooting) section.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# Requirements
## [The demonized modded exes (Github)](https://github.com/themrdemonized/xray-monolith), release 20250306 or newer
You can always get the latest version at the link above, as well as find instructions (which you should follow!) for performing a clean installation.
> [!CAUTION]
> If your exes are older than that, or have outdated components left over from an older version, a lot of the addon probably won't work very well--if at all. You should perform a clean install of your modded exes **ASAP**.
>
> DotMarks *will not work at all* on vanilla Anomaly, and may crash spectacularly if you try.
## [Mod Configuration Menu (Moddb)](https://www.moddb.com/mods/stalker-anomaly/addons/anomaly-mod-configuration-menu), version 1.7.0 or newer
You can also download it from the [Github page for MCM](https://github.com/RAX-Anomaly/Anomaly-Mod-Configuration-Menu/tree/main).
> [!CAUTION]
> Some modpacks, such as Anomaly Custom, use a version of MCM that they have hacked with hardcoded changes. Making a modpack dependent on an altered version of a core component like MCM means that you cannot upgrade it to a newer version, and this kind of installation is not supported by DotMarks.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# Strongly Recommended
These other fantastic addons are not required, but DotMarks is designed to integrate with them and extend its functionality with theirs--or vice-versa. Click the title of each to go to their Moddb pages.
> [!TIP]
> All of these features either respect the relevant MCM settings of each addon, or have their own settings in the DotMarks MCM menu to configure or disable them.
## [FDDA or FDDA Redone](https://www.moddb.com/mods/stalker-anomaly/addons/fdda-redone)
Secondary actions--for example, using consumables--will automatically hand the action and animation off to FDDA or FDDA Redone. The latter is recommended, but either will work.
> [!NOTE]
> Even if you don't normally use either version of FDDA, I recommend giving this a try. It's hard to overstate how immersive it is to see your character pick up an item and immediately begin using it, all without entering your inventory or any other menu.
## [Animated NPC Healing](https://www.moddb.com/mods/stalker-anomaly/addons/animated-npc-healing10)
If this addon is installed, DotMarks will use its excellent animation for the heal-an-NPC secondary action.
## [Artifacts Inspection](https://www.moddb.com/mods/stalker-anomaly/addons/artifacts-inspectionv10)
If this addon is installed, DotMarks will add a secondary action for artefacts to display their inspect animations.
## [Sorting Plus](https://www.moddb.com/mods/stalker-anomaly/addons/sorting-plus)
With Sorting Plus, any items on the ground that you've marked as Favorites or Junk will show an appropriate icon on the Item Card.
## [Utjan's Item UI Improvements](https://www.moddb.com/mods/stalker-anomaly/addons/utjans-item-ui-improvements)
DotMarks will show Utjan's part condition dots on the Item Card if it detects the addon.
> [!NOTE]
> _Sorting Plus_ and _Utjan's Item UI_ are already part of GAMMA--there is no need to download them if you are a GAMMA user.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# Installation / Uninstallation
**Interaction Dot Marks does not write anything to your save file. It is safe to install or uninstall mid-playthrough at any time.**
## Install Instructions
Use Mod Organizer 2. It's the only one supported. Others may work, but I can't help if they don't.
1. Click on File > Install Mod... (Ctrl-M)

![MO2 File Menu > Install Mod... (Ctrl-M)](https://i.imgur.com/8J0KBFn.png)

2. Navigate to where you downloaded the Interaction Dot Marks archive
3. Proceed through the steps of the FOMOD installer
## Install Options
### Hide floating inventory icons
In vanilla Anomaly, floating inventory icons appear on the HUD to identify the primary object that is targeted for pickup. DotMarks looks better without these icons, and this option hides them.
## Addon Support Options
### Animated NPC Healing
If you use the Animated NPC Healing addon, this patch allows DotMarks to trigger that animation when using a secondary action to heal a wounded stalker.
### Anomaly Popup Messages fix
If you use the Anomaly Popup Messages addon, this patch prevents APM from displaying duplicate notifications for item pickup.
### Auto Looter bugfix and compatibility
If you use the Auto Looter addon, this patch allows autolooting as a secondary action in place of the take-all function. It also fixes a longstanding bug in Auto Looter where it does not send the actor_on_item_take_from_box callback when remote looting.
### Fillable Canteen Water Pumps
If you use the Water Pumps feature from the Fillable Canteens addon, this will allow DotMarks to detect those objects and handle them correctly.
### Skill System compatibility
If you use the Skill System addon, this patch allows DotMarks to know when the Scavenger skill has leveled up. Without it, DotMarks will only update this value when a map or savegame loads.
> [!TIP]
> It's safe to accept all of the installer's defaults if you don't have a preference. All default options have been designed for broadest compatibility--if an addon it's patching isn't present, then the patch will automatically disable itself, with no other effect. The options are only there in case anyone needs to disable them for any reason.
## Uninstall Instructions
Either uncheck DotMarks in your MO2 modlist so that it is no longer active, or right-click on it and select `Remove mod...` to uninstall the whole thing.

If you're not using MO2, you're _completely_ on your own.
> [!TIP]
> But if your issue is that you don't like the way something looks or works, check out the [Personalization section](#personalization) before uninstalling--the MCM menu that comes with DotMarks is packed with options that will let you fine-tune the experience to your liking. And you'll need the [Advanced menu](#mcm--dotmarks--advanced-settings) there if you're going to [report an issue](#still-having-problems).

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# Personalization
**Everyone likes different things. "Play your own way" is axiomatic to me.**

DotMarks is designed to let you selectively personalize just about everything in the addon. Every part of the object interaction UI can be enabled, disabled, or moved wherever you want it to be on the screen. Every animation, every icon, every sound, every feature, every color.

* Want the new prompts and secondary actions, but don't want your HUD cluttered by stuff like the dot markers? [Turn them off](#hide-interaction-dot-markers).
* Want those prompts to stay at the bottom of the screen like vanilla, instead of following the object? Turn on [fixed screen position](#use-fixed-screen-positions), and adjust the [primary and secondary prompts](#horizontal-offset-for-first-interact) to where you want them.
* Have an ultrawide or high-resolution monitor, and need the [font](#font-settings) or [UI elements](#scale-gui-elements) to be bigger? You can do that.

It's your UI--make it suit your preferences.

Nearly any setting that would actually be useful to most people is available in the MCM menu, which you can reach from the game's main menu by going to **MCM > DotMarks**. It will always be named that, regardless of language.

However, there are also a large number of "under the hood" settings in the `configs\scripts\dotmarks_defaults.ltx` file--most of which are not in MCM. 
> [!CAUTION]
> For many of them, there's a very good reason for that! Don't touch anything below the line where it says not to:
> ```
> ; 	DON'T TOUCH BELOW THIS LINE
> ```
> It means it. Everything customizable is above that line and well-commented, and most of what is useful to anyone but me or another addon author is already in MCM.

In the list of settings that follow, the "Config setting" lines refer to that setting's variable name under the `[default_settings]` section in the above file, and the "Default" is its default value defined there. Many have helpful tips about their usage, or important things to keep in mind when changing them.
> [!IMPORTANT]
> There's also a good reason why the file is named `dotmarks_defaults`. The values there are only baseline defaults, and MCM will override them with any values saved in `configs\axr_options.ltx`--every time it loads. If a setting is in MCM, it's quicker and easier to test changes there anyway.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# MCM > DotMarks > General Settings
![General Settings banner](https://i.imgur.com/OXAK0xS.png)

Nothing but the basics. In this menu you can enable or disable most of the core UI elements in DotMarks, or set the transparency of the main drop shadow.
## Show device battery percentage
* Config setting: `show_item_charge`
* Default: `true`

If enabled, the battery charge of devices that use power (or the batteries themselves) will be appended to the name of the item.
## Show gear condition percentage
* Config setting: `show_gun_condition`
* Default: `true`
 
If enbabled, the condition of weapons will be appended to the name of the item.
> [!NOTE]
> This option is disabled by default in GAMMA, which does not use condition for guns.
## Show gear condition percentage
* Config setting: `show_item_condition`
* Default: `true`
 
If enbabled, the condition of outfits and headgear will be appended to the name of the item.
## Show condition/charge level in color
* Config setting: `show_condition_color`
* Default: `true`

If enabled, the color of the condition/battery text will change based on the value, using the same colors as the base game.
> [!NOTE]
> This feature may have a very minor performance cost on the most potato of low-end machines. You can try disabling it for a tiny gain, if any, when there are many nearby pickup items that use condition or battery.
## Show total weight in containers
* Config setting: `show_stash_weight`
* Default: `false`

Totals up the weight of all items within marked containers, appending this information to the prompt. If a searched container is empty, it will say that instead.
> [!WARNING]
> This feature is disabled by default because it is known to have a performance cost on some machines. If you experience stutter or FPS loss when there are many stashes, dead stalkers, or other containers nearby, try disabling this setting.
## Show item quick info card
* Config setting: `show_item_card`
* Default: `true`

If enabled, a small display will be shown beside a pickup item's primary interaction prompt, showing its weight and value and, optionally (with the respective addons installed), icons for favorites and junk, as well as indicator dots for the condition of weapon/armor parts.
> [!TIP]
> Disabling the item card will hide all of the elements it contains. The part condition dots and fave/junk icons have their own MCM options if you only want to selectively disable one or both of those elements.
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
## Mouse wheel cycles focused pickup
* Config setting: `wheel_cycles_pickups`
* Default: `true`

If enabled, when there is more than one nearby item that can be picked up, scrolling the mouse wheel will cycle focus through the nearby items instead of its usual function. By default, only the focused item will receive interaction.
> [!NOTE]
> If FDDA Redone's multi-pickup option is disabled, DotMarks will try to respect this by only allowing the focused item to be picked up--but on rare occasions that isn't possible.
## Main drop shadow transparency
* Config setting: `interact_drop_alpha`
* Default: `0.7`

Sets the Alpha (transparency) channel for the main drop shadows underneath the interaction prompts or other UI elements.
> [!TIP]
Set to zero if you want to disable the main drop shadows entirely.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# MCM > DotMarks > Secondary Interact
![Secondary Interact banner](https://i.imgur.com/GgEwrVR.png)

The Secondary Interact provides context-sensitive actions on a second prompt that is located (by default) just below the Primary. If you have FDDA or FDDA Redone installed, and there are animations associated with those actions, they will play out just as if you'd taken the item and then immediately used it.

Available actions include:
* Eat, drink, smoke, or otherwise use any consumable without having to pick it up as a separate action
* Take everything from a stash or body at once--and if that stash is a backpack, pick it up, too
* Unload and take the ammunition from any weapon
* Remove and take the battery from any device
* Deploy or pick up an IED with any timer or detonation mode
* Heal a wounded NPC with the cheapest medkit in your inventory
* And more!
## Keybind, Modifier, and Input Mode for Secondary Interactions
* Config settings: `bind_sec_interact`, `modk_sec_interact`, `imod_sec_interact`
* Allowed Modifiers: Shift, Control, Alt
* Allowed Input Modes: Simple Press, Long Press, Double Tap

These three separate settings define the key combination used to activate the Secondary Interact.

By default it is set to a long-press equivalent of your Primary Interact--in other words, your "use" key. But if you're like me and you absolutely *hate* long-press or double-tap actions, don't worry--you can set it to whatever you like.
> [!NOTE]
> The durations for Long Press and Double Tap actions are set for all addons in `MCM > MCM Keybinds`.

> [!WARNING]
> While having the Secondary Interact as a long-press of the same key used for the Primary is convenient, this requires the script to keep close track of--and sometimes block--the pressing of the "use" key. On very rare occasions it can interfere with normal object interactions. If you keep having issues like that, you might need to try binding the Secondary Interact to a different key entirely.

> [!CAUTION]
> Don't try setting it to a double-tap of your primary use key, though. For any other key that's fine, but for the "use" key it will not work well.

## Long Press Delay
* Config setting: `long_press_delay`
* Default: `1000`

When a long-press action is completed, inputs from that same key will be ignored for this many milliseconds. This is necessary to prevent the same key from incorrectly registering as a simple press just after completing the previous action--a person cannot lift their finger off the key instantaneously.
> [!TIP]
> If you find that your primary interact isn't registering when trying to quickly pick up multiple items, you can try lowering this value. It must be between 50 and 2000.
## Enable "use item" action
* Config setting: `sec_enable_use_act`
* Default: `true`

Enables the secondary action that allows you to immediately use any consumable pickup item.
## Enable "unload weapon" action
* Config setting: `sec_enable_unload_act`
* Default: `true`

Enables the secondary action that allows you to unload and take the ammunition from a weapon on the ground.
> [!NOTE]
> If you have enabled an addon that forces stalkers to keep weapons in their inventory when they die, you will rarely ever see this action--weapons won't end up on the ground unless you're the one who dropped them.
## Enable "take all" action
* Config setting: `sec_enable_takeall_act`
* Default: `true`

Enables the secondary action that allows you to take everything from a container at once.
> [!TIP]
> If you have iTheon's Auto Looter addon installed, there is also an option in the Addons menu that allows you to autoloot the same way. It is enabled by default, but won't be shown if this setting is disabled.
## Enable "heal NPC" action
* Config setting: `sec_enable_heal_act`
* Default: `true`

Enables the secondary action that allows you to heal a wounded stalker on the ground.

Unlike the vanilla function, this will allow you to use any kind of medkit or stimpack you have--but it will always try to use the cheapest one, and (if [show_multi_uses](#show-quantity-for-multi-use-items) is enabled) it will show you on the prompt which one it will use, as well as how many uses you have remaining.
## Default IED placement mode
* Config setting: `sec_mode_setupthebomb`
* Default: `0` (proximity)

Sets the default placement mode for IEDs deployed using a secondary action, or disables that action to make it unavailable. Valid settings:
* `0`  : as a proximity mine
* `1`  : with a definable timer
* `2`  : with a remote detonator
* `-1` : disabled, action unavailable
> [!IMPORTANT]
> The remote detonator option requires the _Remote Controlled Explosives_ addon, or another which implements that functionality the same way.

> [!TIP]
> When targeting an undeployed IED pickup item, you can use the **Fire Mode** keybinds to cycle placement mode, and the mouse wheel to set the duration of the timer (if applicable).

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# MCM > DotMarks > Icon Settings
![Icon Settings banner](https://i.imgur.com/ea0PiDo.png)

The default "dot marker" is a small white dot with a thin black outline. When a marker has **focus**--that is, its Primary Interaction prompt is currently being displayed--the icon changes to an alternate *focus icon*. By default, this is a smaller white dot with a ring around it.

![Active Focus dot marker ](https://i.imgur.com/ZBVBqfN.png)

However, many different types of objects have their own contextual focus icons to indicate their purpose or the nature of the interaction. In this menu you can individually enable or disable those icons to suit your preferences, or customize their size and color for visibility, accessibility, or personal taste.

The default color for all icons is ARGB 255,255,255,255--where the "A" stands for Alpha, or transparency, while the "RGB" values are the Red, Green, and Blue color channels, in that order.
> [!TIP]
> When the RGB values are all 255, the color is white--and a texture will have its default color. Setting all values to 0 will turn it completely black.
>
> Alpha 255 is completely opaque, and 128 is about half-transparent. Setting any element's Alpha to zero will effectively hide it.

All of the ARGB settings function the same: 

* Clicking the **gold button with the gear icon** will open the ARGB control, and the fields will become editable.
* Changing any value will show a preview of the chosen color on the focus icon itself.
* Clicking the **green checkmark button** will send your changes to MCM, which will take effect once you commit MCM's settings.
* Clicking the **red X-shaped icon** will cancel all changes in progress.

> [!NOTE]
> You must press **Enter** after you finish typing the new value, just as with any other input box in Anomaly. If you simply click away from the input box, your changes will be lost.

> [!TIP]
> If you really make a mess of things, don't worry--look in the lower left of MCM for a button that lets you reset to default all of the settings on the current page.
## Interaction Dot Marker size
* Config setting: `dot_marker_size`
* Default: `1.0`

Scales the size of all markers between 0 and 2.0, which acts as a multiplier to their default size.
> [!NOTE]
> Markers are scaled by distance, so in-game they usually will appear slightly smaller than they do here.

> [!TIP]
> You can set this to zero in order to hide all markers. It will achieve essentially the same end result as enabling `hide_interaction_dots`.
## Customize color for the normal dot marker
* Config setting: `argb_dot_normal`
> [!TIP]
> Setting the Alpha for the normal dot marker to zero will hide only the unfocused dot markers, while leaving all others unaffected.
## Customize color for the focus dot marker
* Config setting: `argb_dot_focus`
> [!NOTE]
> The "focus" marker is the ringed dot shown when the actor's focus is placed on an object.
## Enable special icons for task objectives
* Config setting: `enable_icon_tasks`
* Default: `true`

![Primary Task icon](https://i.imgur.com/ScPq6Ki.png) ![Secondary Task icon](https://i.imgur.com/0WJXZMI.png)

If enabled, objects that are the target of the current active task will use a high-resolution task marker icon instead of the standard dot markers. The marker will be gold or white depending on whether or not it is a story task, just as in vanilla.
> [!NOTE]
> Like every other focus icon on this page, you won't see these special icons if you've hidden the dot markers entirely--these are just alternate textures that get swapped in based on the marker's focus state.
## Enable special icons for service NPCs
* Config setting: `enable_icon_services`
* Default: `true`

![Trader icon](https://i.imgur.com/kLLtO3P.png) ![Guide icon](https://i.imgur.com/NFYoIyr.png) ![Leader icon](https://i.imgur.com/soGD4Zb.png) ![Technician icon](https://i.imgur.com/r3te3QI.png) ![Medic icon](https://i.imgur.com/t59Kvis.png)

Certain NPCs provide services to the player: Traders, Guides, Faction Leaders, Technicians, and Medics. While the default focus icon for most stalkers is a speech bubble, these *Service NPCs* each get their own special icon. Disabling this setting will cause them to use the normal speech bubble icon instead.
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

![Talk icon](https://i.imgur.com/obsubKs.png) ![Stash icon](https://i.imgur.com/GaNWZcm.png) ![Door icon](https://i.imgur.com/bAHyDzu.png) ![Butcher icon](https://i.imgur.com/pZ0lduL.png) ![Box icon](https://i.imgur.com/OYzyb98.png) ![Boom icon](https://i.imgur.com/bo7D5xC.png)
## NPCs with dialogue
* Config settings: `enable_icon_talk`, `argb_icon_talk`
* Default: `true`
> [!TIP]
> By default, you won't see any markers at all for stalkers who can't engage in conversation. If you'd rather still see dot markers for them, turn off the [hide_mute_stalkers](#hide-marker-for-untalkative-stalkers) setting in the Advanced menu.
## Stashes
* Config settings: `enable_icon_stash`, `argb_icon_stash`
* Default: `true`
## Doors
* Config settings: `enable_icon_door`, `argb_icon_door`
* Default: `true`
## Prompt to skin mutant
* Config settings: `enable_icon_butcher`, `argb_icon_butcher`
* Default: `true`
## Breakable boxes
* Config settings: `enable_icon_box`, `argb_icon_box`
* Default: `false`
## Explosive objects
* Config settings: `enable_icon_boom`, `argb_icon_boom`
* Default: `false`
> [!NOTE]
> Breakable boxes and explosive objects do not have interaction prompts, and so do not receive focus--their special icon replaces the dot marker at all times. These two icons are disabled by default, but you can enable them here if you wish to see them.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# MCM > DotMarks > Object Settings
![Object Settings banner](https://i.imgur.com/SmLjFL4.png)

The settings in this menu control whether or not markers are added to each different type of object that DotMarks recognizes. If any are distracting or less useful to you, turning off their setting will hide their visible markers entirely. 
All of these are enabled by default with the exception of **Breakable Boxes** and **Explosive Objects**.
> [!TIP]
> As with the other methods of hiding markers, objects with hidden markers will still show interaction prompts normally, which is what you will almost always want--but if you prefer otherwise, you can turn off the [hidden_show_prompts](#hidden-items-still-show-interaction-prompts) setting.

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
> If you use my *Milspec PDA* addon, the [bodies_use_mpda_rules](#body-markers-obey-milspec-pda-rules) setting will cause body markers to use Milspec PDA's visibility rules--meaning that you'll only see a marker if you also see them on your PDA map.
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
> The detection of this item type is based on whether the object, by section, has a `blast` attribute with a value. Some pickup items (e.g. the jerrycan) that have a `blast` value are special cases classified instead as **Tools**.
>
> If an addon needs to tell DotMarks that one of its items should be treated the same way, the author can use DLTX to add its section name to the `section_lookup` section in `dotmarks_defaults.ltx`. Addon authors will find other sections there with similar purposes--most of which should be fairly self-explanatory to anyone who has any business changing them.
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
> This includes the Placeable Workbench from the *Hideout Furniture* addon, if one has been placed. The HF version will show a secondary interaction prompt for opening its control menu.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# MCM > DotMarks > Other Addons
![Other Addons banner](https://i.imgur.com/hdvQEwj.png)

DotMarks is designed to work hand-in-hand with a variety of popular addons, and many of those integrations have options which you can adjust in this menu.

Because DotMarks detects objects based on many of the same fundamental attributes the game itself uses to classify items, it will support many other addons right out of the box, as long as they define their own items in a way consistent with the vanilla definitions.
> [!NOTE]
> But there are always going to be exceptions and conflicts sooner or later. If that happens, see the [Troubleshooting](#troubleshooting) section, or contact the author of the conflicting addon.
## Dead stalkers use PAW faction patches
* Config setting: `bodies_use_paw_patches`
* Default: `false`

If my *Personal Adjustable Waypoint* addon is installed, enabling this option will cause dead stalkers to show their faction patch icon instead of the usual dot markers.
## Body markers obey Milspec PDA rules
* Config setting: `bodies_use_mpda_rules`
* Default: `false`

If my *Milspec PDA* addon is installed, enabling this option will cause bodies to be marked--or not--depending on whether the actor's PDA has the ability to see them on the map.
> [!TIP]
> If corpse markers are disappearing, or not appearing when you expect them to, check whether this setting is on--as well as Milspec PDA's own MCM settings, which are setting the terms for visibility. Also keep in mind that when MPDA clears its own marker (such as when you loot a body) that also hides it from DotMarks.
## Treat all squad members as talkative
* Config setting: `all_squad_members_talk`
* Default: false

In vanilla Anomaly, only the leader of a given squad can engage in conversation. Enable this setting if you have installed any addon which removes this leader-only restriction, such as the `Talk to Everyone` install option in _Modular Miscellaneous Tweaks_.
> [!NOTE]
> This setting is enabled by default in GAMMA, which includes that feature as part of its standard modlist.
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
## Enable animation for healing action
* Config setting: `sec_enable_heal_anim`
* Default: `true`

This option allows DotMarks to use the animation from the _Animated NPC Healing_ addon when healing wounded stalkers using the secondary action. It has no effect if that addon is not installed.
## Enable inspect animation for artefacts
* Config setting: `sec_enable_arty_anim`
* Default: `true`

If you have the _Artifacts Inspection_ addon, this option enables a secondary action for most artefacts which displays that addon's inspect animations upon picking up the artefact.
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
## Part dots orientation
* Config setting: `parts_dot_orientation`
* Default: `2` (radial)

By default, the part dots are arranged radially in a tight circle. You can instead choose to draw them as a horizontal or vertical line, but you'll probably want to adjust their positioning afterwards if you change this. Available settings:
* `0`  : horizontal line
* `1`  : vertical line
* `2`  : radial (in a circle around a center point)
> [!TIP]
> On weapons, in the radial orientation, the barrel will always be the dot at the very top. You can use this to quickly identify which weapons are worth picking up.
## Part condition dots X and Y offsets
* Config settings: `parts_dot_pos_x`, `parts_dot_pos_y`
* Default: `-7, -4`

Fine-tunes the position of the part condition dots on the Item Card.
## Size of the part condition dots
* Config settings: `parts_dot_scale`
* Default: `1.0`

Adjusts the size of the part condition dots. A value of 1 is their default intended size, while zero will hide these icons entirely.
## Part dots drop shadow transparency
* Config settings: `parts_dot_shadow_alpha`
* Default: `0.75`

Sets the Alpha (transparency) channel for the drop shadows underneath the part condition dots. Set to zero to disable their drop shadows.
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
## Name of the skill used for this check
* Config setting: `haru_skill_name`
* Default: `scavenging`

The internal name of the skill from *Skill System* that is used as the basis for the integration.
> [!WARNING]
> Bad things may happen if you typo this, and most of you will never have any need to tinker with it.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# MCM > DotMarks > Advanced Settings
![Advanced Settings banner](https://i.imgur.com/5R5ZGIV.png)

Settings for troubleshooting, adjusting screen positions, enabling or disable specific features, or any other customization that doesn't fall neatly into one of the other menus. If there is a detail you might want to customize, and it's not in one of the other tabs, you'll probably find it here. 
> [!WARNING]
> This is the "Advanced" menu for a reason--most of these settings are just granular UI customization, but a few can really break DotMarks or tank your performance if you make careless changes. Be sure to read the tooltips carefully so that you understand how each setting works.

> [!TIP]
> That said, some of these options will be _essential_ if you run the game in a screen resolution that is anything other than 1920x1080--you may need to adjust fonts and element sizes or positions, among other things.
>
> Remember, you can always reset your settings to default in MCM if things get messed up. Doing so will only reset the settings for the menu page you're on. The worst that will happen is that you have to restart the game.
## Enable debug logging
* Config setting: `debuglogs`
* Default: `false`

Enables or disables basic debug logging from the addon.
> [!IMPORTANT]
> This is _absolutely required_ for reporting any bug or issue. If this setting is not enabled, the game log will contain almost no information from DotMarks, and is probably useless for troubleshooting.
## Debug logging is verbose
* Config setting: `verbose`
* Default: `false`

If `debuglogs` is also enabled, this setting will enable additional logging that is more detailed, but also noisy. You should enable this for short periods if you know how to reproduce a problem, and are about to create a debug log for troubleshooting.
> [!WARNING]
> Don't enable this unless you're trying to troubleshoot a problem, and avoid leaving it enabled for a long time during normal play. It will quickly fill up your game log.
## Hide interaction dot markers
* Config setting: `hide_interaction_dots`
* Default: `false`

If enabled, the interaction dots themselves will be hidden, but the interaction UI will still be available.
> [!TIP]
> This is for all of you out there who are allergic to having stuff on your HUD, but still want to get the nice new prompts and secondary actions.
## Hide marker for untalkative stalkers
* Config setting: `hide_mute_stalkers`
* Default: `true`

If enabled, no marker will be shown at all for stalkers who can't be engaged in conversation.
> [!TIP]
> There is little point to disabling this setting, as untalkative stalkers have no other interactions aside from shooting them in the face. If you're using an addon that enables conversation with all stalkers, set [all_squad_members_talk](#treat-all-squad-members-as-talkative) to true instead of changing this.
## Hidden items still show interaction prompts
* Config setting: `hidden_show_prompts`
* Default: `true`

If a marker is hidden for any reason, its interaction prompts will still be shown.
> [!IMPORTANT]
> If this option is not enabled, hidden markers will not have any visible interaction prompts at all. This will not prevent you from picking them up if you want to, it just won't show the prompt.
## Hide the connecting white line
* Config setting: `hide_connecting_line`
* Default: `true`

If enabled, will hide the white line connecting the dot to the interact prompt.
> [!NOTE]
> This element used to be enabled by default, and looks very nice when it is positioned right--but its size and position has to be manually adjusted any time the prompt or their elements are moved around, and only looks correct in very specific positions. It will likely be replaced at some point by a better implementation.
## Hide the custom primary interact UI
* Config setting: `hide_pri_interact_ui`
* Default: `false`

If enabled, the custom primary interact UI added by DotMarks will not be shown. The primary vanilla "use" function and prompts will still work normally.
## Hide the custom secondary interact UI
* Config setting: `hide_sec_interact_ui`
* Default: `false`

If enabled, the custom secondary interact UI added by DotMarks will not be shown, and secondary interactions will not be available.
## Hide vanilla interact UI
* Config setting: `hide_van_interact_ui`
* Default: `true`

If enabled, any vanilla interact prompts (the text at the bottom of the screen) will be hidden if they have been replaced by a custom version.

This setting has no effect if the custom primary interaction UI is also hidden.
## Disable UI sound effects
* Config setting: `disable_all_sounds`
* Default: `false`

Disables the subtle UI "blip" sound effect that plays when cycling the mouse wheel or during other similar events.
## Scale GUI elements
* Config setting: `ui_scale`
* Default: `1.0`

This slider adjusts the scale of the UI's graphical elements, such as the keybind icon. This has no effect on the font size, but it can be used to fine-tune the GUI to account for your resolution or fonts.
## Scale font width allowance
* Config setting: `font_scale_w`
* Default: `1.0`

This setting allows you to adjust the amount of horizontal space that DotMarks assumes is required by the current fonts.
## Scale font height allowance
* Config setting: `font_scale_h`
* Default: `1.0`

This setting allows you to adjust the amount of vertical space that DotMarks assumes is required by the current fonts.
> [!NOTE]
> Fonts are just textures in Anomaly, and some fonts are taller or wider than others. You may need to adjust one or both of these settings if you are at a resolution other than 1920x1080, install a custom font addon, or change to a different font than the default.
## Keybind icon style
* Config setting: `keybind_bg_style`
* Default: `4`

Sets the visual style for the graphical "key" icons used as backgrounds for the keybind text on interaction prompts, or disables the backgrounds entirely. Valid settings:
* `4` : Flat worn (like Stalker 2)
* `3` : 3D worn appearance
* `2` : 3D black and white
* `1` : White wireframe
* `0` : Disabled, no background
## FONT SETTINGS
Some fonts will look better at different resolutions due to the inconsistent way Anomaly sizes the text. You can choose whichever one looks best to you.
> [!NOTE]
> The addon tries to adjust the UI based on the size of the text, but it may be necessary to manually fine-tune screen position values for the Item Card or other elements when using some fonts.
## Font used for main prompt
* Config setting: `font_main_prompt`
* Default: `2` (Letterica 16)

Valid settings:
* `7` : Graffiti 32
* `6` : Graffiti 22
* `5` : Graffiti 19
* `4` : Letterica 25
* `3` : Letterica 18
* `2` : Letterica 16
* `1` : Medium Print
## Font used for the Item Card
* Config setting: `font_item_card`
* Default: `0` (Small Print)

Valid settings:
* `6` : Graffiti 22
* `5` : Graffiti 19
* `4` : Letterica 25
* `3` : Letterica 18
* `2` : Letterica 16
* `1` : Medium Print
* `0` : Small Print
## Prompt fade-in duration
* Config setting: `prompt_fade_in_time`
* Default: `250`

The duration, in milliseconds, of the fade-in effect for interaction prompts.
## Prompt fade-out duration
* Config setting: `prompt_fade_out_time`
* Default: `150`

The duration, in milliseconds, of the fade-out effect for interaction prompts.
## Marker pop-in duration
* Config setting: `popin_anim_dur`
* Default: `500`

The duration of the elastic pop-in animation that plays when focus is transferred to a marker. Can be up to 2000ms. Setting to zero will disable the feature.
## Action text X offset
* Config setting: `action_text_pos_x`
* Default: `20`

Adjusts the horizontal offset of the action text.
> [!NOTE]
> This is the text that says what the interaction does, e.g. "take/use/eat/drink the thing".
## Action text Y offset
* Config setting: `action_text_pos_y`
* Default: `3`

Adjusts the vertical offset of the action text.
## Keybind text X offset
* Config setting: `bind_text_pos_x`
* Default: `3`

Adjusts the horizontal offset of the keybind text within its icon box.
> [!NOTE]
> This is the text that says what key or button you press to initiate the action, e.g. "T".
## Keybind text Y offset
* Config setting: `bind_text_pos_y`
* Default: `3.5`

Adjusts the vertical offset of the keybind text within its icon box.
## Item Card X offset
* Config setting: `item_card_pos_x`
* Default: `-5`

Adjusts the amount that the Item Card's position is horizontally offset.
## Item Card Y offset
* Config setting: `item_card_pos_y`
* Default: `-2`

Adjusts the amount that the Item Card's position is vertically offset.
## Item Card element X offset
* Config setting: `item_card_elem_x`
* Default: `7.5`

Adjusts the horizontal offset for the weight and value elements in the Item Card.
## Item Card element Y offset
* Config setting: `item_card_elem_y`
* Default: `0`

Adjusts the vertical offset for the weight and value elements in the Item Card.
## Item Card element height
* Config setting: `item_card_elem_h`
* Default: `12`

Adjusts the height of the weight and value elements in the Item Card.
> [!NOTE]
> This value is used to set the spacing between the rows.
## Item Card icon X offset
* Config setting: `item_card_icon_x`
* Default: `4`

Adjusts the horizontal offset for the weight and value icons within their elements.
## Item Card icon Y offset
* Config setting: `item_card_icon_y`
* Default: `2`

Adjusts the vertical offset for the weight and value icons within their elements.
## Item Card icon size
* Config setting: `item_card_icon_sz`
* Default: `7.5`

Changes the size of the icons in the Item Card, used for attributes such as Weight and Value.
## Item Card text Y offset
* Config setting: `item_card_text_y`
* Default: `1`

Adjusts the vertical offset of the weight and value text numbers within their elements.
## Horizontal offset for first interact
* Config setting: `pri_use_x_offset`
* Default: `-13`

Adjust the primary prompt's position horizontally. Allowed values are -512 to 1024.
## Vertical offset for first interact
* Config setting: `pri_use_y_offset`
* Default: `24`

Adjust the primary prompt's position vertically. Allowed values are -384 to 768.
## Horizontal offset for second interact
* Config setting: `sec_use_x_offset`
* Default: `-9`

Adjust the secondary prompt's position horizontally. Allowed values are -512 to 1024.
> [!TIP]
> If you don't like the way the primary and secondary prompts are slightly staggered, and want them to be aligned, change `pri_use_x_offset` and `sec_use_x_offset` to the same number.
## Vertical offset for second interact
* Config setting: `sec_use_y_offset`
* Default: `48`

Adjust the secondary prompt's position vertically. Allowed values are -384 to 768.
## Use fixed screen positions
* Config setting: `fixed_screen_pos`
* Default: `false`

If this option is enabled, the custom interaction prompts will have a fixed screen position based on the offsets above, and the offsets are measured relative to the upper-left corner of the screen, instead of from the position of the attached marker.
> [!IMPORTANT]
> You **must** restart/saveload your game for this setting to take effect.
## Near scan interval
* Config setting: `near_scan_interval`
* Default: `104`

Interval between nearby-object scans, in milliseconds. Smaller values will update more frequently.
## Far scan interval
* Config setting: `early_scan_interval`
* Default: `2457`

Interval between the more-intensive faraway-object scans, in milliseconds. Smaller values will update more frequently, but may have a performance cost.
> [!IMPORTANT]
> The strange, random-seeming numbers are there for a reason. Many addons update exactly once per second, which can cause them to sync up and create stutters when they all try to "stack" their updates at the same time.
>
> Unusual numbers like 104 and 2457 (roughly 0.1 second and 2.5 seconds, respectively) make it almost impossible for updates to regularly stack in this way. If you change these intervals, try to pick numbers like these.
## Radius of near scan
* Config setting: `near_scan_radius`
* Default: `4`

Radius of the nearby-object scan in meters. Many things are based on this value, such as the distances at which markers fade out and scale up/down. It is recommended that you don't adjust this value, or do so in _very_ small increments.
## Radius of far scan
* Config setting: `early_scan_radius`
* Default: `3`

Radius of the faraway-object scan in meters, which is used to detect and initialize objects before their markers need to be shown. This also determines the distance at which the system cleans up markers that are no longer needed. This is measured from the end of the near scan radius.
> [!NOTE]
> Higher values may have a performance cost due to markers remaining active when they aren't needed, and there is no benefit to setting it higher than a few meters.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# Troubleshooting
DotMarks is a big addon with some very strict dependencies. Stuff is gonna break for _someone_.

Hopefully it's not you. But if it is, keep reading and maybe you'll find a solution.
> [!WARNING]
> Stalker's game engine is old, janky as fuck, and sometimes does some wacky shit. Almost any flavor of weird could happen once. If you cannot make it happen twice, **I don't want to know about it**.
>
> Apologies, but I simply cannot chase down every random glitch that comes from Anomaly or someone's mod soup, even if one of my scripts shows up in the error--so **please do not waste my time or yours with any issue that does not reoccur, or which you cannot reproduce.**

Here are the most common issues that DotMarks users have, and how to solve them:

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Missing dependencies
I've tried to set up safety checks that display a message to the player a few moments after they first load into the game if a dependency is missing. But in case that fails, if something isn't working right, there's a good chance that you need to properly update a dependency. You can find out by enabling debug logging and searching the game log for the string `Checking for dependencies`. Whenever the [debuglogs](#enable-debug-logging) setting is enabled, DotMarks will output a block of text on startup that looks like this:
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
## Can't pick up anything that isn't centered in the crosshair, or `actor_on_update_pickup` error seen in log

If your version of the modded exes is earlier than `20250306`, you will see this error in your log:

`![axr_main callback_set] callback actor_on_update_pickup doesn't exist!`

If you are seeing that, you **MUST** update your version of the modded exes in order for object targeting to work properly. Otherwise the addon has to fall back on a vanilla targeting method that requires the crosshair to be *exactly* centered on the object, and you may have trouble interacting with some things.

The reason is that new callbacks are defined in a file that is contained in one of the DB files in the modded exes installation. Older versions may have loose versions of the files that are now contained in the DB, which override the new ones just like any other file in the gamedata folder--and cause things to break.

If you just update the binaries but somehow still have old DB files (or something is overwriting them), then some functions may *appear* to work, but the newly-added callback won't exist--so DotMarks has to rely on finicky, much less accurate methods of detecting the target object. Anything dependent on that callback, such as proper targeting and cycling through nearby pickups with the mouse wheel, won't work well if at all.

I repeat, and let me be completely clear:

### :exclamation: **IF YOU ARE SEEING THAT ERROR IN YOUR LOG ABOUT THE MISSING CALLBACK, YOUR MODDED EXES INSTALLATION IS STILL NOT CURRENT.** :exclamation:
### :exclamation: **EVEN IF YOU HAVE TRIED TO UPDATE THEM, THAT ERROR MEANS YOU MUST REINSTALL THEM FROM SCRATCH ACCORDING TO THE INSTRUCTIONS.** :exclamation:

I'm sorry to be so blunt and shouty, but I've been testing it with users for nearly half a year now, and the vast majority of the bug reports I get come down to the above issue. If you do the clean reinstall correctly, it _will_ fix this issue.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Empty icons where key binding names should be; MCM menus are absent, broken or missing content

You **MUST** update your MCM version to 1.7.0 or higher.

Some modpacks have embedded older versions of MCM, and some have even edited their copy of MCM to bypass Anomaly version checks. Don't do that, please. The version should be [somewhere around here](https://github.com/RAX-Anomaly/Anomaly-Mod-Configuration-Menu/blob/b02d0ff127181e698fa2c03fd434badb278211c9/gamedata/scripts/ui_mcm.script#L92) in your copy of `ui_mcm.script`, and it must be `1.7.0` or higher.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Poor performance / FPS loss / stutter
The setting that shows the total weight within stashes and other containers can cause an FPS loss in bases with many containers, or when there are many lootable bodies nearby. You can turn this off (or customize many other things) in the DotMarks MCM menu.

Beyond that, while I am continually working to optimize it, understand that this addon is *completely replacing most of Anomaly's interaction UI*. It needs to keep track of all nearby objects at all times so that it can know when to show interaction prompts. There is going to be a certain minimum amount of overhead involved in doing that much work, meaning that I can only do so much to optimize it.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Auto Looter keybind doesn't work
Go to **MCM > DotMarks > Other Addons** and disable [hijack_autoloot_keybind](#override-auto-looter-keybind).

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Conflicts with other addons
Interaction Dot Marks touches nearly every part of the game, and is involved with every single object interaction. I have designed it to be as conflict-free as possible, but I can't possibly account for--or support--every addon out there.
For this reason I can only officially support two install configurations at this time:
* Vanilla Anomaly running on current modded exes with _supported_ addons
* GAMMA with current modded exes and the [DEFAULT MODLIST ONLY](https://discord.com/channels/912320241713958912/977190498420801536)
> [!IMPORTANT]
> I consider an addon to be "supported" by DotMarks if it is mentioned by name in either this documentation or the addon itself.

If and when my personal life and free time allow, I will do what I can to help sort out issues you might have with the current version of DotMarks, running on the above two configurations.

If your modlist is anything else, then you can still report an issue--but I can't promise anything.
> [!IMPORTANT]
> That isn't to say DotMarks won't work on other installations--quite the contrary! Most of you will have no issues. But if something goes wrong in someone's mod soup, **I'm not signing up to figure out what it is**.

Some conflicts are easy to fix. A new item from an addon not being detected correctly--or other object identification issues like that--might just require adding a few lines in a config file. But if your Stalker soup with hundreds of mods is encountering an issue no one else has ever seen before and that I can't reproduce, it's very likely something in your modlist, not an issue with DotMarks that needs fixing--and either you or the other addon's author will probably need to sort that out yourself.
> [!TIP]
> If you're an addon author with an object that isn't being handled correctly, I've implemented hooks in DotMarks that make it extremely simple to add support yourself. It may just be a matter of adding your item's information to [dotmarks_defaults.ltx](https://github.com/CatspawMods/Anomaly-Addon-Archive/blob/main/Interaction%20Dot%20Marks/packing/Main/gamedata/configs/scripts/dotmarks_defaults.ltx) in the `[section_lookup]` or one of the blacklists via DLTX, or injecting custom handling using the `register_addon_handler()` function in [ui_hud_dotmarks.script](https://github.com/CatspawMods/Anomaly-Addon-Archive/blob/main/Interaction%20Dot%20Marks/packing/Main/gamedata/scripts/ui_hud_dotmarks.script). See the [Fillable Canteens Water Pumps patch](https://github.com/CatspawMods/Anomaly-Addon-Archive/blob/main/Interaction%20Dot%20Marks/packing/Modules/WaterPumps/gamedata/scripts/dotmarks_canteen_water_pumps.script) for an example of how to do the latter.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Still having problems?
### 1. GENERATE A CLEAN DEBUG LOG
If you need to report a problem, first do this:

1. Go to **MCM > DotMarks > Advanced**
2. Enable both [Debug](#enable-debug-logging) and [Verbose](#debug-logging-is-verbose) logging
3. Exit the game completely and restart it
4. Load a save game where the issue occurs, and reproduce it as simply as possible
5. Exit the game immediately
 
> [!IMPORTANT]
> If you're reporting an issue with a specific object, make your savegame while standing right in front of the object and targeting it. You only need to load the game for a few moments before exiting.
### 2. CHECK DEPENDENCIES
Follow the instructions in the [Missing dependencies](#missing-dependencies) section.
### 3. REPORT THE ISSUE
If all of those are correct and something's still busted, please take the short DEBUG LOG from the instructions above--the whole thing--and post it somewhere I can see it, along with as much information as possible about the issue--including your modlist, if you're not using the [stock default GAMMA modlist](https://discord.com/channels/912320241713958912/977190498420801536). If the issue is visual, or concerns a specific object, include a screenshot of what looks wrong, taken in the spot where you made your debug savegame.
> [!TIP]
> You will find the game log in `<Anomaly Install Folder>\appdata\logs`. If you're a GAMMA user, this will be in the location where you installed the base game.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)
