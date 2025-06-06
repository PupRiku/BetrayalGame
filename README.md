# Betrayal at House on the Hill - Unreal Engine 5 Adaptation

This project is a digital adaptation of the popular board game "Betrayal at House on the Hill," being developed in Unreal Engine 5. The primary goal is to create a visually engaging and atmospheric multiplayer experience (though current development is focused on core single-player mechanics).

**Repository:** [https://github.com/PupRiku/BetrayalGame.git](https://github.com/PupRiku/BetrayalGame.git)

## Current Project Status (As of June 2025)

The project has achieved a complete gameplay loop for the "first act" of the game. This includes a robust, data-driven system for house exploration, dynamic room revealing with player-controlled rotation, and a fully functional card system (Omens, Events, Items) from data structure to UI display to effect execution. The foundational systems for player character stats, custom dice rolling, and the pivotal Haunt Roll mechanic are all in place and have been successfully integrated and tested.

## Features Implemented:

### 1. Player & Camera System

- **First-Person Navigation:** Player character operates from a first-person perspective, centered in the current room.
- **90-Degree Snap Turns:** Smooth, 90-degree turns using Left/Right Arrow keys, with the character's facing direction tracked.
- **Camera Control:** The camera correctly follows the character's snap turns.

### 2. Player Stats & Character System

- **Data-Driven Characters (`DT_Characters`):**
  - A Data Table stores definitions for each unique character.
  - The `FCharacterData` struct holds each character's `CharacterName`, `CharacterPortrait`, `CharacterColor`, starting stat indices, and their **unique stat tracks** (arrays of integers for Might, Speed, Sanity, and Knowledge).
- **Stat Initialization (`BP_ThirdPersonCharacter`):**
  - At game start, the character loads its data from `DT_Characters` to set its starting stat indices and values correctly.
- **Stat Modification:** A `ChangeStat` function cleanly handles stat changes by modifying the character's index on their personal track and updating the corresponding stat value.

### 3. Room System & Deck Management

- **Data-Driven Rooms (`DT_RoomTiles`):** A fully populated Data Table defines all official rooms with their names, floor allowances, default door configurations, and card-drawing `IconType`.
- **Robust Deck Management (`BP_BetrayalGameMode`):**
  - At game start, separate, shuffled decks of available room tiles are created for each floor.
  - Pre-placed starting rooms (Foyer, etc.) are correctly excluded from the draw decks.
  - The system draws unique rooms from the appropriate deck, removing them after use.
  - Globally unique tiles (if allowed on multiple floors) are correctly removed from _all_ decks once drawn from one.
  - The system correctly handles and reports when a floor-specific deck is empty.
- **Player-Controlled Room Placement:**
  - A full placement preview mode with live UI feedback ("VALID" / "INVALID") based on door alignment.
  - Player can rotate the preview tile with A/D keys.
  - Confirmation logic correctly validates placement, spawns the permanent room with the chosen rotation, establishes two-way logical connections based on the rotated orientation, and moves the player.

### 4. Dice & Card Systems

- **Dice Rolling:** A `RollDice` function in the Game Mode accurately simulates rolling any number of custom six-sided Betrayal dice (faces: 0, 0, 1, 1, 2, 2).
- **Card Data Structures:** Separate Structs (`FCard_Omen`, `FCard_Event`, `FCard_Item`) and Data Tables (`DT_...Cards`) define all card types with their text and properties (`bIsWeapon`, `bIsCompanion`, etc.).
- **Card Deck Management:** Omen, Event, and Item decks are initialized, shuffled, and drawn from correctly, handling discard piles and empty-deck rules (Omen reshuffle).
- **Component-Based Card Effects (Scalable Architecture):**
  - A base `BP_CardEffect_Base` actor component defines an `ExecuteEffect` function.
  - Each card in the Data Tables is linked to a specific child component (e.g., `BP_CardEffect_Rotten`) that contains its unique logic.
  - This decouples the player character from knowing specific card effects, making the system clean and easy to expand.
- **Full Card Draw & Effect Loop:**
  1.  Player enters a room with an icon (that hasn't been triggered before).
  2.  The `ProcessRoomEntryEffects` function calls the appropriate `Draw[CardType]Card` function from the Game Mode.
  3.  A `WBP_CardDisplay` UI widget is created and populated with the drawn card's data.
  4.  An **Event Dispatcher** (`OnCardDismissed`) allows the game to wait for player input.
  5.  When the player dismisses the card UI, a `Handle[CardType]Dismissed` event fires.
  6.  This event gets the card's `EffectComponentClass` from its data, adds it to the player character, and calls `ExecuteEffect`.
  7.  The effect component then runs its logic (e.g., making a stat roll with the `RollDice` function and applying results with the `ChangeStat` function).

### 5. Haunt Mechanic - Prerequisites

- **Omen Tracking:** The Game Mode now tracks `OmenCardCount`, which is correctly incremented after an Omen card's effect is resolved.
- **Haunt Roll:** After an Omen's effect is complete, a `CheckForHauntStart` function is called. It rolls 6 dice and compares the result to `OmenCardCount`.
- **Haunt Trigger (Placeholder):** A "THE HAUNT BEGINS!!!" message is printed if the Haunt Roll is failed, marking the successful implementation of the trigger mechanism.

### 6. User Interface (UI)

- **`WBP_PlayerHUD`:** Displays the character's current stats (Might, Speed, Sanity, Knowledge) and the name of the room they are in. Updates automatically when stats or location change.
- **`WBP_CardDisplay`:** A modal UI for displaying drawn cards.
- **`WBP_InteractionPrompt`:** Dynamically displays all context-sensitive prompts.

## Next Steps / Future Goals

- **The Haunt!** Implement the Haunt Chart lookup, Traitor selection process, and display of secret goals for Traitor and Heroes. (Next Up!)
- **Implement more Card Effects** using the established component-based system.
- **Implement Unique Room Rules** (using `bHasUniqueRule` flag).
- Prevent Room Overlaps during placement (spatial grid check).
- Top-Down Map Display for player reference.
- Multiplayer Functionality.

---

This project is a work in progress.
