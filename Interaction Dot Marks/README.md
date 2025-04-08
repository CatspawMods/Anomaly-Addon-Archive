# **Interaction Dot Markers**

## **THIS IS A WORK IN PROGRESS THAT HAS NOT YET BEEN RELEASED ON MODDB**
## EXPECT BUGS

A blatant imitation of the "interaction dot" HUD markers in Stalker 2, and some of its other UI niceties such as floating prompts and secondary interactions.

Except not quite as cool, because Anomaly is jank. Hopefully this makes it a little less jank.

Also implements an "Alternate Interact" keybind with contextual actions such as unloading a weapon without having to pick it up.

Requires MCM `1.7.0` and the `20250306` modded exes by demonized, or (better yet) the latest of either:
* [Demonized modded exes (Github)](https://github.com/themrdemonized/xray-monolith)
* [Mod Configuration Menu (Moddb)](https://www.moddb.com/mods/stalker-anomaly/addons/anomaly-mod-configuration-menu)

If your exes are even older than that, a lot of the addon probably won't work at all. You should update your modded exes ASAP.

This addon *will not work at all* on vanilla Anomaly, and may crash spectacularly if you try. Vanilla Anomaly doesn't support the necessary engine functions, and there is no good reason not to upgrade to the latest modded exes.

## Troubleshooting
I've tried to set up code traps that display a message to the player a few moments after they first load into the game. But in case that fails, or you missed it, you'll need to properly update one or more dependencies.

### Have to exactly center crosshair to pick up anything, or `actor_on_update_pickup` error in log

If your version of the modded exes is earlier than `20250306`, you will see this error in your log:

`![axr_main callback_set] callback actor_on_update_pickup doesn't exist!`

If you are seeing that, you **MUST** update your version of the modded exes in order for object targeting to work properly. Otherwise the addon has to fall back on a vanilla targeting method that requires the crosshair to be *exactly* centered on the object, and you may have trouble interacting with some things.

If you think you have updated, but are still seeing that error or have to center your crosshair exactly in order to pick up anything, you need to do a clean reinstall of the modded exes from scratch. Period. 

Even if it says the correct version at the main menu, if these things are happenig it means either that the DB files haven't updated, or that you need to delete some existing files that are overriding the updated ones.

### Empty icons where key binding names should be, MCM menus are absent or missing content

You **must** update your MCM version to 1.7.0 or higher.

Some modpacks have embedded older versions of MCM, and some have even edited their copy of MCM to bypass Anomaly version checks. Don't do that, please. The version should be [somewhere around here](https://github.com/RAX-Anomaly/Anomaly-Mod-Configuration-Menu/blob/b02d0ff127181e698fa2c03fd434badb278211c9/gamedata/scripts/ui_mcm.script#L92) in your copy of `ui_mcm.script`, and it must be `1.7.0` or higher.

### Poor performance / FPS loss / stutter
The setting that shows the total weight within stashes and other containers can cause an FPS loss in bases with many containers, or when there are many lootable bodies nearby. You can turn this off (or customize many other things) in the DotMarks MCM menu.

Beyond that, while I am continually working to optimize it, understand that this addon is *completely replacing most of Anomaly's interaction UI*. It needs to keep track of all nearby objects at all times so that it can know when to show interaction prompts. There is going to be a certain minimum amount of overhead involved in doing that much work, meaning that optimization can only go so far.

### Known issues
* Take-all from stash opens the stash anyway (need to trap the inventory-open event)
* Mutant skinning not working properly with the knife-in-inventory mod - verify, still an issue?
* Some cabinet doors marked at the hinge instead of the lock, or otherwise require per-visual/map offsets
