# **Interaction Dot Markers**

**THIS IS A WORK IN PROGRESS THAT HAS NOT YET BEEN RELEASED ON MODDB**

A blatant imitation of the "interaction dot" HUD markers in Stalker 2, and some of its other UI niceties such as floating prompts and secondary interactions.

Except not quite as cool, because Anomaly is jank. Hopefully this makes it a little less jank.

Also implements an "Alternate Interact" keybind with contextual actions such as unloading a weapon without having to pick it up.

Requires modded exes by demonized:
https://github.com/themrdemonized/xray-monolith

If your version of the modded exes is earlier than `20250306`, you will
see this error in your log:

`![axr_main callback_set] callback actor_on_update_pickup doesn't exist!``

If you are seeing that, you **MUST** update your version of the modded exes in order for object targeting to work properly.

If you think you have updated, but are still seeing that error, you need to do a clean reinstall of the modded exes from scratch. Period.

If it's even older than that, a lot of the addon probably won't work at all. You should update your modded exes ASAP.
