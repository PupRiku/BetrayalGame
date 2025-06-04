# Betrayal at House on the Hill - Unreal Engine 5 Adaptation

This project is a digital adaptation of the popular board game "Betrayal at House on the Hill," being developed in Unreal Engine 5. The primary goal is to create a visually engaging and atmospheric multiplayer experience (though current development is focused on core single-player mechanics).

**Repository:** [https://github.com/PupRiku/BetrayalGame.git](https://github.com/PupRiku/BetrayalGame.git)

## Current Project Status (As of June 2025)

The project has successfully implemented a robust system for dynamic, data-driven room exploration. This includes drawing unique rooms from floor-specific decks, allowing player-controlled rotation for placement, validating connections based on default door layouts, and establishing correct two-way navigation between rooms. The core gameplay loop for building out the house is now highly functional. All official room data has been entered into the Data Tables, providing a full set of tiles for testing.

## Features Implemented:

### 1. Player & Camera System

- **First-Person Navigation:** Player character operates from a first-person perspective.
- **Room-Centered:** The player is always positioned in the center of their current room tile.
- **90-Degree Snap Turns:** Smooth, 90-degree turns using Left/Right Arrow keys, with the character's facing direction (North, East, South, West) tracked via an Enum.
- **Camera Control:** The camera correctly follows the character's snap turns and maintains orientation.

### 2. Room System & Data (`BP_RoomTile`)

- **Modular Room Tiles:**
  - Each room tile can define whether it has doors on its local North, East, South, and West sides using boolean flags (`bHas[Direction]Door`).
  - Stores a `RoomName` (Text/String).
  - Can hold references to adjacent, connected, revealed rooms (`ConnectedRoom_[Direction]`).
- **Data-Driven Design:**
  - `EFloorType` Enum (e.g., GroundFloor, UpperFloor, Basement, Any) defines floor types.
  - `FRoomData` Struct defines individual room properties:
    - `RoomName` (Text/String)
    - `RoomTileClass` (`TSubclassOf BP_RoomTile`)
    - `AllowedFloors` (Array of `EFloorType`)
    - `bHasNorthDoor_Default`, `bHasEastDoor_Default`, `bHasSouthDoor_Default`, `bHasWestDoor_Default` (Booleans for default door configuration when drawn).
    - `IconType` (String - placeholder for Omen, Event, Item icons).
    - `bHasUniqueRule` (Boolean - flag for rooms with special rules).
  - `DT_RoomTiles` Data Table: Stores definitions for all official room tiles, fully populated.

### 3. Room Deck Management (`BP_BetrayalGameMode`)

- **Initialized Floor Decks:** At game start, separate, shuffled decks of available room tile FNames are created for each floor (Ground, Upper, Basement) from `DT_RoomTiles`.
- **Starting Room Exclusion:** Pre-defined starting rooms (e.g., Foyer) are correctly excluded from these draw decks.
- **Unique Room Drawing (`DrawRandomRoomForFloor` function):**
  - Draws uniquely from the top of the appropriate floor's deck.
  - Drawn rooms are removed from the primary deck.
  - Globally unique rooms (if allowed on multiple floors and present in multiple initial decks) are removed from _all_ other decks once drawn from one.
  - The system correctly handles and reports when a floor-specific deck is empty.

### 4. Exploration & Player-Controlled Room Placement

- **Player State Tracking:** `BP_ThirdPersonCharacter` tracks `CurrentRoom`, `CurrentFloor`, and `CurrentFacingDirection`.
- **Wall Interaction Analysis (`GetFacedWallInteractionInfo` function):**
  - This function is fully rotation-aware: it correctly determines which _local side_ of a (potentially world-rotated) `CurrentRoom` the player is facing.
  - Identifies the faced wall as `SolidWall`, `DoorToRevealedRoom`, or `DoorToUnrevealedArea`.
  - Provides a reference to the `AdjacentRoom` if it's a `DoorToRevealedRoom`.
- **Movement Between Revealed Rooms:** Pressing "E" on a `DoorToRevealedRoom` successfully moves the player to the center of the adjacent connected room, with appropriate orientation.
- **Room Reveal Sequence (for `DoorToUnrevealedArea`):**
  1.  **Enter Placement Mode:** `bIsInRoomPlacementMode` state is activated. `PendingRoomData` (for the drawn room) and `ExitDirectionFromOldRoom` are stored.
  2.  **Preview Actor:** A temporary `PreviewRoomActorRef` is spawned at the correct adjacent location and configured with its name and default doors from `PendingRoomData`.
  3.  **Player-Controlled Rotation:** A/D keys rotate the `PreviewRoomActorRef` in 90-degree increments.
  4.  **Live Validity Feedback:** The UI prompt dynamically updates (e.g., "VALID" or "INVALID - No matching door!") as the player rotates the preview, based on whether a default door on the preview aligns with the exit. (Visual feedback on the preview actor material is also implemented).
  5.  **Confirm Placement (Second "E" press):**
      - **Validity Check:** The system re-confirms if the currently aligned local door of the preview room has a corresponding default door in its `PendingRoomData`.
      - **Invalid Placement:** An error message is shown, and the player remains in placement mode with the preview active to continue rotating/trying.
      - **Valid Placement:**
        - `bIsInRoomPlacementMode` is exited.
        - A permanent room is spawned with the preview's final location and rotation.
        - The preview actor is destroyed.
        - **Rotation-Aware Connections:** Logical two-way connections (`ConnectedRoom_` variables) are established, correctly using the rotated side of the new permanent room.
        - Player is moved into the new permanent room and oriented.

### 5. User Interface (UI)

- **Dynamic Interaction Prompts (`WBP_InteractionPrompt`):**
  - Displays context-sensitive prompts: "Press E to enter [RoomName]", "Press E to reveal new room", or no prompt for solid walls.
  - During room placement: "A/D to Rotate. Press E to Place Room ([VALID]/[INVALID - No matching door!])".

## Controls

- **Left/Right Arrow Keys:** Turn player character 90 degrees left/right.
- **A/D Keys:** While in room placement mode, rotate the preview room tile.
- **E Key:**
  - Interact with faced door (enter revealed room or initiate room placement for unrevealed door).
  - Confirm placement of a rotated preview room.

## Engine Version

- Unreal Engine 5.5.4

## How To Test (Current State)

1.  Ensure custom Game Mode (`BP_BetrayalGameMode`) is set.
2.  Ensure Game Mode's `Default Pawn Class` is `BP_ThirdPersonCharacter`.
3.  Place a `Player Start`.
4.  Place initial `BP_RoomTile` instances for the starting layout (e.g., Foyer, Grand Staircase, Upper Landing - these names should be in `StartingRoomExclusionList` in `BP_BetrayalGameMode`). Ensure these are connected appropriately.
5.  Ensure `DT_RoomTiles` is populated. `FRoomData` should specify `BP_RoomTile` as the `RoomTileClass`.
6.  Player character requires `RoomSideLength` variable to be set (e.g., 500.0).
7.  Player character's `CurrentFloor` should be set to the appropriate starting floor.

## Next Steps / Future Goals

- **Card Systems:** Implement Omen, Event, and Item card drawing and effects (Next Up!).
- **Player Stats & Dice Mechanics.**
- **The Haunt Mechanic.**
- **Implement Unique Room Rules** (using `bHasUniqueRule` flag).
- **Prevent Room Overlaps** during placement (spatial grid check).
- **Top-Down Map Display** for player reference.
- **Multiplayer Functionality.**

---

This project is a work in progress.
