Battle
  Story
    Client wants to simulate battles between two military forces.
    Battlefields consist of grups of units of having different weapons.
    Some units may be ranked heroic because they perform better.
    After the battle the last battle survival report may be fetched.
  Models legend
    + — required
    * — nullable
    many, one — associations
    (...) — set
  Models
    Battlefield
      status: string (pre, ongoing, post)
      created_at: datetime
      updated_at: datetime
    Group
      +one: Battlefield
      name: string
      side: string (allies, axis)
    Unit
      +one: Group
      +many: Weapon
      *one: Hero
      name: string
    Hero
      rank: integer (1, 2, 3)
    Weapon
      +many: Unit
      type: string (light, medium, heavy)
  API
    POST /api/battle — prepares battle
    PUT/PATCH /api/battle — starts prepared battle and runs the simulation
    GET /api/battle — fetches last battle survivors
  Logic
    Army units attack alternately.
    Units attack in ascending order of their id.
    Each regular unit has 1.0 health points. Heroes have 2.0 heath points.
    Unit attack damage depends on what of opponent it is attacking and what weapons it has:
      $attacker vs $defender: $damage
      light vs light: 0.3
      light vs medium: 0.4
      light vs heavy: 0.5
      medium vs light: 0.4
      medium vs medium: 0.3
      medium vs heavy: 0.4
      heavy vs light: 0.5
      heavy vs medium: 0.4
      heavy vs heavy: 0.3
    Heroes get their damage increased by a double of decimal part of their rank, e.g.:
      rank 1, base damage 0.3 -> damage 0.5
      rank 2, base damage 0.4 -> damage 0.8
      rank 3, base damage 0.5 -> damage 1.1
    Heroes may have two weapons, the best is picked.
    Battlefields get created with groups, units and heroes.
    Groups and units are randomized in a queue.
    Fights take place until one military force is destroyed.

