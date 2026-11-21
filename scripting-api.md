# Scripting API

Everything is in the `SebastianEscobar.FoliageShaders2D` namespace. Editing a strip's properties
from code rebuilds its mesh immediately; the wind and interaction values are global and cost nothing
per plant.

```csharp
using SebastianEscobar.FoliageShaders2D;
```

## FoliageStrip

Base class of the three strip components. Abstract: add a GrassStrip, BushStrip or VineStrip.

| Member | Description |
|---|---|
| `Vector2 Size` | Width and height in local units. Under a pixel-art material it reads back snapped to whole art pixels; the value you set is kept. |
| `int Seed` | Random seed. Same seed, same plants. |
| `float Density` | Plants (or vines) per unit of width. Bush strips use Spacing instead. |
| `int SortingLayerID` | Sorting layer of the renderer. |
| `int SortingOrder` | Order in layer. |
| `int CardCount` | Number of cards (blades, leaves, flowers…) in the current mesh. |
| `bool WasTruncated` | True when the strip asked for more plants than one mesh holds and was cut down. |
| `void Randomize()` | Picks a new random seed and rebuilds. |
| `void Rebuild()` | Rebuilds the mesh now. Setting a property already does this; call it after changing the material's pixel-art settings from code. |
| `const float MinSize` | Smallest width or height, 0.05. |
| `const float MaxDensity` | Largest Density, 200. |

```csharp
var grass = GetComponent<GrassStrip>();
grass.Size = new Vector2(12f, 1.2f);   // rebuilds
grass.Randomize();                      // new arrangement
```

## GrassStrip

| Member | Description |
|---|---|
| `Vector2 HeightRange` | Blade height range as a fraction of the strip height (min, max). |
| `float BladeWidth` | Average blade width in local units. |
| `float Clumping` | 0–1: from evenly spread to tight clumps. |
| `float Lean` | Base lean in degrees, -30 to 30. |
| `float GapChance` | 0–1: chance that a blade is skipped. |
| `int Segments` | Segments per blade, 1–6. |
| `float FlowerRatio` | 0–1: share of plants that are flowers. |
| `float ReedRatio` | 0–1: share of plants that are reeds. |

## BushStrip

| Member | Description |
|---|---|
| `float Spacing` | Distance between bushes in local units. |
| `Vector2 BushSize` | Size of each bush as a fraction of the strip height. |
| `int LeavesPerBush` | 4–200. |
| `BushShape Shape` | `BushShape.Round`, `BushShape.Tall` or `BushShape.Creeping`. |

## VineStrip

| Member | Description |
|---|---|
| `Vector2 LengthRange` | Vine length range as a fraction of the strip height (min, max). |
| `float LeavesPerUnit` | Leaves per unit of vine length, 0–30. |
| `float FlowerRatio` | 0–1: share of leaf spots that carry a flower. |
| `float StemWidth` | Stem width in local units, 0.005–0.2. |
| `float LeafScale` | Leaf and flower size, 0.5–2.5. |

## FoliageWind

| Member | Description |
|---|---|
| `static FoliageWind Active` | The wind in use (the last one enabled), or null for the built-in breeze. |
| `float Strength` | Base wind, -3 to 3. Positive blows right. |
| `float GustStrength` | Extra strength of gusts, 0–2. |
| `float GustSpeed` | Gust travel speed, 0–10 units per second. |
| `float GustScale` | Gust size in world units, 0.5–30. |
| `float Turbulence` | Quick per-plant sway, 0–1. |
| `void Gust(float strength, float duration)` | A one-off gust: the wind rises by `strength` and falls back over `duration` seconds. |

```csharp
// A train passes: a strong gust to the right for 2 seconds.
FoliageWind wind = FoliageWind.Active;
if (wind != null)
    wind.Gust(2f, 2f);
```

Gusts need a Foliage Wind in the scene: the built-in breeze used without one can't gust.

## FoliageWindZone2D

| Member | Description |
|---|---|
| `Vector2 Size` | Zone size in local units, centred on the transform. |
| `float Strength` | Wind inside the zone, -3 to 3. |
| `ZoneMode Mode` | `ZoneMode.Add` (on top of the scene wind) or `ZoneMode.Override` (replaces it; 0 = shelter). |
| `float EdgeSoftness` | 0–1: how gradually the zone fades at its edges. |

## FoliagePusher2D

| Member | Description |
|---|---|
| `float Radius` | Push radius in world units. |
| `float Strength` | Push strength; 1 is a person walking. |
| `Vector2 Offset` | Offset from the transform position. |

Enable and disable the component to start and stop pushing.

## FoliageGlobals

| Member | Description |
|---|---|
| `static void Impulse(Vector2 position, float radius, float strength)` | A radial burst: plants within `radius` bend away from `position` and spring back. |
| `const int MaxSlots` | Interaction slots sent to the shaders each frame (16): pushers, impulses and trail points. |
| `const int MaxZones` | Wind zones active at a time (8). |

```csharp
void OnLanded(Vector2 feet, float fallSpeed)
{
    FoliageGlobals.Impulse(feet, 1.6f, Mathf.Clamp01(fallSpeed / 12f) * 1.2f);
}
```

## FoliagePresets

| Member | Description |
|---|---|
| `static void Apply(Material target, Material preset)` | Copies every value and switch of `preset` onto `target` (same shader). Keeps the target's own noise texture when the preset uses the pack's. Works in builds. |
| `const string Label` | `"FoliagePreset"`, the asset label that marks a material as a preset. |
| `const string DefaultNoiseName` | `"FoliageNoise"`, the pack's noise texture. |

To restyle at runtime, apply the preset to a copy of the material and give that copy to the strips.
Don't apply it to `sharedMaterial` directly: in the Editor that changes the material asset itself
(often a shipped preset), and the change stays after you leave Play mode.

```csharp
var autumn = new Material(stripRenderer.sharedMaterial);
FoliagePresets.Apply(autumn, autumnPreset);
foreach (FoliageStrip strip in strips)
    strip.GetComponent<MeshRenderer>().sharedMaterial = autumn;
```

Switching a pixel-art preset on or off changes the snapped size, so call `Rebuild()` on the strips
afterwards. In a build, make sure every effect combination you switch to is included (see
[Performance — Shader variants](performance.md#shader-variants)).

## FoliageShaderSections

Reads a foliage shader's sections (Color, Shape, Flowers, Wind…) from its own property attributes,
so custom editors and runtime UIs can list them. The demo's live material panel uses it.

| Member | Description |
|---|---|
| `static IReadOnlyList<FoliageShaderSection> Get(Shader shader)` | The shader's sections, in inspector order. |
| `static string ToggleKeyword(Shader shader, int propertyIndex)` | The keyword switched by a property's toggle, or null. |
| `static IReadOnlyList<string> EffectKeywords(Shader shader)` | Every effect keyword of the shader (Flowers, Pixel Art, Bands, Outline…). |
| `static bool IsEnabled(Material material, FoliageShaderSection section)` | Whether the section's effect is on in the material (always true for sections without a switch). |

## FoliageShaderSection

| Member | Description |
|---|---|
| `string Name` | Section name as the inspector shows it. |
| `string Keyword` | Keyword of the section's switch, or null. |
| `string ToggleProperty` | Float property of the switch, or null. |
| `IReadOnlyList<int> PropertyIndices` | Shader property indices of the section's settings. |
