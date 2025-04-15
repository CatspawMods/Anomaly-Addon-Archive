# **Interaction Dot Marks**
A blatant imitation of the "interaction dot" HUD markers in Stalker 2, and some of its other UI niceties such as floating prompts and secondary interactions.

Except not quite as cool, because Anomaly is jank. Hopefully this makes it a little less jank.

Also implements a "Secondary Interact" keybind with contextual actions such as unloading a weapon without having to pick it up, or taking everything from a stash without opening the loot window.

Requires **MCM 1.7.0** and the **20250306 modded exes** by demonized, or (better yet) the latest of both:
* [Demonized modded exes (Github)](https://github.com/themrdemonized/xray-monolith)
* [Mod Configuration Menu (Moddb)](https://www.moddb.com/mods/stalker-anomaly/addons/anomaly-mod-configuration-menu)

If your exes are older than that, a lot of the addon probably won't work very well, if at all. You should update your modded exes ASAP.

This addon *will not work at all* on vanilla Anomaly, and may crash spectacularly if you try. Vanilla Anomaly doesn't support the necessary engine functions, and there is no good reason not to upgrade to the latest modded exes.

![rad_symbol_divider](https://i.imgur.com/Y5bQDtj.png)

# Personalization
Everyone likes different things. "Play your own way" is axiomatic to me.

If your issue is that you don't like the way something looks or works, start by checking out the MCM menu under **DotMarks**. It's packed with customization options that will let you selectively disable or tweak just about everything in the addon. And you'll need the **Advanced** menu there if you're going to report an issue.

Everything in DotMarks is customizable, with most settings in MCM. Every individual part of the interaction UI can be enabled, disabled, or moved around. Every animation, every icon, every sound, every feature, every color. It's your UI--make it suit your preferences.

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
