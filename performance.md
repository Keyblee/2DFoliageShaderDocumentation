# Performance

Foliage Shaders 2D is built to be cheap on mobile:

- **One mesh per strip.** All the plants of a strip are cards in one mesh, drawn in one draw call.
  The mesh is built only when you change the strip, never per frame.
- **No CPU per plant.** Wind, gusts, wind zones and interaction are computed in the vertex shader
  from a few global values uploaded once per frame. With no pushers or impulses active, the
  interaction values aren't uploaded at all.
- **SRP Batcher compatible.** Materials keep their values in one constant buffer and nothing is set
  per renderer, so many strips with the same shader batch well.
- **Procedural shapes.** Blades, leaves and flowers are drawn by the fragment shader inside each
  card: no textures to load, except the brush noise, which is read only while Brush Strength is
  above 0.

## Measured

| Device | Scene | Cards | Result |
|---|---|---|---|
| Samsung Galaxy S23 FE (Android) | Stress test: grass with flowers and reeds, bushes, vines, wind and interaction | 5 118 | 120 fps (the screen's refresh rate), about 8.8 ms per frame |

A *card* is one blade, reed, flower stem, flower head or leaf. `FoliageStrip.CardCount` tells you
how many a strip has.

## Keeping it cheap

- **Cards are the main cost.** Lower Density (grass, vines), Leaves Per Bush and Leaves Per Unit
  before anything else. Plants far from the action can be sparser.
- **Segments**: a grass blade with 2 segments has 6 vertices, with 4 it has 10; on short grass most
  players won't see the difference.
- **Overdraw**: many overlapping strips cover the same pixels several times. Prefer one dense strip
  to three sparse ones stacked on top of each other.
- **Big strips** are fine: a strip that is partly off screen is still one draw call. Split very long
  levels into a few strips so off-screen ones are culled.
- **Pushers**: each active Foliage Pusher 2D takes an interaction slot. Disable the pushers of
  characters far off screen.
- One strip holds at most 65 000 vertices. Past that, the strip is cut down and its inspector warns
  you ([Troubleshooting](troubleshooting.md#too-many-plants)).

## Shader variants

Each effect switch (Flowers, Pixel Art, Bands, Outline, Ignore 2D Lights) is a shader keyword. A
build only contains the combinations your materials use, so:

- If you only use materials as they are, there is nothing to do.
- If your game switches effects at runtime, or applies presets from code, add the combinations it
  needs to a **Shader Variant Collection** and include it in **Project Settings > Graphics > Shader
  Loading > Preloaded Shaders**, or warm it up yourself. Otherwise the plant may draw with a
  different combination or briefly hitch while the variant compiles.
- The demo's `Demo/ShowcaseVariants.shadervariants` lists every combination of every foliage shader,
  as the live panel lets players combine all effects.
