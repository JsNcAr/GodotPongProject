# GodotPongProject

A single-player clone of Pong, built with the Godot Engine.

You control the left paddle; the computer plays the right one. The ball speeds
up slightly on every bounce, so rallies get harder the longer they last.

![Game Photo](https://a.l3n.co/i/3W9kpb.png)

## Playing it

| Input | Action |
|---|---|
| <kbd>↑</kbd> / <kbd>↓</kbd> | Move your paddle up and down |
| <kbd>←</kbd> / <kbd>→</kbd> | Also moves the paddle (both axes are summed) |

Scores for both sides are shown at the top of the screen. When the ball leaves
the field through either side, a timer fires and serves a new one from the
centre in a random direction.

## Running from source

1. Install [Godot 4.2](https://godotengine.org/download) or newer.
2. Clone this repository.
3. Open Godot, choose **Import**, and select the `project.godot` file in the
   repository root.
4. Press <kbd>F5</kbd>, or the play button, to run. The main scene is
   `game.tscn`.

The project targets the **GL Compatibility** renderer at 1280×720 and 60 FPS,
so it runs on older hardware and integrated graphics without a Vulkan driver.

## How it's put together

Everything lives in a single scene, `game.tscn`, whose root script keeps the two
scores and updates the on-screen labels.

| File | Role |
|---|---|
| `game.tscn` | Main scene: walls, goal areas, score labels, restart timer, and the three actors below. |
| `ball.gd` | The ball. Moves with `move_and_collide`, reflects off whatever it hits using the collision normal, and multiplies its speed by `1.02` on each bounce. Starts at 450 px/s in a random diagonal. |
| `player.gd` | Your paddle. Reads the `ui_*` input axes and moves vertically at 300 px/s, pinned to `x = 40`. |
| `rival.gd` | The opponent. Tracks the ball's `y` position and moves toward it at 250 px/s, pinned to `x = 1240`. A dead zone keeps it from jittering when it is roughly aligned. |
| `RotatingBallSprite.gd` | Spins the ball sprite at π rad/s while the ball is in play — cosmetic only. |

The AI is deliberately simple: it always knows exactly where the ball is and
chases it, but it is capped at 250 px/s against the ball's 450 px/s starting
speed, so a sharp angle will beat it.

### Goal detection

`LeftLimit` and `RighLimit` are `Area2D` nodes rather than solid walls. The ball
passes straight through them, which awards the point and starts `RestartTimer`;
`UpperWall` and `LoweWall` are `StaticBody2D` nodes, so the ball bounces off
those instead.

## Assets

- **Font:** Poetsen One (`Fonts/`)
- **Audio:** `Audios/audio-bounce.ogg`, played when the ball strikes a paddle
  (not when it hits a wall)

## Credits

Built as a learning project following a [Platzi](https://platzi.com) course on
Godot — the internal project name is still `platziPong`.
