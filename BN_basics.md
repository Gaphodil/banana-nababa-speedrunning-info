## Mechanics

### Preface
- the `speed` of every room is 60, which is the effectively the FPS; a `step` is thus one frame (I'll use these interchangably)
- the speed of entities uses the following:
    - `hspeed` positive is moving right, negative moving left
    - `vspeed` positive is moving down, negative moving up
    - `gravity` is usually a positive real number, and is simple downwards acceleration (add to `vspeed` every step)
- the game uses a 16x16 pixel grid but entities are not locked to them
- the game doubles the size of the window (so each "pixel" is output as 2x2)

### Player (`o_player`)
- the player has 100 max hp
- the initial sprite is facing right but the constructor starts with facing left

#### Horizontal movement
- left/right movement is 1 pixel per frame
- pressing both left/right has net 0 movement but prioritizes facing right

#### Jumps and gravity
- there's no mutual exclusivity for pressing or letting go of jump keys, but the results only affect the player directly
- pressing a jump key while the player is on ground makes a jump noise and sets the player's `vspeed = -6`
- letting go of a jump key if `vspeed < -1` halves the current `vspeed`
    - this check is made immediately after the previous, so if you press one jump key and let go of another on the same frame your initial `vspeed` would be -3
    - if you press both jump keys and let go the frame after you would have a `vspeed` of -1.4
- if air is below the player then they are affected by `gravity = 0.4`
- mathematically maximal jump height is reached at 15 frames and travels 45 pixels vertically (but rounding of decimals between steps may be why this value is larger in practice)
- colliding with a solid block or platform snaps the player to the top and plays a landing noise

#### Attacking
- there's no mutual exclusivity for attack keys, so **two attacks can be made at the same time**
- **attack cooldown is 8 steps**, all attacks do 1 damage to enemies with `hp`
    - I do not know whether attack cooldown finishing or checking keyboard input happens first in the final frame

#### Getting hit
- immediately activates invincibility frames (iframes) then returns to vulnerability after 40 frames

### Health Pickups (`o_powerup_hp`)
- random `hspeed`, downwards `vspeed` and `gravity`
- stops moving on hitting any block (currently solid or otherwise)
- has comment "GAIN 10 HP" but only restores 5

### Platforms (`o_block_hollow`)
- are considered solid only when **all three** are true:
    - the player's feet are above the platform
    - the player has a vertical speed of 0 or is moving downward
    - the Down key is not being held

---

## That one trick I found and related mechanics

### Stage select:
- becomes `visible` after 30 steps; most inputs will not do anything until full ui is visible
- pressing enter on a valid stage will wait 90 steps, then perform the transition over 100 steps

### Gameover:
- `press <Escape>` whenever `o_player` exists calls its destroy event
- `o_player`'s destroy event changes to the gameover screen after 70 steps 
- `press <Enter>` event from `o_gameover` fades to stage select over unspecified (presumably default) steps (80 in GM8.1 but then it shouldn't save as much time as it does?)
    - game appears to have been released 2007-08-07, and GM8 came out 2009-12
    - playing back video of similar transition (pressing play from `o_title`) gives 65 frames to full display, so should be 35 frames out of gameover
        - this appears to be variable, especially during post-boss transitions, taking longer for unknown reasons

### Collecting an orb (`o_powerup_lvl#`):
- turns off the timer, then changes to stage select after 200 steps
- the timed stage select transition seems to be overwritten by the gameover transition?

Thus, hypothetical perfect inputs and loading time makes doing the cutscenedeathskipthing maximally 200 - 105 = 95 frames = 19/12 or \~1.58 seconds faster than not.
