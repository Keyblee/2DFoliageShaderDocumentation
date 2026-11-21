# Demo Scenes

Two scenes in `Demo/Scenes` show the pack in a small playable level. Open one and press Play.

- **Showcase**: a level with every family: front and back grass, tall grass, bushes, autumn bushes
  and hanging vines, lamps with 2D lights, wind with gusts, and a gusty corner made with a wind zone.
- **PixelArt**: the same idea in pixel art, with a Pixel Perfect Camera at 16 pixels per unit
  (reference resolution 320 × 180).

**Controls:** A/D or the arrow keys to run, Space to jump (W and Up also jump with the Input System
package). Run through the grass to
push it; land from a jump to thump it.

## The live material panel

The panel in the top corner edits the foliage materials while the game runs, in the Editor and in
player builds:

- **Plant**: which group of strips to edit (Grass, Tall Grass, Bushes, Autumn Bushes, Vines).
- **Style**: any preset of that family.
- Every effect switch and value of the shader, grouped like the material inspector. Effects can be
  combined freely.
- **Reset**: undoes your edits, back to the values of the chosen Style.
- **Night**: dims the global light, brightens the lamps and darkens the sky.
- **Open the Pixel Art demo** / **Open the Showcase demo**: switches between the two scenes.

The panel works on in-memory copies of the materials: your material assets are never changed, and
everything returns to normal when you stop Play mode.

## Demo components

These components are demo helpers (**Add Component > Foliage 2D > Demo > …**). You can reuse them,
but they are kept simple on purpose.

### Foliage Material Panel

**Foliage 2D > Demo > Foliage Material Panel**. Needs a UI Document with `Demo/UI/FoliageMaterialPanel.uxml`.

| Setting | What it does |
|---|---|
| Document | The UI Document that shows the panel. |
| Families | The plant groups offered in the Plant list. Each has a **Name**, the **Strips** that share the group's material, the **Presets** offered for it, and a **Start Preset**: the preset applied when the scene starts. |
| Start Family | Group shown first. |
| Lighting | Optional Demo Day Night that the Night toggle drives. |
| Variants | Shader variant collection with every effect combination, warmed up on start so a build never waits to compile (`Demo/ShowcaseVariants.shadervariants`). |
| Other Scene | Scene the scene button opens. It must be in the build; leave it empty to hide the button. |
| Other Scene Label | Text of that button. |

### Demo Character 2D

**Foliage 2D > Demo > Demo Character 2D**. A small run-and-jump character on a Rigidbody2D, with a
Foliage Pusher 2D so the plants bend around it.

| Setting | What it does |
|---|---|
| Run Speed | Running speed in units per second. |
| Jump Speed | Upward speed of a jump in units per second. |
| Acceleration | How quickly the character reaches its running speed. |
| Ground | Layers the character can stand on. |

### Demo Impulse On Land

**Foliage 2D > Demo > Demo Impulse On Land**. Calls `FoliageGlobals.Impulse` when the character
lands hard ([Interaction](interaction.md#impulses)).

| Setting | What it does |
|---|---|
| Threshold | Falling speed (units per second) from which a landing thumps. |
| Radius | Radius of the thump in world units. |
| Max Strength | Strength of the hardest thump. |

### Demo Day Night

**Foliage 2D > Demo > Demo Day Night**.

| Setting | What it does |
|---|---|
| Global Light | The scene's global 2D light. |
| Lamps | Lamps that shine brighter at night. |
| Sky | Camera whose background turns to the night sky (optional). |
| Night Intensity | Global light intensity at night. |
| Night Color | Global light colour at night. |
| Night Lamp Boost | Lamp intensity multiplier at night. |
| Night Sky | Sky colour at night. |

### Demo Camera Follow

**Foliage 2D > Demo > Demo Camera Follow**.

| Setting | What it does |
|---|---|
| Target | What the camera follows. |
| Smoothing | How quickly the camera catches up (higher is tighter). |
| Limits | Leftmost and rightmost X the camera centre may reach. |

## Building the demos

1. Add `Demo/Scenes/Showcase` and `Demo/Scenes/PixelArt` to **File > Build Profiles** (Build
   Settings).
2. Build as usual. The input works with both the Input System package and the old Input Manager.
3. The first frame takes a moment longer while the panel warms up its shader variants.

You don't need any of this for your own game: the demo folder can be deleted.
