# Pipeline Setup

The foliage shaders need the **Universal Render Pipeline (URP)** with the **2D Renderer**, the same
setup as URP's 2D lights and lit sprites.

## New project

Create the project from the **Universal 2D** template in the Unity Hub. Everything is set up.

## Existing URP project

1. Create a 2D Renderer: in the Project window, **Create > Rendering > URP 2D Renderer**.
2. Select your URP asset (the one in **Project Settings > Graphics**, and in **Quality** if each level
   has its own) and add the 2D Renderer to its **Renderer List**. Make it the default, or pick it on
   your camera's **Renderer**.
3. Add 2D lights to the scene (a **Global Light 2D** at least) so the plants are lit like your
   sprites. Plants with **Ignore 2D Lights** on show their colours as set.

## Lighting

- The plants take URP 2D lights like a lit sprite: global, spot, freeform and sprite lights all
  work, including their colours.
- Normal maps and shadow casters are not used by the foliage.
- With Pixel Art on, lights brighten or darken the palette colours, like on a pixel-art sprite.

## Pixel Perfect Camera

For pixel art, add URP's **Pixel Perfect Camera** component to your camera. It comes with URP's 2D
support. See [Pixel Art](pixel-art.md).

## Not supported yet

- **URP with the Universal (3D) Renderer** and the **Built-in** render pipeline: planned for later
  updates. The shaders don't draw there yet.
- **Unity 2022.3 LTS**: planned for a later update.
