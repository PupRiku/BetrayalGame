# Betrayal at House on the Hill - Unreal Engine 5 Adaptation

This project is a digital adaptation of the popular board game "Betrayal at House on the Hill," being developed in Unreal Engine 5. The primary goal is to create a visually engaging and atmospheric multiplayer experience.

**Repository:** [https://github.com/PupRiku/BetrayalGame.git](https://github.com/PupRiku/BetrayalGame.git)

## Current Project Status (As of June 2025)

The project features a complete, end-to-end gameplay loop from initial exploration through the start of the Haunt. All foundational systems are in place, including a data-driven framework for characters, rooms, and cards. **All Omen cards have been implemented**, featuring a robust, scalable system for immediate effects, passive bonuses, and interactive player choices. A rule-accurate, turn-based movement system and a functional, player-initiated combat system are now complete, providing the core mechanics for both the pre-Haunt and Haunt phases of the game.

## Features Implemented:

### 1. Player & Character Systems

- **Data-Driven Characters (`DT_Characters`):** A Data Table stores definitions for each unique character, including their **unique stat tracks** and color.
- **Stat Management:** The player character correctly loads its stats and has a `ChangeStat` function to handle modifications from game events, which are reflected on the Player HUD.
- **Player Inventory:** The character has a functional inventory system. Kept Omen and Item cards are correctly added to the inventory and displayed on the HUD.
- **Rule-Accurate Movement:** Player movement is governed by "Movement Points" derived from their current Speed stat. Moving between rooms consumes a point. Drawing any card ends movement for the turn.

### 2. Turn & Game State Management

- **Game State Machine (`EGameState`):** The Game Mode tracks the current game phase (`PreHaunt_Exploration`, `Haunt_HeroesTurn`, `Haunt_TraitorTurn`).
- **Haunt Turn Structure:** A basic turn manager is in place. An "End Turn" button correctly cycles the game state between the Heroes' and Traitor's turns once the Haunt begins.
- **Action Tracking:** A `bHasAttackedThisTurn` flag correctly enforces the once-per-turn attack rule.

### 3. Dice, Combat, & Damage

- **Custom Dice Rolling:** A `RollDice` function accurately simulates rolling any number of custom Betrayal dice.
- **Flexible Combat Functions:** A generic `ResolveAttack` function handles the "dice-off," and a `TakeDamage` function applies Physical or Mental damage to the correct stats.
- **Player-Initiated Combat:** An `IA_Attack` input action allows the player to initiate an attack, but is correctly disabled before the Haunt begins.
- **Passive & Interactive Combat Effects:** The combat logic correctly checks the player's inventory for passive bonuses (e.g., the **Spear**) and presents interactive UI choices for abilities (e.g., using the **Ring** to attack with Sanity).

### 4. Exploration & Room System

- **Data-Driven Rooms (`DT_RoomTiles`):** A fully populated Data Table defines all official rooms.
- **Robust Room Deck Management:** At game start, separate, shuffled decks of rooms are created for each floor, excluding starting tiles. The system draws unique rooms from the decks.
- **Player-Controlled Room Placement:** A full placement preview mode with live UI and visual feedback on placement validity based on door alignment.
- **Rotation-Aware Connections:** Placed rooms are logically connected using their correct rotated sides.

### 5. Card & Event System

- **Complete Data Foundation & Decks:** Data Tables and shuffled, depleting decks are set up for Omens, Events, and Items.
- **Component-Based Card Effects:** A clean, scalable system where each card's unique logic is contained in its own Actor Component.
- **Full Card Draw & Effect Loop:** The system correctly handles drawing a card, displaying its UI, waiting for player dismissal via an Event Dispatcher, and then executing the card's specific effect.
- **All Omen Cards Implemented:** The logic for all 13 Omen cards is complete, covering stat changes, companions, passive gear, combat events, and player-activated abilities.

### 6. The Haunt

- **Complete Haunt Reveal Sequence:** The game correctly tracks the `OmenCardCount`, triggers a Haunt Roll, looks up the specific Haunt in the `DT_HauntChart`, determines the Traitor, and displays the secret objectives to the players.

### 7. User Interface (UI)

- **`WBP_PlayerHUD`:** Displays character stats, current room name, and a dynamic inventory list.
- **`WBP_CardDisplay` & `WBP_HauntBriefing`:** UI for displaying cards and secret Haunt rules.
- **`WBP_AttackChoice` & `WBP_DamageChoice`:** UI for handling player choices during game events.

## Up-to-Date Checklist

- `[x]` Project Setup & Version Control
- `[x]` Player Stats & Character System
- `[x]` Custom Dice Rolling & Foundational Combat System
- `[x]` Room Data & Deck Management Systems
- `[x]` Player-Controlled Room Placement & Connection Logic
- `[x]` Card Data & Deck Management Systems
- `[x]` Component-Based Card Effect Architecture & All Omen Effects
- `[x]` Player Inventory System
- `[x]` Haunt Reveal & Traitor Selection Logic
- `[x]` Haunt Turn Structure (State Cycling)
- `[x]` All Foundational UI Widgets
- `[ ]` **UI High Fidelity Polish (Player HUD, Cards, etc.)** (Next Up!)
- `[ ]` Implement All Event & Item Card Effects
- `[ ]` Implement Player-Activated Abilities (e.g., Crystal Ball, Mask, Spirit Board)
- `[ ]` Turn Management (Handling individual hero turns within Heroes' phase)
- `[ ]` House Layout Graph & Pathfinding (for companion/monster movement)
- `[ ]` Implementation of all 50+ Haunt Scenarios
- `[ ]` Initial House Layout & Preventing Room Overlaps
- `[ ]` Top-Down Map Display
