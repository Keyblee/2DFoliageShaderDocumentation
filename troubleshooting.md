# Troubleshooting

## The plants are pink or invisible

- The project must use URP with the **2D Renderer** ([Pipeline Setup](pipeline-setup.md)). With the
  Universal (3D) Renderer or the Built-in pipeline the foliage doesn't draw yet.
- Check that the strip's Sorting Layer and Sorting Order put it in front of your background.

## The plants are too dark

They are lit by URP 2D lights, like your lit sprites. Check your **Global Light 2D** and other 2D
lights, or switch **Ignore 2D Lights** on in the material if the plants should not react to lights.

## The strip looks stretched or the plants lean strangely

The inspector warns: *Scale stretches the plants.* Keep the object's scale at 1 and use **Size** (or
the edge handles) instead. A scale of -1 on X mirrors the strip and is fine.

## Wind and bending go sideways

The inspector warns: *Rotation is not supported.* Wind and bending assume an upright strip. Set the
rotation back to 0 and mirror with a scale of -1 on X if you need to.

## Too many plants

The inspector warns: *Too many plants: density reduced to fit one mesh (65000 vertices).* Lower
Density (or Leaves Per Bush, Leaves Per Unit, Segments), or split the strip into several.

## Changing a style changed other objects too

You chose **Change the Preset** when Unity asked, so the shared preset material changed. Undo
(Ctrl+Z), then pick the style again and choose **Create a Copy** ([Presets](presets.md)).

## Plants don't bend away from the player

- The player needs a **Foliage Pusher 2D**, enabled ([Interaction](interaction.md)).
- Check the Radius and Offset: select the pusher and its green circle should overlap the plants.
- The material's Push Response must be above 0.
- With many pushers and impulses at once, only 16 interaction slots reach the shaders; disable
  pushers far off screen.

## There is no wind

- Without a Foliage Wind, plants sway in a gentle built-in breeze. Add one to control it
  ([Wind](wind.md)).
- The material's Wind Response must be above 0.
- *More than one FoliageWind is active*: only the last one enabled is used. Keep one per scene.
- *More than 8 FoliageWindZone2D are active*: zones past the eighth are ignored. Wind zones are
  always axis-aligned boxes; their rotation is ignored.

## Pixel-art plants look broken or blurry

- Match **Art Pixels Per Unit** to your sprites and add a **Pixel Perfect Camera**
  ([Pixel Art](pixel-art.md)).
- Blades thinner than two art pixels break up into single pixels: use the Pixel Art prefabs or raise
  Blade Width.

## Flowers don't show

Switch **Flowers** on in the material and raise **Flower Ratio** on the strip. Both are needed.

## In a build, an effect switched from code looks wrong

The build may lack that effect combination. Add it to a Shader Variant Collection
([Performance — Shader variants](performance.md#shader-variants)).
