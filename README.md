# Betrayal at House on the Hill - Unreal Engine 5 Adaptation

This project is a digital adaptation of the popular board game "Betrayal at House on the Hill," being developed in Unreal Engine 5. The primary goal is to create a visually engaging and atmospheric multiplayer experience (though current development is focused on core single-player mechanics).

**Repository:** [https://github.com/PupRiku/BetrayalGame.git](https://github.com/PupRiku/BetrayalGame.git)

## Current Project Status (As of June 2025)

The project has achieved a highly functional core gameplay loop for exploration and dynamic house layout generation. This includes a robust, data-driven system for managing and drawing unique room tiles from shuffled, floor-specific decks, and a complete player-controlled room placement mechanic with live validity feedback and rotation-aware connections. Furthermore, the foundational data structures and logic for Omen, Event, and Item card systems are in place, including deck initialization, card drawing, and placeholder triggers for card events upon room discovery. All official room data has been entered, providing a comprehensive set of tiles for gameplay.

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
  - Stores an `IconType` (`ERoomIconType` Enum: None, Omen, Event, Item).
  - Stores `bIconAlreadyTriggered` (Boolean) to ensure one-time icon events.
  - Can hold references to adjacent, connected, revealed rooms (`ConnectedRoom_[Direction]`).
- **Data-Driven Design:**
  - `EFloorType` Enum (e.g., GroundFloor, UpperFloor, Basement, Any) defines floor types.
  - `FRoomData` Struct defines individual room properties:
    - `RoomName` (Text/String)
    - `RoomTileClass` (`TSubclassOf BP_RoomTile`)
    - `AllowedFloors` (Array of `EFloorType`)
    - `bHasNorthDoor_Default`, `bHasEastDoor_Default`, `bHasSouthDoor_Default`, `bHasWestDoor_Default` (Booleans for default door configuration).
    - `IconType` (`ERoomIconType` Enum).
    - `bHasUniqueRule` (Boolean - flag for rooms with special rules).
  - `DT_RoomTiles` Data Table: Stores definitions for all official room tiles, fully populated.

### 3. Room Deck Management (`BP_BetrayalGameMode`)

- **Initialized Floor Decks:** At game start, separate, shuffled decks of available room tile FNames are created for each floor (Ground, Upper, Basement) from `DT_RoomTiles`.
- **Starting Room Exclusion:** Pre-defined starting rooms are correctly excluded from these draw decks.
- **Unique Room Drawing (`DrawRandomRoomForFloor` function):**
  - Draws uniquely from the top of the appropriate floor's deck.
  - Drawn rooms are removed from the primary deck.
  - Globally unique rooms (if allowed on multiple floors) are correctly removed from _all_ other decks once drawn from one.
  - The system correctly handles and reports when a floor-specific deck is empty.

### 4. Exploration & Player-Controlled Room Placement

- **Player State Tracking:** `BP_ThirdPersonCharacter` tracks `CurrentRoom`, `CurrentFloor`, and `CurrentFacingDirection`.
- **Wall Interaction Analysis (`GetFacedWallInteractionInfo` function):**
  - Fully rotation-aware: Correctly determines which _local side_ of a world-rotated `CurrentRoom` the player is facing.
  - Identifies the faced wall as `SolidWall`, `DoorToRevealedRoom`, or `DoorToUnrevealedArea`.
- **Movement Between Revealed Rooms:** Pressing "E" on a `DoorToRevealedRoom` successfully moves the player between connected rooms.
- **Room Reveal Sequence (for `DoorToUnrevealedArea`):**
  1.  **Enter Placement Mode:** `bIsInRoomPlacementMode` state activated. `PendingRoomData` (for the drawn room) and `ExitDirectionFromOldRoom` are stored.
  2.  **Preview Actor:** A temporary `PreviewRoomActorRef` is spawned at the correct adjacent location and configured.
  3.  **Player-Controlled Rotation:** A/D keys rotate the `PreviewRoomActorRef`.
  4.  **Live Validity Feedback:** UI prompt dynamically updates to "VALID" or "INVALID - No matching door!" based on whether a default door on the rotated preview aligns with the exit. (Visual feedback on preview actor material also implemented).
  5.  **Confirm Placement (Second "E" press):**
      - **Validity Check:** Re-confirms if the aligned local door of the preview room has a corresponding default door in `PendingRoomData`.
      - **Invalid Placement:** Message shown, player remains in placement mode.
      - **Valid Placement:** Permanent room is spawned with the preview's final transform (location and rotation). Preview is destroyed. Rooms are logically connected (two-way, using the correctly rotated side of the new room). Player is moved into the new permanent room.

### 5. Card System - Initial Foundation

- **Card Data Structures:**
  - `FCard_Omen` Struct (includes `CardName`, `FlavorText`, `RuleText`, `bPlayerKeepsCard`, `bIsWeapon`, `bIsCompanion`) and `DT_OmenCards` Data Table.
  - `FCard_Event` Struct and `DT_EventCards` Data Table.
  - `FCard_Item` Struct (includes `bIsWeapon`) and `DT_ItemCards` Data Table.
  - Sample cards populated in each Data Table.
- **Card Deck Initialization (`BP_BetrayalGameMode`):**
  - Separate deck and discard pile array variables (Array of FName) for Omen, Event, and Item cards.
  - `InitializeCardDecks` function populates and shuffles these decks from their respective Data Tables at game start.
- **Card Drawing Functions (`BP_BetrayalGameMode`):**
  - `DrawOmenCard()`, `DrawEventCard()`, `DrawItemCard()` functions implemented.
  - These handle drawing from the correct deck, moving to discard, removing from the main deck, and managing empty deck scenarios (Omen deck reshuffles its discard; Event/Item decks report failure if empty).
- **Card Draw Triggering (`BP_ThirdPersonCharacter`):**
  - `ProcessRoomEntryEffects` function is called when a player enters a room.
  - Checks the `CurrentRoom`'s `IconType` (Omen, Event, Item, None) and `bIconAlreadyTriggered` status.
  - If an icon is present and not yet triggered, it calls the appropriate `Draw[CardType]Card` function from the Game Mode.
  - `bIconAlreadyTriggered` is set to true on the room instance to prevent re-triggering.
  - Placeholder `Print String` messages currently display the drawn card's details.
  - A "TODO: Make Haunt Roll!" placeholder is triggered after an Omen card is drawn.

### 6. User Interface (UI)

- **Dynamic Interaction Prompts (`WBP_InteractionPrompt`):**
  - Displays context-sensitive prompts for movement, room reveal, and room placement (including rotation and validity status).

## Controls

- **Left/Right Arrow Keys:** Turn player character 90 degrees left/right.
- **A/D Keys:** While in room placement mode, rotate the preview room tile.
- **E Key:**
  - Interact with faced door (enter revealed room or initiate room placement for unrevealed door).
  - Confirm placement of a rotated preview room.

## Engine Version

- Unreal Engine 5.5.4

## How To Test (Current State)

- Ensure custom Game Mode (`BP_BetrayalGameMode`) is set with `BP_ThirdPersonCharacter` as `Default Pawn Class`.
- Level requires a `Player Start` and initial `BP_RoomTile` instances (e.g., Foyer) configured in `StartingRoomExclusionList`.
- `DT_RoomTiles`, `DT_OmenCards`, `DT_EventCards`, `DT_ItemCards` should be populated.
- Player character's `RoomSideLength` and initial `CurrentFloor` should be set.

## Next Steps / Future Goals

- **Implement Player Stats & Dice Rolling Mechanics** (Next Up!).
- **Develop UI for Displaying Drawn Cards.**
- **Implement Specific Card Effects.**
- **The Haunt Mechanic** (Haunt Roll, Traitor Selection, Hidden Information).
- Implement Unique Room Rules (using `bHasUniqueRule` flag).
- Prevent Room Overlaps during placement (spatial grid check).
- Top-Down Map Display for player reference.
- Multiplayer Functionality.

---

This project is a work in progress.
