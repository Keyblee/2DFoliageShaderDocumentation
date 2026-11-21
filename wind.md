# Wind

Wind moves every strip in the scene at once. It is computed in the shader from a few global values,
so it costs no CPU per plant and works in Edit mode too.

Without any wind component the plants sway in a gentle built-in breeze. Add a **Foliage Wind** to
control the wind, and **Foliage Wind Zone 2D** boxes for local gusts or shelter.

## Foliage Wind

Add it to any object, for example an empty *Wind* object (**Add Component > Foliage 2D > Foliage
Wind**). One is enough for the scene: if several are enabled, the last one enabled is used and the
others wait (the console says so).

| Setting | What it does |
|---|---|
| Strength | Base wind, -3 to 3. Positive blows right, negative blows left. |
| Gust Strength | Extra strength of gusts on top of the base wind. |
| Gust Speed | How fast gusts travel across the world, in units per second. |
| Gust Scale | Size of a gust in world units. Larger gives wider, slower-looking waves. |
| Turbulence | Small, quick per-plant sway. It drives the materials' Tip Flutter and Leaf Flutter. |

Gusts travel across the world, so plants further along feel the same gust a moment later, like
wind running over a field. For a one-off gust (a door opening, a passing train), call
`Gust(strength, duration)` from a script: see [Scripting API](scripting-api.md#foliagewind).

## Foliage Wind Zone 2D

A box that changes the wind inside it (**Add Component > Foliage 2D > Foliage Wind Zone 2D**). Drag
its handles in the Scene view to size it. Zones are always axis-aligned; the object's rotation is
ignored. Up to 8 zones are active at a time (`FoliageGlobals.MaxZones`); further ones are ignored
with a console warning.

| Setting | What it does |
|---|---|
| Size | Width and height of the zone in local units, centred on the object. |
| Strength | Wind inside the zone, -3 to 3. Positive blows right. |
| Mode | **Add**: on top of the scene wind (a windy gap between buildings). **Override**: replaces it; Strength 0 makes a shelter (a cave, behind a wall). |
| Edge Softness | How gradually the zone fades out towards its edges, so plants don't jump as they cross it. |

## Wind

Each material decides how much it follows the wind. The Wind section has the same settings on every
family, except where noted.

| Setting | What it does |
|---|---|
| Wind Response | How much this material follows the wind (Foliage Wind, zones and gusts). 0 ignores wind; 2 is very floppy. |
| Tip Flutter | Grass only. Quick extra wobble at the blade tips, driven by the wind's Turbulence. |
| Leaf Flutter | Bushes and vines. Quick extra wobble of each leaf, driven by the wind's Turbulence. |
| Swing | Vines only. Slow pendulum swing of the hanging vines; their lower ends swing the most. 0 means they only move with the wind. |

Tips:

- Give bushes a lower Wind Response than grass (the shipped defaults do): they are heavier.
- For a calm scene keep Strength low and raise Turbulence a little: plants stay alive without
  leaning.
- Stiffness varies from plant to plant and reeds are stiffer than blades, so a field never moves as
  one block.
