### Contents

- [Hazards](#hazards-o-hazard-prefix)
    - [Beams](#beams)
        - [`_dragon` prefix](#dragon-prefix)
        - [`_orb` prefix](#orb-prefix)
    - [Fire](#fire)
        - [`_chainfire` prefix](#chainfire-prefix)
        - [`_fire` prefix](#fire-prefix)
    - [Orbsd \[sic\] (other `_orb` prefix)](#orbsd-sic-other-orb-prefix)
    - [Other](#other)

---

## Hazards (`o_hazard` prefix)
- `o_hazard_hazard` (parent class) deals 1 damage, is destroyed on contact

### Beams
- all beams deal 10 damage and persist on contact with player

#### `_dragon` prefix
- `_beam`
    - after 20 frames, get `hspeed = 4` and create `_beam_trail` every 2 frames after
    - on hitting a wall, it creates `_beam_2` and destroys itself
- `_beam_2` spawns at the same y-level as the player
    - after 10 frames, get `hspeed = -3` and create `_beam_trail` after 3 frames (probably a typo?), then every 2 frames after
    - on room exit, destroys itself
- `_beam_trail` deals 10 damage, persists on contact
    - after 20 frames, destroys itself

#### `_orb` prefix
- `_lazer`
    - after 60 frames, move towards the player at a speed of 5, then create `_trail` every frame after
    - destroys itself on contact with any block or room exit
- `_lazer_2` same as above, but its speed is 3, it flashes slightly differently, and it creates `o_hazard_orb_trail_2`
- `_trail` 
    - after 30 frames, destroys itself
- `_trail_2` does the same (and flashes slightly differently)

### Fire

#### `_chainfire` prefix
- `_drop_left` and `_drop_right` deal 10 damage, persist on contact, and have `vspeed = 2`
    - hitting any block creates `_pillar_left` or `_pillar_right` respectively and destroys itself
- `_pillar_left` and `_pillar_right` are 2 blocks tall
    - after 6 frames, if there is an available platform 1/2 a block to its right or left, it creates a new `_pillar_left` or `_pillar_right`
    - destroyed at animation end
        - sprite is 14 images, image speed is 0.8

#### `_fire` prefix
- all deal 5 damage, persist on contact
- `_drop` has `vspeed = 2`
    - hitting any block creates `_pillar` and destroys itself
- `_pillar` is 2 blocks tall
    - destroyed at animation end
        - sprite is 9 images, image speed is 0.4

### Orbsd \[sic\] (other `_orb` prefix)
- all deal 5 damage, destroyed on contact with player or outside room
- the fabled `o_hazard_orb` moves towards the player at a speed of 1
- `_2` has a direction of `instance_number` \* 90 + 45 (4 diagonals) and a speed of 2
- `_triplet_1` and `_triplet_2` have direction of 310 + (`instance_number` \* 25) and a speed of 2
- `_counter` moves towards the player at a speed of 2
- `_line` spawns at an x of 8 + `instance_number` \* 32
    - after 60 + `instance_number` * 20 frames, move toward the player again at a speed of 2
    
### Other
- `_block` deals 5 damage, persists on contact, starts with 0.1 gravity
    - stops moving on contact with floor or player
    - destroyed at animation end
        - sprite is 6 images, image speed is 0 on creation, 0.5 on collision with floor/player
- `_bounce` deals 10 damage, destroyed on contact, has `hspeed = -1` and 0.1 gravity
    - after 260 frames, destroy itself
    - colliding with a block sets `vspeed = -4`
- `_thunder` deals 10 damage, persists on contact
    - creates another instance of itself below as long as there is `place_free` (read: air, **non-solid platforms**) and it's within the room boundaries
    - if there is not a space free, creates `_shock` in the current location
    - after 15 frames, destroys itself
- `_shock` deals 10 damage, persists on contact
    - after 85 frames, destroys itself