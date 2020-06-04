## Effex \[sic\] (`o_fx` prefix)

- `o_fx_dead` is a tiny animated orange sprite that
    - gets a randomized animation speed
    - is constructed 36 times in a circular spread when the player dies
    - destroys itself when its animation ends
- `o_fx_hit` is a small animated sprite that
    - plays a "hit" sound effect
    - is constructed whenever a player weapon is destroyed on contact
- `o_fx_hurt` is the first frame of the same animated sprite that 
    - plays a different "hurt" sound effect
    - is constructed when the player takes damage from attacks 
    - flashes the screen white with an alpha of `0.5` (meaning the sprite is a functional requirement)
    - after 2 frames, destroys itself
- `o_fx_light` is the first frame of a "dragon" sprite ([Swarm Lord](BN_bosses.md#swarm-lord-dragon-prefix) that
    - is not `visible` if an instance already exists
    - flashes the screen white with an alpha of `0.3`
    - after 6 frames, destroys itself
- `o_fx_ocean` is a 16x16 sprite with a single pixel that
    - gets a randomized animation speed
    - is created with the title screen and is placed throughout `rm_intro`
- `o_fx_thunder` is the same frame of `o_fx_light` that
    - flashes the screen white with an alpha of `0.7`
    - alternates `visible = false` and `visible = true` 4 times each then is destroyed, with 6 frames in between each stage (a total of (8+1) \* 6 = 54 frames)
- `o_fx_splode` is an animated red explosion sprite that
    - gets a randomized animation speed
    - plays a "hit" sound effect
    - is constructed multiple times when boss hp hits 0
    - destroys itself when its animation ends