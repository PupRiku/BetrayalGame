# Betrayal at House on the Hill - Unreal Engine 5 Adaptation

This project is a digital adaptation of the popular board game "Betrayal at House on the Hill," being developed in Unreal Engine 5. The primary goal is to create a visually engaging and atmospheric multiplayer experience.

**Repository:** [https://github.com/PupRiku/BetrayalGame.git](https://github.com/PupRiku/BetrayalGame.git)

## Current Project Status (As of June 2025)

The project features a complete, end-to-end gameplay loop for the "first act" and the pivot into the Haunt. All foundational systems are in place, including a data-driven framework for characters, rooms, and all card types. The Omen card deck has been fully implemented, with each card's unique mechanics (stat changes, companions, passive bonuses, combat events, and player-activated abilities) handled by a robust, scalable component-based architecture. The initial Haunt reveal and Traitor selection are fully functional.

## Features Implemented:

### 1. Player Stats & Character System

- **Data-Driven Characters (`DT_Characters`):** A Data Table stores definitions for each unique character, including their **unique stat tracks** and color.
- **Stat Management:** The player character correctly loads its stats and has a `ChangeStat` function to handle modifications from game events, which are reflected on the Player HUD.
- **Player Inventory:** The character now has a functional inventory system. Kept Omen and Item cards are correctly added to the inventory and displayed on the HUD.

### 2. Dice, Combat, & Damage

- **Custom Dice Rolling:** A `RollDice` function accurately simulates rolling any number of custom Betrayal dice.
- **Foundational Combat System:** A `ResolveAttack` function in the Game Mode handles comparing an attacker's roll to a defender's roll.
- **Damage System:** A `TakeDamage` function on the character correctly applies Physical or Mental damage to the appropriate stats. It has been refactored to support player choices (e.g., for the Skull Omen).

### 3. Exploration & Room System

- **Data-Driven Rooms (`DT_RoomTiles`):** A fully populated Data Table defines all official rooms with their properties.
- **Robust Room Deck Management:** At game start, separate, shuffled decks of rooms are created for each floor, excluding starting tiles. The system draws uniquely from the decks and correctly handles removing globally unique tiles from all decks once drawn.
- **Player-Controlled Room Placement:** A full placement preview mode with live UI and visual feedback on placement validity. The player can rotate the preview, and the confirmation logic correctly validates and connects the rotated room.

### 4. Card & Event System

- **Complete Data Foundation:** Data Tables for Omens, Events, and Items are set up and populated.
- **Card Deck Management:** All three card decks are initialized, shuffled, and drawn from correctly, managing discard piles and empty-deck rules.
- **Component-Based Card Effects:** A clean, scalable system where each card's unique logic is contained in its own Actor Component (`BP_CardEffect_Base` children), linked from the Data Tables.
- **Full Card Draw & Effect Loop:** The system correctly handles drawing a card, displaying its UI, waiting for player dismissal via an Event Dispatcher, and then executing the card's specific component-based effect.
- **Omen Cards Fully Implemented:** The logic for all Omen cards, covering a wide variety of effect types (immediate stat changes, companions, passive weapon/gear bonuses, combat events, player-activated abilities), is now complete.

### 5. The Haunt

- **Complete Haunt Reveal Sequence:** The game correctly tracks the Omen count, triggers and performs a Haunt Roll, looks up the specific Haunt in the `DT_HauntChart` based on the triggering Omen and Room, retrieves the full `FHauntData`, determines the Traitor based on the Haunt's `TraitorSelectionRule`, and displays the secret objectives to the Traitor and Heroes using the `WBP_HauntBriefing` UI.

### 6. User Interface (UI)

- **`WBP_PlayerHUD`:** Displays character stats, current room name, and a dynamic inventory list.
- **`WBP_CardDisplay` & `WBP_HauntBriefing`:** Fully functional, data-driven UI for displaying cards and secret Haunt rules.
- **`WBP_DamageChoice`:** A UI for handling player choices during game events (e.g., the Skull Omen).

## Up-to-Date Checklist

- `[x]` Project Setup & Version Control
- `[x]` Player Stats & Character System (Data & Initialization)
- `[x]` Custom Dice Rolling Logic
- `[x]` Room Data Structures & Deck Management
- `[x]` Player-Controlled Room Placement & Connection Logic
- `[x]` Card Data Structures & Deck Management
- `[x]` Component-Based Card Effect Architecture
- `[x]` **All Omen Card Effects Implemented**
- `[x]` Player Inventory System (Adding Items/Omens)
- `[x]` Haunt Reveal & Traitor Selection Logic
- `[x]` UI for HUD, Cards, and Haunt Briefing
- `[ ]` **Haunt Turn Structure (Heroes' Turn, Traitor's Turn)** (Next Up!)
- `[ ]` **Full Combat System** (Attacking other players/monsters)
- `[ ]` Implement All Event & Item Card Effects
- `[ ]` Implement Player-Activated Abilities (e.g., Crystal Ball, Mask, Spirit Board)
- `[ ]` Turn Management System (Tracking turn phases)
- `[ ]` House Layout Graph & Pathfinding (for companion/monster movement)
- `[ ]` Physical Items & Companions as Actors in the World
- `[ ]` Implementation of all 50+ Haunt Scenarios
- `[ ]` Initial House Layout & Preventing Room Overlaps
- `[ ]` Top-Down Map Display
- `[ ]` Multiplayer Functionality
