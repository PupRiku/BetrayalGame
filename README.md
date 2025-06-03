# Betrayal at House on the Hill - Unreal Engine 5 Adaptation

This project is a digital adaptation of the popular board game "Betrayal at House on the Hill," being developed in Unreal Engine 5. The primary goal is to create a visually engaging and atmospheric multiplayer experience (though current development is focused on core single-player mechanics).

**Repository:** [https://github.com/PupRiku/BetrayalGame.git](https://github.com/PupRiku/BetrayalGame.git)

## Current Project Status (As of June 2025)

The project has successfully implemented the core mechanics for first-person exploration, dynamic room revealing, and tile placement with player-controlled rotation. The foundation for a data-driven room system is in place.

## Features Implemented:

### 1. Player & Camera System
* **First-Person Navigation:** Player character operates from a first-person perspective.
* **Room-Centered:** The player is always positioned in the center of their current room tile.
* **90-Degree Snap Turns:** Smooth, 90-degree turns using Left/Right Arrow keys, with the character's facing direction (North, East, South, West) tracked via an Enum.
* **Camera Control:** The camera correctly follows the character's snap turns.

### 2. Room System & Data
* **Modular Room Tiles (`BP_RoomTile`):**
    * Each room tile can define whether it has doors on its North, East, South, and West sides using boolean flags.
    * Stores a `RoomName` (String/Text).
    * Can hold references to adjacent, connected rooms for each of its doors.
* **Data-Driven Design:**
    * `EFloorType` Enum (e.g., GroundFloor, UpperFloor, Basement, Any) defines floor types.
    * `FRoomData` Struct defines individual room properties: Room Name, the Blueprint class to spawn for the tile, an array of allowed floors, and default door configurations (N,E,S,W booleans).
    * `DT_RoomTiles` Data Table stores the definitions for all available room tiles, populated with sample rooms.

### 3. Exploration & Room Reveal Mechanics
* **Player State:** The player character tracks its `CurrentRoom` (reference to a `BP_RoomTile` instance) and `CurrentFloor`.
* **Wall Interaction Analysis (`GetFacedWallInteractionInfo` function):**
    * This function is rotation-aware: it correctly determines which *local side* of a (potentially rotated) `CurrentRoom` the player is facing.
    * Identifies the faced wall as `SolidWall`, `DoorToRevealedRoom` (if connected to an existing room), or `DoorToUnrevealedArea`.
    * Provides a reference to the `AdjacentRoom` if it's a `DoorToRevealedRoom`.
* **Player-Controlled Room Placement (for Unrevealed Doors):**
    * When interacting with a `DoorToUnrevealedArea`:
        1.  **Enter Placement Mode:** A boolean state (`bIsInRoomPlacementMode`) is activated.
        2.  **Draw Room:** A `DrawRandomRoomForFloor` function (in the Game Mode) selects a suitable room's data (`PendingRoomData`) from `DT_RoomTiles` based on the player's `CurrentFloor`.
        3.  **Preview Actor:** A temporary "preview" instance of the drawn room tile is spawned at the correct adjacent location (based on `ExitDirectionFromOldRoom` and `RoomSideLength`) with a default rotation. The preview is configured with its name and default doors.
        4.  **Rotate Preview:** The player can use A/D keys to rotate the `PreviewRoomActorRef` in 90-degree increments.
        5.  **Confirm Placement:** Pressing "E" again triggers confirmation.
            * **Validity Check:** The system checks if the currently aligned local door of the preview room (based on its rotation) has a corresponding default door in its `PendingRoomData`.
            * **Invalid Placement:** If no default door aligns, an error message is shown, and the player remains in placement mode to continue rotating/trying.
            * **Valid Placement:**
                * The `bIsInRoomPlacementMode` state is exited.
                * A permanent room actor is spawned using the preview's final location and rotation.
                * The preview actor is destroyed.
                * **Rooms Connected:** Logical two-way connections (`ConnectedRoom_` variables) are established between the old room and the new permanent room, using the correctly rotated side of the new room. The connecting door boolean on the new room is ensured to be true.
                * **Player Moves:** The player character is moved to the center of the new permanent room, facing an appropriate direction.
* **Movement Between Revealed Rooms:** If facing a `DoorToRevealedRoom`, pressing "E" correctly moves the player to the center of that adjacent room.

### 4. User Interface (UI)
* **Dynamic Interaction Prompts (`WBP_InteractionPrompt`):**
    * A UMG widget displays contextual prompts to the player.
    * Examples: "Press E to enter [RoomName]", "Press E to reveal new room".
    * If facing a solid wall, no prompt is shown.
    * During room placement mode, the prompt changes to "A/D to rotate. Press E to Place Room."

## Controls
* **Left/Right Arrow Keys:** Turn player character 90 degrees left/right.
* **A/D Keys:** While in room placement mode, rotate the preview room tile counter-clockwise/clockwise.
* **E Key:**
    * Interact with faced door (enter revealed room or initiate room placement for unrevealed door).
    * Confirm placement of a rotated preview room.

## Engine Version
* Unreal Engine 5 (please specify your exact version, e.g., 5.X.X, if you wish)

## How To Test (Current State)
1.  Ensure a custom Game Mode (e.g., `BP_BetrayalGameMode` containing `DrawRandomRoomForFloor`) is set as the default or world override.
2.  Ensure the Game Mode's `Default Pawn Class` is set to `BP_ThirdPersonCharacter`.
3.  Place a `Player Start` actor in the level.
4.  Place an initial `BP_RoomTile` instance in the level.
    * Configure its `RoomName` and door booleans (`bHasNorthDoor`, etc.).
    * Ensure at least one door is set to `True` but its corresponding `ConnectedRoom_[Direction]` is `None` to test room reveal.
5.  Populate `DT_RoomTiles` with a few sample rooms, ensuring some are valid for the player's starting floor and have varied door configurations.
    * `FRoomData` should specify `BP_RoomTile` as the `RoomTileClass` for now.
6.  Player character requires `RoomSideLength` variable to be set (e.g., 500.0).

## Next Steps / Future Goals
* **Polish Room Placement:**
    * Add live visual feedback during preview rotation (e.g., highlight valid/invalid door alignments).
    * Potentially disable confirmation if current preview rotation is invalid.
* **Robust Room Deck Management:** Prevent repeats, handle running out of rooms.
* **Card Systems:** Implement Omen, Event, and Item card drawing and effects.
* **Player Stats & Dice Mechanics.**
* **The Haunt Mechanic.**
* **Multiplayer Functionality.**

---

This project is a work in progress. Contributions, suggestions, and feedback are welcome (if applicable).