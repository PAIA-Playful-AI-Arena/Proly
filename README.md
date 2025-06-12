# Proly

[![Unity](https://img.shields.io/badge/Unity-%23000000.svg?logo=unity&logoColor=white)](#) [![MLGame3D](https://img.shields.io/pypi/v/mlgame3d?label=MLGame3D
)](https://pypi.org/project/mlgame3d/)

Proly is a Unity-based multi-mode game that supports various gameplay styles including racing, item collection, and combat. It integrates with the MLGame3D framework, allowing AI agents to interact with the game and learn various strategies and skills.

## Downloads

[![Windows](https://custom-icon-badges.demolab.com/badge/Windows-0.6.0-blue?logo=windows)](https://github.com/PAIA-Playful-AI-Arena/Proly/releases/download/0.6.0/Proly-win32-0.6.0.zip)
[![macOS](https://img.shields.io/badge/macOS-0.6.0-red?logo=apple)](https://github.com/PAIA-Playful-AI-Arena/Proly/releases/download/0.6.0/Proly-darwin-universal-0.6.0.zip)
[![Linux](https://img.shields.io/badge/Linux-0.6.0-green?logo=linux)](https://github.com/PAIA-Playful-AI-Arena/Proly/releases/download/0.6.0/Proly-linux-0.6.0.zip)

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

- **Main Objectives**:
  - Complete the race in the shortest time

- **Items**:
  - Each player can hold up to 3 items simultaneously
  - Items can be obtained through pickup
  - Items have different effects and usage methods
  - When your inventory is full, you'll pass through new items without picking them up

- **Penalty Mechanisms**:
  - Being hit by attacks: Lose HP or trigger abnormal status
  - Interactive scenes: Negative effects triggered by active or passive mechanisms
  - Time penalty: 10-second countdown after the first player completes the course

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

- `map`: Sets the map scene to load by build index
  - Values: `0` (Island, default), `1` (Atoll)
  - Example: `-gp map 1` (loads the Atoll map)

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
python -m mlgame3d -i examples/simple_mlplay.py -gp checkpoint 10 -gp map 1 -gp items 1,2,3 -gp max_time 240 -gp mud_pit 2 -gp audio false path/to/proly.exe
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
- `agent_position`: Current position of the agent (Vector3)
- `agent_forward_direction`: Current forward direction of the agent (Vector2, x and z)
- `agent_velocity`: Current velocity of the agent (Vector2, x and z)
- `agent_health`: Current health of the agent (float)
- `agent_health_normalized`: Current health normalized to [0, 1] (float)
- `last_checkpoint_index`: Index of the last checkpoint passed (int)
- `current_time`: Current time in the episode (float)
- `current_time_normalized`: Current time normalized to [0, 1] based on maximum time (float)
- `inventory_item_count`: Number of items in the agent's inventory (int)
- `inventory_items`: List of items in the agent's inventory (List of up to 3 items)
  - Each item contains: `item_id` (int)
- `selected_item_index`: Index of the currently selected item, -1 if none (int)
- `nearby_items`: Information about nearby items (List of up to 5 items)
  - Each item contains: `relative_position` (Vector2) and `item_id` (int)
- `nearby_map_objects`: Information about nearby map objects (List of up to 5 objects)
  - Each object contains: `relative_position` (Vector2) and `object_type` (int)
  - Object types: 1: MudPit, 2: Bomb, 3: Shuriken
- `other_players`: Information about other players (List of up to 3 players)
  - Each player contains: `relative_position` (Vector3) and `relative_velocity` (Vector3)
  - When the player is hidden, default values are: relative_position (0, -10, 0) and relative_velocity (0, 0, 0)
- `terrain_grid`: 5x5 grid of terrain features around the agent (Grid)
  - Each cell contains: `relative_position` (Vector2) and `terrain_type` (int)
  - Terrain types: 0: Normal, -1: Water, 1: Obstacle
- `unpassed_checkpoints`: Information about unpassed checkpoints (List of fixed 10 items)
  - Each checkpoint contains: `checkpoint_index` (int) and `checkpoint_position` (Vector3)
  - Shows the next 10 checkpoints that the player hasn't passed yet
  - If fewer than 10 unpassed checkpoints remain, remaining slots are filled with default values: checkpoint_index = -1, checkpoint_position = (0, 0, 0)

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
