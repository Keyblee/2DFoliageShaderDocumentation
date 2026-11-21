# Presets

A preset is an ordinary material labelled `FoliagePreset`. Applying one copies all its values and
switches onto your material; nothing else is linked, so you can keep editing afterwards.

## Using a preset

1. Select a strip and open its material in the Inspector.
2. Choose a style in **Style Preset** at the top. The list shows every preset material of that
   family by name (*Grass_Cartoon*, *Grass_Autumn*…), including your own.
3. The shipped materials, defaults included, are shared presets. When you pick a style while the
   object still uses one of them, Unity asks:
   - **Create a Copy**: a new material with the chosen style, saved next to your scene (in `Assets`
     if the scene isn't saved yet) and assigned to the selected objects. Choose this almost always.
   - **Change the Preset**: edits the shared preset itself, so every object using it changes.
   - **Cancel**.
4. Once the object has its own material, picking another style just restyles it.

The copy is named after the style, for example *Grass_Cartoon Copy*.

**Edit copies, not the shipped materials.** Changing a value directly on a shipped material (the
defaults included) changes every strip that uses it, and a later update of the pack can overwrite
your edits. Pick a style with **Create a Copy** first, or duplicate the material, and edit that.

A custom Brush Noise Texture is kept when you switch style, as long as the preset uses the pack's
FoliageNoise.

## Making your own

1. Tune a material until you like it.
2. Press **Save as Preset…** next to Style Preset and choose a name and folder.
3. The copy is labelled `FoliagePreset` and appears in Style Preset for every material of the same
   shader. To turn any existing material into a preset, add the `FoliagePreset` label to it in the
   Inspector (the label button at the bottom of the asset preview).

From a script, `FoliagePresets.Apply(target, preset)` does the same in the Editor and in builds: see
[Scripting API](scripting-api.md#foliagepresets).

## Shipped presets

All in `Materials/<Family>/Presets`, except the three defaults in `Materials/<Family>`. "Cartoon_"
variants use the variant's colours on the Cartoon look.

### Grass

| Material | Look |
|---|---|
| Grass_Default | Natural green, flowers on, no style switches. |
| Grass_Cartoon | Cartoon style. |
| Grass_PixelArt | Pixel Art style, 16 art pixels per unit. |
| Grass_Painted | Painted style. |
| Grass_Realistic | Realistic style. |
| Grass_Flat | Flat style. |
| Grass_Spring | Bright fresh green with pink flowers. |
| Grass_Autumn | Orange and brown. |
| Grass_Dry | Straw yellow, pale flowers. |
| Grass_Snow | Frosted green-white tips. |
| Grass_Fantasy | Purple roots, cyan tips, magenta flowers. |
| Grass_Cartoon_Spring, Grass_Cartoon_Autumn, Grass_Cartoon_Dry, Grass_Cartoon_Snow, Grass_Cartoon_Fantasy | The colour variants on the Cartoon look. |

### Bushes

| Material | Look |
|---|---|
| Bush_Default | Natural green, no style switches. |
| Bush_Cartoon | Cartoon style. |
| Bush_PixelArt | Pixel Art style. |
| Bush_Painted | Painted style. |
| Bush_Realistic | Realistic style. |
| Bush_Flat | Flat style. |
| Bush_Spring | Bright fresh green. |
| Bush_Autumn | Orange and brown. |
| Bush_Fantasy | Purple and cyan. |
| Bush_Cartoon_Spring, Bush_Cartoon_Autumn, Bush_Cartoon_Fantasy | The colour variants on the Cartoon look. |

### Vines

| Material | Look |
|---|---|
| Vine_Default | Natural green, no flowers. |
| Vine_Cartoon | Cartoon style. |
| Vine_PixelArt | Pixel Art style. |
| Vine_Painted | Painted style. |
| Vine_Realistic | Realistic style. |
| Vine_Flat | Flat style. |
| Vine_Jungle | Deep jungle green. |
| Vine_Autumn | Orange and brown. |
| Vine_Fantasy | Purple and cyan. |
| Vine_Moss | Grey-green, like hanging moss. |
| Vine_Blossom | Fresh green with pink flowers (Flowers on). |
| Vine_Cartoon_Jungle, Vine_Cartoon_Autumn, Vine_Cartoon_Fantasy, Vine_Cartoon_Moss, Vine_Cartoon_Blossom | The colour variants on the Cartoon look. |

The prefabs that use these materials are listed in [Getting Started](getting-started.md#2-pick-a-style-or-a-plant-type).
