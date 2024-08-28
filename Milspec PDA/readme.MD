# **Milspec PDA and Kill Tracker**  
Moddb: https://www.moddb.com/mods/stalker-anomaly/addons/milspec-pda-for-anomaly-151-152-and-gamma

# Key MCM feature flags  
`milpda\milpdagen\tracking_mode` - When to show markers for dead bodies on the map  
`milpda\milpdagen\monster_tracking_mode` - When to show markers for dead mutants on the map  
`milpda\milpdagen\victim_text_mode` - When to show hint text identifying the body  
`milpda\milpdagen\killer_text_mode` - When to show hint text identifying the killer  

**The default for all of the above is `2` (actor and squad) but this can be adjusted for preference:**  
`0` = do not show  
`1` = show only for actor  
`2` = show for actor squad also  
`3` = show all  

**There is a faction blacklist, which lets you always hide certain factions regardless of unlocks or PDA features, e.g:**  
`milpda\facblacklist\monolith = true`  
`milpda\facblacklist\greh = true`  
`milpda\facblacklist\isg = true`  

**There are also flags that let you separately override the default behaviors of each PDA model:**  
`milpda\cfg_device_pda_1``  
`milpda\cfg_device_pda_2`  
`milpda\cfg_device_pda_3`  
`milpda\cfg_device_pda_milspec`  

**Each has the following settings:**  
`enabled` - Whether to use overrides for this PDA model, all below settings are default if false  
`show_f` - Whether it can display your own faction on the map, bool  
`show_n` - Whether it can display neutral stalkers, bool  
`show_e` - Whether it can display enemy stalkers, bool  
`show_k` - Whether it can display body markers, bool  
`obey_bl` - Whether it obeys the faction blacklist, bool  
`progmult` - Multiplier related to the progression feature, float  

**Finally, speaking of which, there is the progressive mode, which creates a new gameplay loop where the player must loot PDAs from a given faction in order to accumulate enough data to unlock access to see them on the map. I love playing with this and highly recommend it.**
`milpda\progressive\enabled = true`
	Whether faction mapspot visbiility must be progressively unlocked - if this is false, everything below can be ignored  

`milpda\progressive\gain_mode = 1` - When the actor gains unlock progression from a PDA  
`0` = on first pickup  
`1` = when first read  
`2` = when scrapped (ignores decryption by sacrificing the PDA)  

`milpda\progressive\must_decrypt = true`  - Whether a locked PDA must first be decrypted in order to gain progress  
`milpda\progressive\access_resets = true` - Factions will realize their codes are compromised and revoke access after a time  
`milpda\progressive\only_enemy_unlock = false` - If true, neutral factions are unlocked by default on supported devices  
`milpda\progressive\only_enemy_resets = true` - If true, neutral factions won't revoke access once gained  
`milpda\progressive\access_time = 7` - Duration of access gained  
`milpda\progressive\access_unit = d` - Unit of time for above (s, m, h, d)  

**This list is not exhaustive, just some of the most important. See `catsy_milpda_mcm.script` for a full list of feature flags.**
