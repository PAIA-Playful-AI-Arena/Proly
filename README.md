# Proly

[![Unity](https://img.shields.io/badge/Unity-%23000000.svg?logo=unity&logoColor=white)](#) [![MLGame3D](https://img.shields.io/pypi/v/mlgame3d?label=MLGame3D
)](https://pypi.org/project/mlgame3d/)

Proly is a Unity-based multi-mode game that supports various gameplay styles including racing and battle modes, featuring item collection and combat mechanics. The game offers two distinct modes:

- **Racing Mode**: Traditional checkpoint-based racing with time-based objectives
- **Battle Mode**: Survival-based combat without checkpoints, focusing on player elimination

It integrates with the MLGame3D framework, allowing AI agents to interact with the game and learn various strategies and skills across different game modes.

## Downloads

[![Windows](https://custom-icon-badges.demolab.com/badge/Windows-1.2.2-blue?logo=windows)](https://github.com/PAIA-Playful-AI-Arena/Proly/releases/download/1.2.2/Proly-win32-1.2.2.zip)
[![macOS](https://img.shields.io/badge/macOS-1.2.2-red?logo=apple)](https://github.com/PAIA-Playful-AI-Arena/Proly/releases/download/1.2.2/Proly-darwin-universal-1.2.2.zip)
[![Linux](https://img.shields.io/badge/Linux-1.2.2-green?logo=linux)](https://github.com/PAIA-Playful-AI-Arena/Proly/releases/download/1.2.2/Proly-linux-1.2.2.zip)

## How to Play

### Game Controls

#### Multiplayer Controls

Proly supports up to 4 players simultaneously with the following control schemes:

- **Player 1**:
  - **Movement**: WASD keys
  - **Select**: Q key
  - **Use**: E key

- **Player 2**:
  - **Movement**: Arrow keys
  - **Select**: Right Shift key
  - **Use**: Right Ctrl or Right Alt key

- **Player 3**:
  - **Movement**: YGHJ keys (Y=up, G=left, H=down, J=right)
  - **Select**: T key
  - **Use**: U key

- **Player 4**:
  - **Movement**: P;L' keys (P=up, L=left, ;=down, '=right)
  - **Select**: O key
  - **Use**: [ key

#### Special Function Keys

- **F key**: Toggle FPS display on/off
- **Space key**: Reset camera position to follow character

### Game Rules

#### Racing Mode (Default)

- **Main Objectives**:
  - Complete the race in the shortest time by passing through all checkpoints
  - The game has a default maximum time limit, but will end 10 seconds after any player reaches the final checkpoint

- **Items**:
  - Each player can hold up to 3 items simultaneously
  - Items can be obtained through pickup
  - Items have different effects and usage methods
  - When your inventory is full, you'll pass through new items without picking them up

- **Penalty Mechanisms**:
  - Being hit by attacks: Lose HP or trigger abnormal status
  - Interactive scenes: Negative effects triggered by active or passive mechanisms
  - Death penalty: Dead players respawn at the last checkpoint passed or the original spawn point, unable to move during respawn period

#### Battle Mode

- **Main Objectives**:
  - Survive as long as possible while eliminating other players
  - Last player standing wins

- **Key Differences from Racing Mode**:
  - No checkpoints - pure survival-based gameplay
  - Players have double the health compared to racing mode
  - Rankings are determined by survival time and elimination order
  - Game ends when only one player remains or time runs out

- **Survival Mechanics**:
  - Players are eliminated when their health reaches zero
  - Survival time is continuously tracked for all players
  - Final rankings consider both elimination order and remaining health

## Integration with MLGame3D

Proly can be integrated with the MLGame3D framework, allowing AI agents to interact with the game.

### Setup Steps

1. Install MLGame3D:
   ```bash
   pip install mlgame3d
   ```

2. Connect to Proly game using MLGame3D:
   ```bash
   python -m mlgame3d path/to/proly.exe
   ```

3. Use a custom MLPlay class:
   ```bash
   python -m mlgame3d -i examples/proly_mlplay.py path/to/proly.exe
   ```

### Game Parameters

Proly supports various game parameters that can be set using the `-gp` option in MLGame3D:

- `mode`: Sets the game mode
  - Values: `racing` (default), `battle`
  - `racing`: Traditional checkpoint-based racing mode
  - `battle`: Survival-based combat mode without checkpoints
  - Example: `-gp mode battle`

- `map`: Sets the map scene to load by build index
  - Values: `0` (Island, default), `1` (S-shaped Island), `2` (V-shaped Island), `3` (Pothole Island)
  - Example: `-gp map 2` (loads the V-shaped Island)

- `checkpoint`: Sets the number of checkpoints to generate
  - Values: Any positive integer
  - Note: When this parameter is set, checkpoint mode is automatically set to "random"
  - Example: `-gp checkpoint 10`

- `items`: Filters which items can appear in the game
  - Values: List of item IDs (empty list means all items are available)
  - Example: `-gp items 1,2,3`

- `max_time`: Sets the maximum time limit for each game in seconds
  - Values: Any positive number (default is 180 seconds / 3 minutes)
  - Example: `-gp max_time 300` (sets time limit to 5 minutes)

- `audio`: Enables or disables game audio
  - Values: `true`/`false` or `1`/`0` (default is `true`)
  - Example: `-gp audio false` (mutes all game audio)
- `mud_pit`: Sets the number of randomly generated mud pits
  - Values: `0` to `3`
  - When this parameter is not set: Uses predefined mud pits in the scene
  - `0`: Removes all mud pits from the scene
  - `1` to `3`: Generates the specified number of mud pits at random positions
  - Example: `-gp mud_pit 2` (generates 2 random mud pits)
  - Example: `-gp mud_pit 0` (removes all mud pits)


Example of using multiple game parameters:
```bash
# Racing mode with custom settings
python -m mlgame3d -i examples/simple_mlplay.py -gp mode racing -gp checkpoint 10 -gp map 1 -gp items 1,2,3 -gp max_time 240 -gp mud_pit 2 -gp audio false path/to/proly.exe

# Battle mode example
python -m mlgame3d -i examples/simple_mlplay.py -gp mode battle -gp map 0 -gp items 3,4,5 -gp max_time 300 -gp mud_pit 1 path/to/proly.exe
```

### Item ID Reference

Proly uses a unified item ID system:

- `1`: HealCake - Restores health
- `2`: SpeedCoffee - Provides a speed boost
- `3`: Bomb - Causes area damage
- `4`: Shuriken - Fast projectile with damage
- `5`: Shield - Provides temporary invulnerability

When using the `available_items` game parameter, you can specify which items should be available in the game:

```bash
# Allow only HealCake and SpeedCoffee
python -m mlgame3d -gp items 1,2 path/to/proly.exe

# Allow only offensive items (Bomb and Shuriken)
python -m mlgame3d -gp items 3,4 path/to/proly.exe

# Allow all items (default behavior)
python -m mlgame3d -gp items "" path/to/proly.exe

# Disable item generation completely (use non-existent ID)
python -m mlgame3d -gp items 0 path/to/proly.exe
```

### Observation Space

Proly provides a rich observation space, as defined in the observation_structure.json file. The key observations include:

- `target_position`: Position of the next checkpoint (Vector3)
  - In Racing Mode: Points to the next checkpoint to reach
  - In Battle Mode: Set to (0, 0, 0) as there are no checkpoints
- `agent_position`: Current position of the agent (Vector3)
- `agent_forward_direction`: Current forward direction of the agent (Vector2, x and z)
- `agent_velocity`: Current velocity of the agent (Vector2, x and z)
- `agent_health`: Current health of the agent (float)
- `agent_health_normalized`: Current health normalized to [0, 1] (float)
- `is_respawning`: Whether the player is currently respawning (bool)
- `last_checkpoint_index`: Index of the last checkpoint passed (int)
- `reached_final_checkpoint`: Whether the player has reached the final checkpoint (bool)
- `current_time`: Current time in the episode (float)
- `current_time_normalized`: Current time normalized to [0, 1] based on maximum time (float)
- `inventory_item_count`: Number of items in the agent's inventory (int)
- `inventory_items`: List of items in the agent's inventory (List of up to 3 items)
  - Each item contains: `item_id` (int)
- `selected_item_index`: Index of the currently selected item, -1 if none (int)
- `selected_item_id`: ID of the currently selected item (0 if no item is selected)
- `nearby_items`: Information about nearby items (List of up to 5 items, sorted by distance from nearest to farthest)
  - Each item contains: `relative_position` (Vector2) and `item_id` (int)
- `nearby_map_objects`: Information about nearby map objects (List of up to 5 objects, sorted by distance from nearest to farthest)
  - Each object contains: `relative_position` (Vector2) and `object_type` (int)
  - Object types: 1: MudPit, 2: Bomb, 3: Shuriken
- `other_players`: Information about other players (List of up to 3 players)
  - The list contains information about other players in order, skipping the current player
  - For example, if current player is Player 2, the list will contain Player 1, Player 3, Player 4 in that order
  - Each player contains:
    - `relative_position` (Vector3): Relative position to other player
    - `relative_velocity` (Vector3): Relative velocity to other player
    - `health` (float): Current health of the other player
    - `health_normalized` (float): Current health of the other player normalized to [0, 1]
    - `inventory_item_count` (int): Number of items in the other player's inventory
    - `inventory_items` (List of up to 3 items): Items in the other player's inventory, each containing `item_id` (int)
    - `selected_item_index` (int): Index of the currently selected item, -1 if none
    - `selected_item_id` (int): ID of the currently selected item (0 if no item is selected)
  - When the player is hidden, default values are used: relative_position (0, -10, 0), relative_velocity (0, 0, 0), health (0), etc.
- `terrain_grid`: 5x5 grid of terrain features around the agent (Grid)
  - Each cell contains: `relative_position` (Vector2) and `terrain_type` (int)
  - Terrain types: 0: Normal, -1: Water, 1: Obstacle
- `unpassed_checkpoints`: Information about unpassed checkpoints (List of fixed 10 items)
  - Each checkpoint contains: `checkpoint_index` (int) and `checkpoint_position` (Vector3)
  - In Racing Mode: Shows the next 10 checkpoints that the player hasn't passed yet
  - In Battle Mode: All slots are filled with default values: checkpoint_index = -1, checkpoint_position = (0, 0, 0)
  - If fewer than 10 unpassed checkpoints remain, remaining slots are filled with default values

### Action Space

Proly supports the following actions:
- **Continuous actions**: Movement (x, z)
  - Values range from -1.0 to 1.0 for each axis
  - Represents normalized movement direction
  - Controls acceleration rather than direct velocity:
    - When input is applied, the agent accelerates in the specified direction
    - When no input is applied, the agent gradually decelerates
    - Terrain effects (like mud pits) increase friction, reducing acceleration and increasing deceleration
    - Character rotation speed adapts based on movement velocity

- **Discrete actions**:
  - Item selection: 0 (no action), 1 (select next item)
  - Item usage: 0 (no action), 1 (use selected item)
