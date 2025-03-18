# **Fair Fast Travel**  
Moddb: https://www.moddb.com/mods/stalker-anomaly/addons/fair-fast-travel-duration-for-anomaly-151

## Change Log
**2.17.1:**
- Updated `utils_catspaw_text` to 1.5.
- Fix for "no guides" error message incorrectly preventing travel from labs and other dangerous areas.
- Changes to some MCM defaults for non-GAMMA users:
  * Basic Fast Travel no longer has a cost by default
  * Brain Scorcher blocking travel is off by default
- These were always intended to be optional features, and were initially set to default in order to gather playtest data, but were intended to be disabled sooner.

**2.17.0:**
- Updated my utility scripts.
- Unlocalized some tables in the `game_guide_travel`.

**2.16.0:**
- Travel costs now scale based on a `map_hazard_coef` value that is set per-map in `configs\scripts\fast_travel\travel_destinations.ltx`.This feature is on by default, and can be disabled in MCM.
- Fix for crash when trying to use the Monolith guide in Outskirts.
- Added encyclopedia entry that unlocks the first time the player right-clicks on a destination blocked by the Brain Scorcher.
- Various bits of code cleanup.

**2.15.2:**
- Fix for crash from typo in the previous fix.

**2.15.1:**
- Fix for task markers targeting fast travel points disappearing on savegame load or level transition.

**2.15.0:**
- Fix for "SendCallback" crash when trying to use guide travel. 
- Updated modded exes download link and clarified that some features are unavailable without them. 
- Updated `utils_catspaw_common` to 1.5 with hopefully better detection for "non-standard" modded exes like EFP.

**2.14.2:**
- Fix for issue with Haruka Skill System integration.

**2.14.1:**
- Fix for disappearing task markers in GAMMA.

**2.14.0:**
- Added new encyclopedia entry to PDA guide; requires modded exes. 
- Clarified that the Guide Travel Overhaul also requires modded exes (though PDA guides do not). 
- Fixed crash when updating travel spots in while using vanilla exes. 
- Fixed travel zones not spawning correctly (and thus being unable to travel with TZ restrictions in place) when using vanilla exes.

**2.13.1:**
- Fix for crash when using the Anomaly keywrapper addon with the default (unbound) squad teleport hotkey.

**2.13.0:**
- Updated utils_catspaw_text to 1.3 to fix a minor bug with color substitutions.

**2.12.0:**
- Fixed an unusual crash when a player companion doesn't have a game object.

**2.11.0:**
- Fixed an issue with the FOMOD structure that caused some of the travel zone indicator files to get copied incorrectly, causing a crash when the indicator was enabled.

**2.10.0:**
- Companions now come with the actor on local travel, optional in MCM. 
- Added keybind to manually trigger this function. 
- Updated utils_catspaw_common to avoid crash when loading a newer PAW after FFT.

**2.9.2:**
- Fix for possible crash when iterating guide destinations. 
- Fix for crash when right-clicking on custom travel destinations in the new Promzone maps. 
- Fix for some debug destinations being allowed when disabled. 
- Updated utility scripts and adjusted some logging.

**2.9.1:**
- A test copy of one of the scripts got zipped up into the final archive by mistake. Sorry about that!

**2.9.0:**
- New status indicator for when the player is in a travel zone. 
- Campfires can now optionally count as a travel zone, as well as a fast travel destination. 
- A vast amount of refactoring related to an effort to unify some of my common code.

**2.8.0:**
- Added new feature to restrict fast travel when outside of a friendly base. 
- Added support for Western Goods guides. 
- Fixed intro text for Tourist, and some copy/paste errors in the Guide Overhaul API functions. 
- Overhauled MCM menu using UI functors. 
- Completely refactored text generation to support custom strings. 
- Various minor text fixes and cleanup of unused code.

**2.7.2:**
- Fix for nil "levname" crash when right-clicking mapspots with "Use long names" enabled in Anomaly's FT settings.

**2.7.1:**
- Fix for Meadow and Truck Cemetery being considered "northern" maps, which could block travel in story mode. 
- Added missing fast travel destinations in Meadow.

**2.7.0:**
- Fix for crash when trying to travel when a condition prevents it. 
- Fix for incorrect route settings causing some Merc and Army guides to not work. 
- Added option for the Brain Scorcher to block certain routes in story mode until it is disabled. 
- Improved landing location for the Army to Yantar heli rides. 
- Rewrote all text generation functions for future customizability. 
- Reorganized general MCM options. Added option whether to show distance/duration on travel prompts. 
- Added option to toggle verbose logging. 
- Cleaned up old code and some spammy logging lines.

**2.6.0:**
- Overhauled all NPC guides, moving them into a new, more modding-friendly framework. 
- Guides can now use FFT's calculations for distance, duration, and cost. 
- Guide dialogue now shows these details up-front. 
- Added option to scale FT costs progressively with player rank. 
- Added option to round all generated fares to multiples of 100.

**2.5.3:**
- Another fix for nil value crash on guide travel--pretty sure I nailed the source for good this time.

**2.5.2:**
- Fix for nil value crash on some guide travel.

**2.5.1:**
- Fix for distance calculations on inter-map travel.

