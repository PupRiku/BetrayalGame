# Betrayal at House on the Hill - Unreal Engine 5 Adaptation

This project is a digital adaptation of the popular board game "Betrayal at House on the Hill," being developed in Unreal Engine 5. The primary goal is to create a visually engaging and atmospheric multiplayer experience.

**Repository:** [https://github.com/PupRiku/BetrayalGame.git](https://github.com/PupRiku/BetrayalGame.git)

## Current Project Status (As of June 2025)

The project has achieved a complete gameplay loop for the "first act" of the game and a fully functional transition into the "second act." All foundational systems are in place, including a data-driven framework for characters, rooms, and all card types. **All 13 Omen cards and all 22 Item cards have been implemented**, featuring a robust, scalable system for immediate effects, passive bonuses, and interactive player choices. A rule-accurate, turn-based movement system, a functional combat system, and the full Haunt Reveal sequence are all complete and working in concert.

## Features Implemented:

### 1. Player & Character Systems

- **Data-Driven Characters (`DT_Characters`):** A Data Table stores definitions for each unique character, including their **unique stat tracks**, color, and portrait.
- **Stat Management:** The character correctly loads its stats and has robust functions to `ChangeStat` (relative amounts) and `RestoreStatToStart`.
- **Player Inventory:** The character has a functional inventory system. Kept Omen and Item cards are correctly added to the inventory and displayed on the HUD. A `HandleCardLoss` function correctly processes contingent effects for specific lost items.
- **Rule-Accurate Movement:** Player movement is governed by "Movement Points" derived from their current Speed stat. Moving between rooms consumes a point, and drawing any card correctly ends the movement phase for that turn.

### 2. Turn & Game State Management

- **Game State Machine (`EGameState`):** The Game Mode tracks the current game phase (`PreHaunt_Exploration`, `Haunt_HeroesTurn`, `Haunt_TraitorTurn`).
- **Haunt Turn Structure:** A basic turn manager is in place. An "End Turn" button correctly cycles the game state between the Heroes' and Traitor's turns once the Haunt begins.
- **Action Tracking:** The system tracks and enforces "once per turn" rules for actions like attacking and using certain items (e.g., Idol, Puzzle Box).

### 3. Dice, Combat, & Damage

- **Custom Dice Rolling:** A `RollDice` function accurately simulates rolling any number of custom Betrayal dice. This system has been upgraded to support "forced roll" results from items like the Angel Feather.
- **Flexible Combat Functions:** A generic `ResolveAttack` function handles the core "dice-off," and a `TakeDamage` function applies `Physical` or `Mental` damage and can be interrupted by UI choices (for the Skull).
- **Player-Initiated Combat:** An `IA_Attack` input allows the player to initiate an attack, correctly disabled before the Haunt begins.
- **Interactive Combat & Damage:** The system presents UI choices to the player for abilities that modify combat or damage (e.g., using the **Ring** to attack with Sanity, or the **Skull** to take mental damage as physical).
- **Item-Based Roll Modification:** The core rolling systems check for and correctly apply bonuses from items like the **Adrenaline Shot** (+ to result), **Idol** (+ to result with a cost), **Lucky Stone**, and **Rabbit's Foot** (post-roll re-roll choices).

### 4. Exploration & Room System

- **Data-Driven Rooms (`DT_RoomTiles`):** A fully populated Data Table defines all official rooms with their doors, windows, and card-drawing `IconType`.
- **Robust Room Deck Management:** At game start, separate, shuffled decks of rooms are created for each floor, excluding starting rooms. The system draws unique rooms and correctly removes globally unique tiles from all decks once drawn.
- **Player-Controlled Room Placement:** A full placement preview mode with live UI and visual feedback on placement validity based on door alignment.
- **Rotation-Aware Connections:** Placed rooms are logically connected using their correct rotated sides.

### 5. Card & Event System

- **Complete Data Foundation & Decks:** Data Tables and shuffled, depleting decks are set up for Omens, Events, and Items.
- **Component-Based Card Effects:** A clean, scalable system where each card's unique logic is contained in its own Actor Component.
- **Full Card Draw & Effect Loop:** The system correctly handles drawing a card, displaying its UI, waiting for player dismissal via an Event Dispatcher, and then executing the card's specific effect.
- **All Omen & Item Cards Implemented:** The logic for all 13 Omen cards and all 22 Item cards is complete, covering stat changes, companions, passive gear, combat events, and complex player-activated abilities.

### 6. The Haunt

- **Complete Haunt Reveal Sequence:** The game correctly tracks the `OmenCardCount`, triggers a Haunt Roll, looks up the specific Haunt in the `DT_HauntChart`, determines the Traitor, and displays the secret objectives.

### 7. User Interface (UI)

- **Polished Player HUD (`WBP_PlayerHUD`):** Displays the character's portrait in a custom frame, four interactive stat indicators with hover-tooltips showing the full stat track, a custom room name plaque, and a dynamic inventory list.
- **Animated Omen Counter:** A custom-themed UI element that displays the Omen count and plays a dynamic animation when the count increases.
- **Modal Widgets:** Functional UI for Card Display, Haunt Briefings, and player choices (Attack Type, Damage Type, Healing, Re-rolls, etc.).

## Up-to-Date Checklist

- `[x]` Project Setup & Version Control
- `[x]` Player Stats & Character System
- `[x]` Rule-Accurate Movement System
- `[x]` Custom Dice Rolling & Foundational Combat System
- `[x]` Room Data & Deck Management Systems
- `[x]` Player-Controlled Room Placement & Connection Logic
- `[x]` Card Data & Deck Management Systems
- `[x]` Component-Based Card Effect Architecture
- `[x]` **All Omen & Item Card Effects Implemented**
- `[x]` Player Inventory System & HUD Display
- `[x]` Haunt Reveal & Traitor Selection Logic
- `[x]` Haunt Turn Structure (State Cycling)
- `[x]` All Foundational UI Widgets & Visual Polish Pass
- `[ ]` **Implement All Event Cards** (Next Up!)
- `[ ]` **Haunt Gameplay - Full Turn Management** (Handling individual hero turns within Heroes' phase)
- `[ ]` Implement Player-Activated Abilities' missing UI/Systems (Crystal Ball, Mask, Spirit Board, Dynamite, etc.)
- `[ ]` Implementation of all 50+ Haunt Scenarios
- `[ ]` Initial House Layout & Preventing Room Overlaps
- `[ ]` Top-Down Map Display
