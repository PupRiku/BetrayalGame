# Betrayal at House on the Hill - Unreal Engine 5 Adaptation

This project is a digital adaptation of the popular board game "Betrayal at House on the Hill," being developed in Unreal Engine 5. The primary goal is to create a visually engaging and atmospheric multiplayer experience.

**Repository:** [https://github.com/PupRiku/BetrayalGame.git](https://github.com/PupRiku/BetrayalGame.git)

## Current Project Status (As of June 2025)

The project has achieved a complete, end-to-end gameplay loop for the entire "first act" of the game and the pivotal transition into the "second act." The system can now proceed from initial exploration to drawing an Omen card, making a Haunt Roll, successfully looking up the specific Haunt from a data-driven chart, determining the Traitor based on the Haunt's rules, and distributing the secret objectives to the players. The foundational systems for all core mechanics are now in place.

## Features Implemented:

### 1. Player Stats & Character System

- **Data-Driven Characters (`DT_Characters`):** A Data Table stores definitions for each unique character, including their **unique stat tracks** (Might, Speed, Sanity, Knowledge) and color.
- **Stat Initialization & Modification:** The player character correctly loads its stats from the Data Table and has a function (`ChangeStat`) to handle stat changes from game events.

### 2. Exploration & Room System

- **Data-Driven Rooms (`DT_RoomTiles`):** A fully populated Data Table defines all official rooms with their names, floor allowances, default door configurations, and card-drawing `IconType`.
- **Robust Room Deck Management (`BP_BetrayalGameMode`):**
  - At game start, separate, shuffled decks of available room tiles are created for each floor.
  - Starting rooms are correctly excluded from the draw decks.
  - The system draws unique rooms from the appropriate deck, removing them after use and from all other potential decks if the tile is globally unique.
  - Correctly handles and reports when a floor-specific deck is empty.
- **Player-Controlled Room Placement:**
  - A full placement preview mode with live UI and visual feedback on placement validity.
  - Player can rotate the preview tile.
  - Confirmation logic correctly validates placement based on door alignment, spawns the permanent room with the chosen rotation, establishes correct two-way logical connections, and moves the player.

### 3. Dice & Card Systems

- **Dice Rolling:** A `RollDice` function accurately simulates rolling any number of custom Betrayal dice.
- **Card Data Structures & Decks:** Data Tables for Omens, Events, and Items are set up and managed by shuffled, depleting decks in the Game Mode.
- **Component-Based Card Effects (Scalable Architecture):** A clean, scalable system where each card's unique logic is contained in its own Actor Component (`BP_CardEffect_Base` children), which is specified in the card's data table row.
- **Full Card Draw & Effect Loop:** The system correctly handles:
  1.  Triggering a card draw upon entering a room with an icon.
  2.  Displaying the card's information in a UI widget (`WBP_CardDisplay`).
  3.  Waiting for player dismissal via an Event Dispatcher.
  4.  Executing the card's specific component-based effect, including making stat rolls and applying consequences.

### 4. The Haunt Reveal

- **Omen Tracking & Haunt Roll:** The game correctly tracks the number of Omens discovered and triggers a Haunt Roll (`CheckForHauntStart` function) after an Omen card's effects are resolved.
- **Haunt Chart Lookup:** When the Haunt Roll succeeds, the system uses the triggering Omen card and Room to look up the correct Haunt scenario in a `DT_HauntChart` Data Table.
- **Haunt Data Retrieval:** The corresponding Haunt's full data (name, rules, goals) is retrieved from a `DT_Haunts` Data Table.
- **Traitor Selection:** A `DetermineTraitor` function in the Game Mode parses the `TraitorSelectionRule` from the Haunt's data (e.g., "Haunt revealer," "Lowest Sanity") to correctly identify and flag the Traitor player.
- **Secret Information Distribution:** After the Traitor is determined, the system displays a `WBP_HauntBriefing` UI, showing the secret Traitor information (from the "Traitor's Tome") to the Traitor player, and the secret Hero information (from the "Secrets of Survival") to the Hero players.

### 6. User Interface (UI)

- **`WBP_PlayerHUD`:** Displays the character's current stats and room name.
- **`WBP_CardDisplay`:** A modal UI for displaying drawn cards.
- **`WBP_HauntBriefing`:** A scrollable UI for displaying detailed secret Haunt objectives.
- **Dynamic Interaction Prompts:** Context-sensitive prompts for all interactions.

## Up-to-Date Checklist

- [x] Project Setup & Version Control
- [x] Data-Driven Design (Data Tables)
- [x] Game Mode Setup
- [x] Player Character Setup
- [x] Data Structure for Characters (Unique Stats/Tracks/Colors)
- [x] Stat Initialization from Data Table
- [x]Stat Modification Function (ChangeStat)
- [x] Custom Dice Rolling Logic (RollDice function)
- [x] Room Data Structures & Data Table
- [x] Player Navigation (First-person, Centered, 90-degree smooth turns)
- [x] Room Deck Management (Initialization, Shuffling, Unique Draws, Empty Deck Handling)
- [x] Exclusion of Starting Rooms from Decks
- [x] Player-Controlled Room Placement (Preview & Rotation)
- [x] Rotation-Aware Validity Check & Connection Logic
- [x] Card Data Structures & Data Tables
- [x] Card Deck Management
- [x] Card Draw Triggering & One-Time Trigger Logic
- [x] Card Display UI & Asynchronous Handling (Event Dispatcher)
- [x] Component-Based Card Effect Architecture & First Implemented Effect
- [x] Omen Count Tracking
- [x] Haunt Roll Mechanic
- [x] Haunt Chart Data Structure & Lookup Logic
- [x] Traitor Selection Logic
- [x] Haunt Information Distribution & UI (WBP_HauntBriefing)
- [ ] Player Inventory System (for kept Items and Omens) (Next Up!)
- [ ] Implement All Remaining Omen, Event, and Item Card Effects
- [ ] Implementation of all 50+ Haunt Scenarios (Traitor/Hero actions, monster control, win condition checking)
- [ ] Initial House Layout (Placing Foyer, etc. at game start)
- [ ] Spatial Grid System (To prevent placed rooms from overlapping)
- [ ] Implementation of Unique Room Rules
- [ ] Top-Down Map Display
- [ ] Character Selection Screen
- [ ] Multiplayer Functionality

---

This project is a work in progress.
