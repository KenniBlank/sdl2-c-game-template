# sdl2-c-game-template

A minimal SDL2 game development template written in C.

Provides a clean project structure, basic game loop, and asset handling setup—no engine, no frameworks, just SDL2 and C.

Designed to be simple, hackable, and easy to extend for small-to-medium game projects.

## Example Game
- Sand-Tetris: https://github.com/KenniBlank/Sand-Tetris

# Wiki

## Project Structure

```text
./
├── assets/            # Game assets (images, audio, etc.)
├── src/
│   ├── config.h       # Build-time configuration
│   ├── game.c         # All game logic
│   ├── game.h         # Game interface
│   └── main.c         # Entry point and main loop
├── LICENSE
├── Makefile
└── README.md
```

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

# Build System (Makefile)

This Makefile provides a minimal build system for a C project using SDL2. It's designed to be simple and easy to understand.

## Build Targets

### `all` (Default Target)
- Creates the `build/` directory if it doesn't exist
- Compiles the source files into an executable named `game` in the `build/` directory
- **Usage**: `make` or `make all`

### `clean`
- Removes the entire `build/` directory and its contents
- **Usage**: `make clean`

## Dependencies
- **Compiler**: GCC
- **Libraries**: SDL2 (Simple DirectMedia Layer 2)
- Optional (commented out):
- SDL2_image
- SDL2_ttf

## Compilation Flags
- `-Wall`: Enable all standard warnings
- `-std=c11`: Use C11 standard
- SDL2-specific flags from `sdl2-config`
- **Optional**: `-Wextra` (extra warnings - currently commented out)

## Usage Examples

1. **Build the project**:
```bash
make
```

2. **Clean build artifacts**:
```bash
make clean
```

3. **Rebuild from scratch**:
```bash
make clean && make
```

## Customizing the Makefile
Customization Tips

1. Add more source files: Add to the SRC variable:
```makefile
SRC = src/main.c src/game.c src/another.c
```

2. Enable optional libraries: Uncomment the relevant lines for:
- SDL2_image (image loading)
- SDL2_ttf (TrueType font rendering)

3. Change output name: Modify the `OUT` variable:
```makefile
OUT = build/my_game
```

4. Add optimization flags:
```makefile
CFLAGS += -O2
```

# Final Note

- Insure SDL development libraries are installed
- All source code in `src/` folder
- All assets like images, videos, fonts, etc in `assets/` folder
