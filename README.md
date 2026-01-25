# sdl2-c-game-template

A minimal SDL2 game development template written in C.

Provides a clean project structure, basic game loop, and asset handling setup—no engine, no frameworks, just SDL2 and C.

Designed to be simple, hackable, and easy to extend for small-to-medium game projects.

# Wiki

## Architecture Overview

The project is structured around a simple and explicit game lifecycle.
All game logic lives in game.c, while main.c only handles initialization, the main loop, and shutdown.

> You should not need to modify `main.c` unless you are changing debug behavior defined in `config.h`

## Entry point (`main.c`)
- main.c is intentionally minimal:
- Creates the SDL window and renderer
- Runs the main loop
- Calls game lifecycle functions
- Handles shutdown

All gameplay code belongs in `game.c`

## Core Structures

**`GameContext`**

Holds the runtime context of the game.
Contains:
- SDL window and renderer
- Timing and frame data
- Input state
- Pointers to shared systems or resources
- `GameData`

This struct is passed to all game lifecycle functions.

**`GameData`**
Stores game-specific state and data.
Examples:
- Player and entity state
- Level data
- Gameplay variables

Keeps game logic separate from platform and SDL boilerplate.

## Game Lifecycle (`game.h`)

The game follows a clear, predictable flow:
```c
bool game_init(GameContext*);
void game_handle_events(GameContext*);
void game_update(GameContext*);
void game_render(GameContext*);
void game_cleanup(GameContext*);
```

Lifecycle breakdown:

- `game_init`
Initializes game state, assets, and systems.
Returns false if initialization fails.

- `game_handle_events`
Handles input and SDL events.

- `game_update`
Updates game logic at a fixed rate.

- `game_render`
Draws the current game state to the screen.

- `game_cleanup`
Frees resources and performs shutdown cleanup.

## Configuration (`config.h`)
Build-time configuration values are defined in config.h:
```c
#define TARGET_FPS 60

#define WINDOW_WIDTH 1280
#define WINDOW_HEIGHT 960

#define VIRTUAL_WIDTH 320
#define VIRTUAL_HEIGHT 240

#define DEBUG 1
```

Notes
- The window size is a 4× scale of the virtual resolution.
- Virtual resolution allows consistent rendering across displays.
- DEBUG enables debug-specific behavior without touching main.c.

> Future improvements may move these values to a runtime config file.