# Betrayal at House on the Hill - Unreal Engine 5 Adaptation

This project is a digital adaptation of the popular board game "Betrayal at House on a Hill," being developed in Unreal Engine 5. The primary goal is to create a visually engaging and atmospheric multiplayer experience.

**Repository:** [https://github.com/PupRiku/BetrayalGame.git](https://github.com/PupRiku/BetrayalGame.git)

## Current Project Status (As of June 2025)

The project has achieved a complete gameplay loop for the "first act" of the game and a fully functional transition into the "second act." All foundational systems are in place, including a data-driven framework for characters, rooms, and all card types. **All 13 Omen, 22 Item, and 45 Event cards have been implemented**, featuring a robust, scalable system for immediate effects, passive bonuses, and interactive player choices. The player inventory is now fully interactive, with functional logic for using, dropping, picking up, and initiating trades.

## Features Implemented:

### 1. Player & Character Systems

- **Data-Driven Characters (`DT_Characters`):** A Data Table stores definitions for each unique character, including their **unique stat tracks**, color, and portrait.
- **Stat Management:** The player character correctly loads its stats and has robust functions to `ChangeStat` (relative amounts) and `RestoreStatToStart`.
- **Player Inventory:** The character has a functional inventory system. Kept Omen and Item cards are correctly added to the inventory and displayed on a dynamic, animated slide-out HUD panel.
- **Inventory Interaction:** A full UI flow allows players to select items in their inventory to trigger "Use", "Drop", and "Trade" actions. The logic for dropping items (removing from player, adding to room) and picking them up is fully implemented, along with a "Trade" initiation and confirmation system.
- **Rule-Accurate Movement:** Player movement is governed by "Movement Points" derived from their current Speed stat. Drawing any card correctly ends the movement phase for that turn.
- **Status Effects:** The character can be affected by persistent status effects like "Buried", "Soiled", and "Darkness", which apply specific penalties and have unique cure conditions that are checked by the game systems.

### 2. Turn & Game State Management

- **Game State Machine (`EGameState`):** The Game Mode tracks the current game phase (`PreHaunt_Exploration`, `Haunt_HeroesTurn`, `Haunt_TraitorTurn`).
- **Haunt Turn Structure:** A basic turn manager is in place. An "End Turn" button correctly cycles the game state between the Heroes' and Traitor's turns once the Haunt begins.
- **Action Tracking:** The system tracks and enforces "once per turn" rules for actions like attacking and using specific items.

### 3. Dice, Combat, & Damage

- **Custom Dice Rolling:** A `RollDice` function accurately simulates rolling any number of custom Betrayal dice and can be modified by item effects.
- **Flexible Combat Functions:** A generic `ResolveAttack` function handles the core "dice-off," and a `TakeDamage` function applies `Physical` or `Mental` damage and can be interrupted by UI choices.
- **Player-Initiated Combat:** An `IA_Attack` input allows the player to initiate an attack, correctly disabled before the Haunt begins.

### 4. Exploration & Room System

- **Data-Driven Rooms (`DT_RoomTiles`):** A fully populated Data Table defines all official rooms with their doors, windows, and card-drawing `IconType`.
- **Interactable World Objects:** The game supports persistent, interactable objects spawned in rooms by events (e.g., Closet Door, Locked Safe, Secret Passages), with a dedicated player input to use them.
- **Robust Room Deck Management:** At game start, separate, shuffled decks of rooms are created for each floor, excluding starting rooms.
- **Player-Controlled Room Placement:** A full placement preview mode with live UI and visual feedback on placement validity.
- **Rotation-Aware Connections:** Placed rooms are logically connected using their correct rotated sides. Special connections like Secret Passages are also supported.
- **Room Inventory:** Room tiles can now hold their own inventory of dropped items.

### 5. Card & Event System

- **Complete Data Foundation & Decks:** Data Tables and shuffled, depleting decks are set up for Omens, Events, and Items.
- **Component-Based Card Effects:** A clean, scalable system where each card's unique logic is contained in its own Actor Component.
- **Full Card Draw & Effect Loop:** The system correctly handles drawing a card, displaying its UI, waiting for player dismissal via an Event Dispatcher, and then executing the card's specific effect.
- **All Omen, Item, & Event Cards Implemented:** The logic for all card types has been implemented, covering a massive range of effects.

### 6. The Haunt

- **Complete Haunt Reveal Sequence:** The game correctly tracks the `OmenCardCount`, triggers a Haunt Roll, looks up the specific Haunt in the `DT_HauntChart`, determines the Traitor, and displays the secret objectives.

### 7. User Interface (UI)

- **Polished Player HUD (`WBP_PlayerHUD`):** Displays the character's portrait, interactive stat indicators with tooltips, a custom room name plaque, a dynamic inventory list, and status effect icons.
- **Animated Omen Counter:** A custom-themed UI element that displays the Omen count and plays a dynamic animation.
- **Modal Widgets:** A full suite of functional UI for Card Display, Haunt Briefings, and complex player choices (Attack Type, Damage Type, Target Selection, etc.).

## Up-to-Date Checklist

- `[x]` Project Setup & Version Control
- `[x]` Player Stats & Character System
- `[x]` Rule-Accurate Movement System
- `[x]` Custom Dice Rolling & Foundational Combat System
- `[x]` Room Data & Deck Management Systems
- `[x]` Player-Controlled Room Placement & Connection Logic
- `[x]` Card Data & Deck Management Systems
- `[x]` Component-Based Card Effect Architecture & All Card Effects
- `[x]` Player Inventory System & UI (Drop/Pickup/Use/Trade)
- `[x]` Haunt Reveal & Traitor Selection Logic
- `[x]` Haunt Turn Structure (State Cycling)
- `[x]` All Foundational UI Widgets & Visual Polish Pass
- `[ ]` **Add "Home Screen" UI and Menus** (Next Up!)
- `[ ]` **Haunt Gameplay - Full Turn Management** (Handling individual hero turns within Heroes' phase)
- `[ ]` Implement a Full Haunt Scenario ("The Mummy Walks")
- `[ ]` Implement Special Room Rules
- `[ ]` Testing and Reworking Complex Card Abilities (Pathfinding, etc.)
- `[ ]` Build Visual Room Geometry
- `[ ]` Add AI Controlled "players"
- `[ ]` Multiplayer Functionality
