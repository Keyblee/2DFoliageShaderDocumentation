# Sorting and Depth

Each strip is one renderer with its own **Sorting Layer** and **Sorting Order**, set on the strip
component. Strips sort against sprites, tilemaps and each other exactly like sprites do.

## Walking inside the grass

A single strip is either in front of the player or behind it. To make the player walk *through*
grass, use two strips:

1. Choose **GameObject > 2D Object > Foliage > Grass (Front + Back)**. It creates a parent with two
   grass strips:
   - **Grass Back**: full height, Sorting Order -1, drawn behind the player;
   - **Grass Front**: 60 % as tall, Sorting Order 1, drawn in front of the player, with a different
     seed so the two layers don't line up.
2. Give the player a Sorting Order between them (0) on the same Sorting Layer.
3. Put a **Foliage Pusher 2D** on the player so both strips bend away from it
   ([Interaction](interaction.md)).

The front strip is shorter so the character stays readable inside the grass. Lower it further, or
lower its Density, for a small character.

The same idea works with any family: a bush strip behind the player and a low ground-cover strip in
front of it, for example.

## Layering a scene

- **Background**: a strip on a background Sorting Layer, darker colours (or a darker preset) and a
  lower Wind Response read as distance. Turn **Ignore 2D Lights** on if the background should not
  react to your lights.
- **Gameplay layer**: back and front strips around the player, as above.
- **Foreground**: a tall, sparse strip in front of everything (high Sorting Order), for example tall
  reeds at the bottom of the screen.

## Vines and ledges

A vine strip hangs from its position. Put its Sorting Order below the ledge tiles so the vines seem
to grow from behind the edge, or above them to drape over it.
