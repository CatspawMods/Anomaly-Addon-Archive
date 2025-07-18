![DotMarks top banner](https://i.imgur.com/OXAK0xS.png)

# Interaction Dot Marks - Troubleshooting
DotMarks is a big addon with some very strict dependencies. Stuff is gonna break for _someone_.

Hopefully it's not you. But if it is, keep reading and maybe you'll find a solution.
> [!WARNING]
> Stalker's game engine is old, janky as fuck, and sometimes does some wacky shit. Almost any flavor of weird could happen once. If you cannot make it happen twice, **I don't want to know about it**.
>
> Apologies, but I simply cannot chase down every random glitch that comes from Anomaly or someone's mod soup, even if one of my scripts shows up in the error--so **please do not waste my time or yours with any issue that does not reoccur, or which you cannot reproduce.**

Here are the most common issues that DotMarks users have, and how to solve them:

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Nothing works at all, not even the "DotMarks" MCM menu - it's like the addon isn't even installed
Well that sucks.

DotMarks is pretty robust. If it isn't working at all, and you don't even see its MCM menu, then something has caused the main script to completely fail to load. And that probably means that a critical file is either missing or outdated. When it first starts to load, DotMarks checks to make sure it has all its core files--particularly my utility scripts.

Those scripts are also used by my other addons. If you load a much older one after DotMarks, and it replaces one of those core scripts with one that doesn't have the right functions, DotMarks will fail to load. And if the main script fails to load, the MCM script won't load either.

And there won't be any question at all that this is what's happened--just open your game log, and search for the text "Interaction Dot Marks". What you _should_ be seeing is a line like this, with the version number and release date:
```
Interaction Dot Marks 1.0 (rel 20250714) began initialization at 4186
```
If the main script failed to load, you'll instead see a stack trace followed by an error similar to this one:
```
! Interaction Dot Marks requires script utils_catspaw_common, which is outdated or missing!
```
If you are seeing any error like that mentioning one of my utility scripts, then moving DotMarks to the bottom of your load order to resolve it. This is a good general troubleshooting step anyway. It'd also be a good idea to update my other addons to their most recent versions as well, because one or more is outdated.

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
```
![axr_main callback_set] callback actor_on_update_pickup doesn't exist!
```

If you are seeing that, you **MUST** update your version of the modded exes in order for object targeting to work properly. Otherwise the addon has to fall back on a vanilla targeting method that requires the crosshair to be *exactly* centered on the object, and you may have trouble interacting with some things.

The reason is that new callbacks are defined in a file that is contained in one of the DB files in the modded exes installation. Older versions may have loose versions of the files that are now contained in the DB, which override the new ones just like any other file in the gamedata folder--and cause things to break.

> [!TIP]
> The specific file that's causing this issue is going to be `callbacks_gameobject.script`. If you see that file, you **must** delete it. But the base install shouldn't even *have* a `scripts` folder anymore.

If you just update the binaries but somehow still have old DB files (or something is overwriting them), then some functions may *appear* to work, but the newly-added callback won't exist--so DotMarks has to rely on finicky, much less accurate methods of detecting the target object. Anything dependent on that callback, such as proper targeting and cycling through nearby pickups with the mouse wheel, won't work well if at all.

I repeat, and let me be completely clear:

> [!CAUTION]
> :exclamation::exclamation::exclamation: **IF YOU ARE SEEING THAT ERROR IN YOUR LOG ABOUT THE MISSING CALLBACK, YOUR MODDED EXES INSTALLATION IS STILL NOT CURRENT.** :exclamation::exclamation::exclamation:
>
> :exclamation::exclamation::exclamation: **EVEN IF YOU HAVE TRIED TO UPDATE THEM, THAT ERROR MEANS YOU MUST REINSTALL THEM FROM SCRATCH ACCORDING TO THE INSTRUCTIONS.** :exclamation::exclamation::exclamation:

I'm sorry to be so blunt and shouty, but I've been testing it with users for a long time now, and the vast majority of the bug reports I get come down to the above issue. If you do the clean reinstall correctly, it _will_ fix this issue.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Empty icons where key binding names should be; MCM menus are absent, broken or missing content

You **MUST** update your MCM version to 1.7.0 or higher.

Some modpacks have embedded older versions of MCM, and some have even edited their copy of MCM to bypass Anomaly version checks. Don't do that, please. The version should be [somewhere around here](https://github.com/RAX-Anomaly/Anomaly-Mod-Configuration-Menu/blob/b02d0ff127181e698fa2c03fd434badb278211c9/gamedata/scripts/ui_mcm.script#L92) in your copy of `ui_mcm.script`, and it must be `1.7.0` or higher.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## I'm a GAMMA user and I don't see any of the prompts for secondary actions
These prompts are disabled in GAMMA's current default settings, but you can easily change that in MCM:

**MCM > DotMarks > Interactions**

There you can place a check beside any secondary actions you want to enable.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

## Not seeing an interaction prompt when you think one should appear (or vice-versa)
If you ever have any question whether or not DotMarks is correctly displaying a prompt when it should do so, go to **MCM > DotMarks > Advanced** and turn off the [Hide vanilla interact UI](#hide_vanilla_interact_ui) setting. This is the feature that hides the text prompts at the bottom of the screen.

If you see a vanilla text interaction prompt, but DotMarks does not correctly display a prompt--or vice-versa--please do the usual and [create a clean debug log](#1-generate-a-clean-debug-log) for me to examine.

If you disable that setting and still do not see a vanilla interact prompt, then you wouldn't see one whether or not DotMarks was installed.
> [!NOTE]
> Drivable vehicles have huge hitboxes that are difficult to handle with a single marker, and the prompt appearance will not always be consistent with the vanilla prompt. It's a known issue, no need to further report it. It won't stop you from doing the actual interaction.

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
> If you're an addon author with an object that isn't being handled correctly, I've implemented hooks in DotMarks that make it extremely simple to add support yourself. It may just be a matter of adding your item's information to [dotmarks_defaults.ltx](https://github.com/CatspawMods/Anomaly-Addon-Archive/blob/main/Interaction%20Dot%20Marks/packing/Main/gamedata/configs/scripts/dotmarks_defaults.ltx) in the `[section_lookup]` or one of the blacklists via DLTX, or injecting custom handling using one of the new callbacks added.
>
> Everything you'll need to know is in the [addon support documentation](addon_support.md) 

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
>
> This log gets *deleted and overwritten* every time the game launches, so don't delay after creating the log--zip it up or make a copy of it right away, before launching the game again.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)
