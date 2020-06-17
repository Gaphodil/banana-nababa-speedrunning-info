# Bosses (`o_en` prefix) 

### Contents

- [Voodoo Mask](#voodoo-mask-_mask-prefix)
    - [Phase 1](#phase-1-no-suffix)
    - [Phase 2](#phase-2-_2)
    - [Phase 3](#phase-3-_3)
- [Swarm Lord](#swarm-lord-_dragon-prefix)
    - [Phase 1](#phase-1-no-suffix-1)
    - [Phase 2](#phase-2-_2-1)
- [Plague Beetle](#plague-beetle-_dark-prefix)
    - [Phase 1](#phase-1-no-suffix-2)
    - [Phase 2](#phase-2-_2-2)
    - [Phase 3](#phase-3-_3-1)
- [Giga Slith](#giga-slith-_slith-prefix)
    - [Phase 1](#phase-1-no-suffix-3)
    - [Phase 2](#phase-2-_2-3)
- [Gatekeeper](#gatekeeper-_gate-prefix)
    - [Phase 1](#phase-1-_1)
    - [Phase 2](#phase-2-_2-4)
    - [Phase 3](#phase-3-_3-and-_4)
- [False Raiden](#false-raiden-_abo-prefix)

---

## Voodoo Mask (`_mask` prefix)
- has a 4x2 tiles (64x32 pixels) sprite regardless of phase and actual collision

### Phase 1 (no suffix)
- hp = 50, `xplodes` = 12
- after 50 frames, starts the music
- after 100 frames, does alarm 0
    - alarm 0 (1/4): create two [`o_hazard_orb`](BN_hazards.md#orbsd-sic-other-orb-prefix)
        - after 90 frames, `round(random(2))` one of alarms `0, 1, 2` 
    - alarm 1 (1/2): `round(random(1))` which side "fake minion" is on
        - `_minion_fake` 
            - destroyed at animation end
                - sprite has 22 images, image speed is 0.3
        - `_minion_true` 
            - after 60 frames, create [`o_hazard_chainfire_drop_left`](BN_hazards.md#chainfire-prefix) if on the left, otherwise `_right` 
            - destroyed at animation end
                - sprite has 22 images, image speed is 0.2
        - after 200 frames, return to alarm 0
    - alarm 2 (1/4): "Create the hominh \[sic\] orb line" (6 \* `o_hazard_orb_line`)
        - after 240 frames, return to alarm 0
- on death: start exploding after 1 frame
    - continue exploding every 6 frames for `xplodes`/0.5 = 24 explosions 
    - create `o_powerup_hp` on the last 3 explosions (starts 20 frames before phase change)
    - after 2 frames, creates `_2`
    
### Phase 2 (`_2`)
- hp = 100, `xplodes` = 12
- after 100 frames, does alarm 0
    - alarm 0 (1/4): create two [`o_hazard_orb_lazer`](#orb-prefix)
        - if there is no `_medusa` then create one
        - `_medusa` deals 5 damage, persists on contact (though you can't touch it anyway)
            - after `random(100)` + 100 frames, create [`o_hazard_fire_drop`](#fire-prefix) and repeat after a new `random(100)` + 100 frames
            - only starts moving after animation finishes 
                - sprite has 11 images, image speed is 0.4
            - bounces horizontally off walls
            - drops `o_powerup_hp` if hit by a weapon
        - after 140 frames, `round(random(2))` one of alarms `0, 1, 2` 
    - alarm 1 (1/2): same as previous phase
        - after 260 frames, return to alarm 0
    - alarm 2 (1/4): creates two `_minion_fake` and two `o_hazard_orb_lazer` from the same spot
        - after 150 frames, return to alarm 0
- on death: start exploding after 1 frame
    - continue exploding every 6 frames for `xplodes`/0.5 = 24 explosions 
    - create `o_powerup_hp` on the last 3 explosions (starts 20 frames before phase change)
    - after 2 frames, creates `_3`

### Phase 3 (`_3`)
- hp = 40, `xplodes` = 16
- deals 5 damage, persists on contact
- after 100 frames, does "Jump attack"
    - `vspeed = -5`
    - `hspeed` depends on player position; left is -0.5, right is 0.5, **equal is 0**
    - after 110 frames, repeats
- bounces horizontally off walls
- after collision with floor, create `o_hazard_chainfire_drop`s in each direction
- on death: start exploding after 1 frame
    - continue exploding every 6 frames for `xplodes`/0.5 = 32 explosions 
    - create [`o_fx_hurt`](BN_effects.md) (flashing the screen) on `xplodes = `6, 4, and 3 (80, 56, 44 frames before orb)
    - after 2 frames, creates orb centered 3 tiles above the central platform

---

## Swarm Lord (`_dragon` prefix)
- has a 5x3 tiles (80x48 pixels) sprite

### Phase 1 (no suffix)
- hp = 60, `xplodes` = 18, `minions` = 8
- after 70 frames, starts the music
- after 100 frames, does alarm 0
    - alarm 0 (1/2): create two `_minion_2`
        - `_minion_2` deals 10 damage, destroyed on contact
            - starts with an `hspeed` of 2
            - on hitting a wall, move towards player with a speed of 1
            - drops `o_powerup_hp` if hit by a weapon
            - after 60 frames, does alarm 0, which... doesn't exist
        - stops moving normally (`vspeed` and `hspeed = 0`)
        - move onto roughly circular movement cycle if it hasn't yet (`pth_dragon`)
        - after 90 frames, `round(random(1))` one of alarms `0, 1`
    - alarm 1 (1/2): 
        - if `minions > 0`
            - subtract one and create `_minion_3`
                - `_minion_3` deals 10 damage, destroyed on contact
                    - follows `pth_dragon_minion` (a better circle)
                    - after 200 frames, ends path and moves towards player with a speed of 2
            - repeat every 20 frames (total 160 frames)
        - else
            - `minions = 8`
            - after 200 frames, does alarm 0 
- every 200 frames, creates [`o_hazard_dragon_beam`](BN_hazards.md#dragon-prefix)
- on death: start exploding after 1 frame
    - destroy all minions
    - continue exploding every 6 frames for `xplodes`/0.5 = 36 explosions
    - after 5 frames, creates `_2`

### Phase 2 (`_2`)
- hp = 70, `xplodes` = 18, `minions` = 6
- deals 5 damage, persists on contact (but you'd be silly to touch it)
- after 80 frames, stops moving, starts `pth_dragon`
- after 120 frames, does alarm 1
    - alarm 1: 
        - stop moving along path
        - if `minions > 0`
            - subtract one and create `_minion_4`
                - `_minion_4` deals 10 damage, destroyed on contact
                    - follows `pth_dragon_minion_2` (identical to `pth_dragon_minion` but starts 90 degrees further clockwise)
                    - after 300 frames, does alarm 0 (it shouldn't ever be called by this)
                        - alarm 0: stop path, move diagonally down-right with `hspeed` and `vspeed = 2`
            - repeat every 10 frames (total 60 frames)
        - else
            - for every `_minion_4` call its alarm 0 after 5 frames
            - `minions = 6`, resume path
            - after 160 frames, repeat alarm 1
- after 40 frames, does alarm 2
    - alarm 2: creates `_minion` and `_minion_2`
        - `_minion` deals 10 damage, destroyed on contact
            - moves towards player with a speed of 1
            - after 60 frames, moves towards player again
            - is oddly worth 68 score where other minions have 40
        - after 140 frames, repeats
- on death: start exploding after 1 frame
    - stop alarms 0-4 (though 3 and 4 don't exist)
    - destroy all `_minion_4` (but not the other ones)
    - continue exploding every 6 frames for `xplodes`/0.5 = 36 explosions
    - create `o_fx_hurt` on `xplodes = `6, 4, and 3 (80, 56, 44 frames before orb)
    - after 2 frames, creates orb 1(?) tile above the ground on the furthest left platform

---

## Plague Beetle (`_dark` prefix)
- has a 2x2 tiles (32x32 pixels) sprite

### Phase 1 (no suffix)
- hp = 60, `xplodes` = 18
- deals 10 damage, persists on contact
- after 30 frames, starts the music
- after 60 frames, creates two solid blocks to fill the invisible hole in the ceiling the boss came in through
- after 120 frames, does alarm 0
    - alarm 0: hops towards the player (more accurate than voodoo mask) with hspeed magnitude of 1
        - `vspeed = -6`, `gravity = 0.2`
        - creates `_curse`
            - `_curse` deals 5 damage, destroyed on contact
                - moves towards the player with a speed of 1
                - after `random(20)+2` frames, chase the player again
                - after 8 frames, chase the player again (twice) for a total of 4 times over 18 to 38 frames
                - activates player's attack cooldown for 80 frames regardless of iframes
                - never explicitly destroyed unless it touches the player (uh oh!)
        - after 10 frames, does alarm 3
        - after 100 frames, repeats
    - alarm 3: creates `_slime`
        - `_slime`
            - starts with `vspeed = -3`, `hspeed = random(1)-random(1)`, and `gravity = 0.1`
            - hits a block with a tiny amount of friction
            - explodes on hitting a wall
            - causes the player to be unable to move horizontally while colliding
- every 800 frames, does alarm 1
    - alarm 1: shoot straight up with `vspeed = -8`
        - after 200 frames, does alarm 0 (overriding previous timer)
        - after 60 frames, does alarm 2
    - alarm 2: create 3 \* [`o_hazard_block`](BN_hazards.md#other) at `16+random(200)` along the ceiling
- bounces horizontally off walls
- on death: start exploding after 1 frame
    - stops alarms 0, 1, 2
    - continue exploding every 6 frames for `xplodes`/0.5 = 36 explosions
    - after 5 frames, creates `_2`

### Phase 2 (`_2`)
- hp = 60, `xplodes` = 18, `gravity_direction` = 90 (upward)
- deals 10 damage, persists on contact
- after 60 frames, does alarm 4 (nothing)
- after 120 frames, does alarm 0
- every 800 frames, does alarm 1
    - alarms 0, 1 identical to previous phase except `vspeed` reversed
    - alarm 2: create 2 \* `_minion`
        - `_minion` does 5 damage, destroyed on contact
            - starts with `vspeed = 3` (falling entry from ceiling)
            - after 30 frames, does alarm 0
                - alarm 0: move towards player with a speed of 0.5, then repeat every 4 frames
            - after 500 frames, creates `o_powerup_hp` and destroys itself
            - when hit with a weapon, delay alarm 0 to 40 frames and `hspeed = -hspeed*5` (which sometimes makes the minion shoot out of bounds)
    - alarm 3: creates `_slime` and `_slime_minion`
        - `_slime_minion`
            - starts with `vspeed = -3`, `hspeed = random(1)-random(1)`, and `gravity = 0.1`
            - hits a block and retains `hspeed`
            - explodes on hitting a wall
            - causes the player to be unable to move horizontally while colliding
- bounces horizontally off walls
- on death: start exploding after 1 frame
    - stops alarms 0, 1, 2
    - continue exploding every 6 frames for `xplodes`/0.5 = 36 explosions
    - after 5 frames, creates `_3`
    
### Phase 3 (`_3`)
- hp = 30, `xplodes` = 18
- deals 10 damage, persists on contact
- speed of 1, initial direction of `random(360)`
- after 120 frames, does alarm 0
    - alarm 0: create `_curse` and `_pillar`, repeat every 80 frames
        - `_pillar` deals 5 damage, persists on contact
            - starts with `vspeed = -3`, `hspeed = random(1)-random(1)` (if 0, set to -1), and `gravity = 0.1`
            - hitting floor or ceiling switches sprite, retains `hspeed`
            - hitting the ceiling makes the upside-down version
            - explodes on hitting a wall
            - invincible to weapons but provides more score than normal when hit
- after 400 frames, does alarm 1
    - alarm 1: create 2\* `_minion`, repeat every 600 frames
- colliding with a wall increases `direction` by 45 degrees (counterclockwise)
- colliding with a solid block (floor, ceiling) increases `direction` by `random(45) + random(20) + 5` (5 to 70, estimated avg. 25 to 50) degrees
- on death: start exploding after 1 frame
    - stop alarms 0, 1, 2 and remove all damaging attacks
    - continue exploding every 6 frames for `xplodes`/0.5 = 36 explosions
    - create `o_fx_hurt` on `xplodes = `6, 4, and < 2 (77, 53, 23, 17, 11 frames before orb)
    - after 5 frames, creates orb centered 2 tiles above the ground

---

## Giga Slith (`_slith` prefix)
- each segment has a 1x1 tile (16x16 pixels) sprite

### Phase 1 (no suffix)
- hp = 40, `xplodes` = 18
- deals 5 damage, persists on contact
- after 50 frames, starts the music
- every 8 frames, create `_body`
    - `_body` deals 5 damage, persists on contact
    - after 128 frames, destroys itself
    - when hit, create [`o_hazard_orb_counter`](BN_hazards.md#orbsd-sic-other-orb-prefix)
- every 64 frames, create 4 \* `o_hazard_orb_2`
- after 32 frames, do alarm 2
    - alarm 2: pick one of 10 paths from `round(random(9))` with a speed of 1; internal naming is 1 greater than random variable
        - (for the following rudimentary path descriptions, the 6 holes in the arena are numbered 1-6 from left to right and represent entering or exiting, the two platforms are labelled upper and lower)
        - path 1 (1/18): 1 -> right across upper platform -> 6 -> 4 -> left across lower platform -> 3
        - path 2 (1/9): 1 -> 3 -> 4 -> 6
        - path 3 (1/9): 1 -> above upper platform height then down -> right across ground -> above upper platform height then down -> 6
        - path 4 (1/9): 1 -> lower platform height -> right across upper platform -> lower platform height -> 6
        - path 5 (1/9): 1 -> upper platform height -> diagonal down to ground -> diagonal up from ground -> upper platform height -> 6
        - path 6 (1/9): 6 -> straight left -> 1
        - path 7 (1/9): 6 -> above upper platform height -> down through right side of platforms to lower platform -> up through left side of platforms -> 1
        - path 8 (1/9): 6 -> 5 -> 4 -> 3 -> 2 -> 1
        - path 9 (1/9): 6 -> left at lower platform height -> 1
        - path 10 (1/18): 6 -> vaguely heart shaped -> diagonal down to lower platform -> diagonal up from lower plaform -> vaguely heart shaped -> 1
- after exiting the room (end of a path), do alarm 2 after 130 frames
- on death: start exploding after 1 frame
    - stops alarms 0, 1, 2
    - continue exploding every 6 frames for `xplodes`/0.5 = 36 explosions
    - after 5 frames, creates `_2`

### Phase 2 (`_2`)
- hp = 40, `xplodes` = 18
- deals 5 damage, persists on contact
- creates `_minion_left` and `_minion_right`
    - `_minion_left` rise from the ground with a `vspeed` of -1
        - after 80 frames, stop moving and do alarm 1 after 30 frames
            - alarm 1: create `o_hazard_orb`, repeat every 300 frames
        - after 500 frames, spawn 3 groups of 2 \* `_minion` with 40 frames in between (and after), then repeats after 500 frames
            - `_minion` deals 5 damage, persists on contact
                - creates `o_powerup_hp` when hit
        - when hit, cause `malfunction` for 20 \* 5 frames, delaying alarm 1 for the same amount of time; will refresh on hit
        - `_minion_right` does all the same except `_minion` spawning
- every 8 frames, create `_body`
- every 64 frames, create 4 \* `o_hazard_orb_2`
- after 32 frames, do alarm 2 (same paths and chances with a speed of 1.5)
- on death: start exploding after 1 frame
    - stop and remove all attacks except `_minion`; side minions sink under the ground
    - continue exploding every 6 frames for `xplodes`/0.5 = 36 explosions
    - does not create any `o_fx_hurt` 
    - after 5 frames, creates orb centered two tiles above the upper platform
---

## Gatekeeper (`_gate` prefix)
- each side has a 1x5 tiles (16x80 pixels) sprite with a 21 pixel high hurtbox
- both phases 1 and 2 are already present
- can only be hit when `image_index < 4` (immune during 2 subimages of eye closing/closed)
    - `image_speed` = 0.1 (changes every 10 frames) -> 20 frames of invincibility every 60 frames

### Phase 1 (`_1`)
- hp = 40, `xplodes` = 12, `shots` = 4
- after 80 frames, starts the music
- after 100 frames, do alarm 0 (which repeats every 200 frames and does nothing else)
- after 200 frames, do alarm 1
    - alarm 1: if `shots > 1`:
        - subtract by one
        - if `shots = 4` create 3 \* [`o_hazard_orb_triplet_1`](BN_hazards.md#orbsd-sic-other-orb-prefix) from the top section
        - if `shots = 3` create 3 \* `o_hazard_orb_triplet_2` from the bottom section
        - if `shots = 2` create [`o_hazard_orb_lazer_2`](BN_hazards.md#orb-prefix) from the top section
        - if `shots = 1` create `o_hazard_orb_lazer_2` from the bottom section
        - repeat after 15 frames
        - else, `shots = 5` and repeat after 300 frames
- every 300 frames, create `_minion_right`
    - `_minion_right` deals 5 damage, destroyed on contact
        - after 60 frames, sets `vspeed = -3`, `hspeed = 0.5`, and `gravity = 0.1`
        - collision with floor sets `vspeed = -4`
        - destroyed on collision with wall
        - creates `o_powerup_hp` when hit by weapon
- on death: start exploding after 1 frame
    - continue exploding every 6 frames for `xplodes`/0.5 = 24 explosions
    - after 5 frames, closes eye, activates `_2` after 100 frames

### Phase 2 (`_2`)
- hp = 40, `xplodes` = 12, `shots` = 4
- after 100 frames, do alarm 1 (identical but replace both `_triplet_#` with `_bounce`)
    - this still makes 3 copies of `_bounce` despite them layering on top of each other and effectively being only one
- every 300 frames, create `_minion_left` (identical but reversed `hspeed`)
- on death: start exploding after 1 frame
    - continue exploding every 6 frames for `xplodes`/0.5 = 24 explosions
    - after 5 frames, destroys `_1` and `_2`, creates `_3` and `_4`

### Phase 3 (`_3` and `_4`)
- hp = 60, `xplodes` = 22 (12 on `_4`, unused), `shots` = 4 (on both)
- `_3` alarms identical to `_1`
- `_4` alarms identical to `_2`
- on death: start exploding after 1 frame
    - stop alarms 0, 1, 2 on both sides and remove all attacks (except `_triplet`s)
    - continue exploding every 6 frames for `xplodes`/0.5 = 44 explosions
    - create `o_fx_hurt` on `xplodes < 4` (47, 41, 35, 29, 23, 17, 11 frames before orb)
    - after 5 frames, creates orb centered two tiles above top platform

---

## False Raiden (`_abo` prefix)
- hp = 190, `xplodes` = 28
- after 120 frames, starts the music
- after 220 frames, does alarm 0
    - alarm 0: records player x in `strike`, flashes eyes
        - after 35 frames, `round(random(4))+1` one of 5 alarms 
    - after cooldowns, return to warning
    - alarm 1 (1/8): burst of 10 \* `_360`
        - cooldown 100 frames
        - `_360` deals 5 damage, destroyed on contact
            - direction of `instance_number` \* 36
            - speed of 2, -0.05 gravity
    - alarm 2 (1/4): spawns `_strike` at (`strike`, 200)
        - cooldown 50 frames
        - `_strike` does no damage, spawns on player y anyways
            - after 35 frames, creates [`o_hazard_thunder`](BN_hazards.md#other) from the top of the screen
    - alarm 3 (1/4): `round(random(1))` to choose side of screen, creates either `_summon` at left (1/2) or `_summon_3` at right (1/2)
        - cooldown 140 frames
        - `_summon`(`_3`)) deals 5 damage, destroyed on contact, has speed of 0.8 or -0.8
            - after 16 frames, spawns `o_hazard_thunder` from the top then repeats every 8 frames
            - can be destroyed by weapons
    - alarm 4 (1/4): create 3 `_summon_2`
        - cooldown 100 frames
        - `_summon_2` deals 5 damage, destroyed on contact
            - moves towards player with a speed of 3, direction changed by `random(10)-random(10)`
            - spawns `o_hazard_thunder` from the top if it hits a solid block
            - can be destroyed by weapons
    - alarm 5 (1/8): summons `_fire` every 5 frames
        - first time shoots 12, subsequent shoots 10
        - final 2 shots create healing items
        - cooldown 120 frames after all shots fired
            - `_fire` deals 5 damage, destroyed on contact
                - moves towards player with a speed of 2.5, direction changed by `random(5)-random(5)`
- on death: start exploding after 1 frame
    - clear all enemy attacks
    - continue exploding every 6 frames for `xplodes`/0.5 = 56 explosions 
    - last 9 (`0 < xplodes < 5`) explosions cause a hurt sfx
    - after 5 frames, spawn orb
    - total frames after last hit: 1 + 56\*6 + 5 = 342 frames