**2.5.0:**
- Added option (on by default) to apply Guide Travel's trip duration and survival stat pause settings to NPC guides using a new monkeypatch script. 
- Significant refactoring of distance calculations and travel logic to accomodate new external modding hooks and functionality--these are mainly code syntax and table structure changes; no calculation results should differ.

**2.4.0:**
- Regular fast travel can now be disabled in MCM separately from Anomaly's FT setting. Anomaly's setting now controls only which travel points are displayed on the map. 
- This is so that you can do things like disabling fast travel but allowing backpack and/or guide travel. 
- Cleaned up deprecated code that hasn't been used for a long time.

**2.3.4:**
- Fast travel options will now show the correct name for Quick Pin markers (from Personal Adjustable Waypoint), including ones that you have renamed. 
- Proactive fix to prevent harmless log spam with the latest demonized binaries.

**2.3.3:**
- Hotfix for crash on existing GAMMA saves.

**2.3.2:**
- Very minor patch that slightly adjusts the update interval so that it isn't precisely 1000ms, and adds a hidden MCM key for adjusting this interval for use by modpack makers. 
- This is to avoid stacking updates with other addons that all tend to use precise 1000ms intervals.

**2.3.1:**
- Added option to determine which items equipment degradation is applied to:**
all gear, all armor only, all weapons only, or one item at random. 
- Added minimum health and satiety thresholds for travel. 
- Fixed a number of RUS localization strings.

**2.3.0:**
- Added optional equipment degradation for all travel types, configurable in MCM. By default it is on in very small amounts for inter-map travel types, and off for local travel. 
- Added MCM option to re-enable the vanilla "Open backpack" context menu option that is usable from across the map. 
- Compatibility fixes for Arszi's Radiation Overhaul (integrated) and Warfare Alife Overhaul (MCM option). 
- Many internal changes that should be invisible. 
- Updated RUS strings, some from leeviathan and some (more recent) from GT.

**2.2.1:**
- Hotfix for survival stats being reset to old values on game load or level transition when stat pausing was enabled. This should also remove the issue from any save that had it.

**2.2.0:**
- Added MCM settings for NPC combat distance and timeout values, and new custom MCM menu slide images. 
- Made a handful of string fixes. 
- Updated RUS localization from leeviathan.

**2.1.0:**
- Fixed issue with location names when "visit" messages are enabled. 
- Added option to individually configure costs for each travel type, instead of just for guides. 
- Added option to disable guides ignoring status effects. Implemented new method of detecting combat: only NPCs within 100 meters who have seen you within the last 10 seconds count. 
- RUS localization update thanks to leeviathan.

**2.0.0:**
- Huge changes necessitating a new major version. 
- Added paid guide travel. 
- Added options for individual or linked cooldowns for each fast travel type. 
- Added Quick Pin travel. 
- Added option to pause survival stats during travel. 
- Moved all fast travel functionality into a single script and converted game_backpack_travel.script into a stub for compatibility. 
- Many, many other changes and new customization options.

**1.7.0:**
- Now compatible with both 1.5.1 and 1.5.2, tested on fresh installs both with and without MCM. 
- Integrated the LPFT mod patch into the main travel scripts, so now the only patch needed for Demonized's mod is the one in this latest archive. 
- Various bits of polish on the script.

**1.6.3:**
- Added integration with Haruka's Anomaly Skill System mod and an MCM option for using it (off by default). If you have their Skill System installed (highly recommended!) and enable the option in MCM, FFTD will adjust travel times based on the speed multiplier from your Endurance skill. 
- Also added a patch/merge with replacement scripts for compatibility with Demonized's Limited and Paid Fast Travel mod. Overwrite their scripts with the ones from this archive.

**1.6.2:**
- Corrected an issue that could cause backpack travel to not work when the Anomaly setting for time change was disabled. 
- Added MCM support for toggling the disguise fix, along with RUS localization for all current strings. 
- Console debug logging added, off by default.

**1.6.1:**
- There was a minor logic error in the mapspots that sometimes caused incorrect data to be shown--this was visual only; fast travel was functionally fine. However, in the process of fixing that, I discovered a vanilla Anomaly bug where duplicate mapspots get added if you have both backpack and normal fast travel enabled. 
- Added new logic to filter out these dupes and uploaded this hotfix.

**1.6.0:**
- Added proper distance calculations for travel to other maps, along with an estimate of distance and travel time in the map spot context menu. 
- If you are upgrading from a previous version, I strongly recommend changing the minutes per 100m for other-map travel from 15 to 10, to be the same as same-map travel.

**1.5.0:**
- Added a fix for the vanilla Anomaly bug that causes your disguise to be exposed if you fast travel in front of an NPC with the "consider time spent near NPCs" option enabled.

**1.4.1:**
- Corrected encoding on Russian localization. It should now be properly saved in Windows-1251, so please let me know if something doesn't display correctly! 
- As with 1.4, there is no functional change, just Russian text for those who need it.

**1.4.0:**
- Added Russian localization. I produced this using Google Translate, so while I took great pains to ensure it is as accurate as I could make it, please let me know if you find any mistakes.
- There is no functional change, so if you don't need Russian localization, you don't need this update.

**1.3.0:**
- Cleaned up a few leftover scraps in the MCM script from when I was figuring out how to add MCM support. No need to re-download; the scraps were unused and there is no functional change.

**1.2.0:**
- Initial public release.
