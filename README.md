# Pyasteroids

A small Asteroids-style prototype built with **Python and Pygame**. Pilot a triangular ship through a field of moving asteroids, fire projectiles, and avoid collisions.

The project focuses on object-oriented game entities, sprite groups, vector movement, collision detection, and a frame-timed update/render loop. It is a learning project with a deliberately small feature set.

## Implemented features

- A 1280 × 720 window with simple white-on-black geometric rendering.
- Ship rotation and forward/backward movement using `pygame.Vector2`.
- Projectiles with a 0.3-second firing cooldown.
- Asteroids spawned from random screen edges, with randomized velocities and three radius sizes.
- Circle-based player–asteroid collision detection that ends the game.
- Separate sprite groups for updating, drawing, asteroids, and shots.
- JSON Lines logging for sampled game state and player-hit events.
- A loop capped at 60 FPS, with movement scaled by elapsed frame time.

**Current gameplay:** shots are created and drawn, but the main loop does not yet check shot–asteroid collisions. Asteroid destruction, splitting, scoring, and restart behavior are not implemented.

## Quick start

### Requirements

- Python **3.13**, matching `.python-version` (`pyproject.toml` requires Python 3.13 or newer).
- Pygame **2.6.1**.
- A graphical desktop session with keyboard input.
- `uv`, or `pip` with a virtual environment.

Run from the repository root, where `main.py` and `pyproject.toml` are located.

With `uv`:

```bash
uv sync --frozen
uv run python main.py
```

Alternatively, on macOS or Linux:

```bash
python3.13 -m venv .venv
source .venv/bin/activate
python -m pip install pygame==2.6.1
python main.py
```

No API key, external game assets, or database is required. When the ship hits an asteroid, the program prints `Game over!` and exits. Launch it again to start a new game.

## Controls

| Key | Action |
| --- | --- |
| **W** | Move forward. |
| **S** | Move backward. |
| **A** | Rotate left. |
| **D** | Rotate right. |
| **Space** | Fire; holding the key fires repeatedly as the cooldown allows. |
| **Window close button** | Exit. |

## Implementation

`CircleShape` provides shared position, velocity, radius, and collision behavior. `Player`, `Asteroid`, and `Shot` specialize drawing and updates. `AsteroidField` periodically creates asteroids, while `main.py` coordinates input events, updates, player collisions, and rendering.

| File | Responsibility |
| --- | --- |
| `main.py` | Window, sprite groups, game loop, rendering, and game-over condition. |
| `circleshape.py` | Shared sprite base and circle-overlap detection. |
| `player.py` | Ship geometry, movement, rotation, and firing cooldown. |
| `shot.py` | Projectile drawing and movement. |
| `asteroid.py` | Asteroid drawing and movement. |
| `asteroidfield.py` | Randomized edge spawning. |
| `constants.py` | Screen dimensions, sizes, speeds, and spawn/firing intervals. |
| `logger.py` | Sampled state snapshots and event logging. |

Adjust gameplay settings in `constants.py`; there is no separate settings UI or configuration file.

## Logs

The application writes logs to its current working directory:

| File | Contents |
| --- | --- |
| `game_state.jsonl` | Sampled sprite-group counts and object state, including positions, velocities, radii, and player rotation where available. |
| `game_events.jsonl` | Events such as `player_hit`. |

State logging samples approximately every 60 loop iterations during the initial roughly 16-second window, with at most 10 sampled sprites per group. Each log file is overwritten on its first write in a new process and then appended to. Both filenames are excluded by `.gitignore`.

## Manual checks

There is no automated test suite in the current repository. To check the implemented behavior:

1. Launch the game and confirm the ship appears near the center.
2. Use **A/D** to rotate and **W/S** to move in the ship's facing direction.
3. Hold **Space** and confirm projectiles are separated by the firing cooldown.
4. Observe asteroids entering from screen edges.
5. Collide with an asteroid and confirm the game exits with `Game over!`.

## Current limits and possible extensions

- Objects do not wrap around the screen, and off-screen asteroids and shots are not removed from their groups.
- Shots do not damage asteroids; collision handling and asteroid splitting are natural next extensions.
- There is no score, lives system, pause menu, sound, or in-game restart.

