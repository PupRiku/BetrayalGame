# Betrayal at House on the Hill - Unreal Engine 5 Adaptation

This project is a digital adaptation of the popular board game "Betrayal at House on the Hill," being developed in Unreal Engine 5. The primary goal is to create a visually engaging and atmospheric multiplayer experience.

**Repository:** [https://github.com/PupRiku/BetrayalGame.git](https://github.com/PupRiku/BetrayalGame.git)

## Current Project Status (As of June 2025)

The project has achieved a complete gameplay loop for the "first act" of the game and a fully functional transition into the "second act." All foundational systems are in place, including a data-driven framework for characters, rooms, and cards. A significant visual polish pass on the User Interface has been completed, resulting in a custom-themed, interactive HUD with animated elements. All Omen card effects have been implemented, and the core combat and movement systems are robust and rule-accurate.

## Features Implemented:

### 1. Player & Character Systems

- **Data-Driven Characters (`DT_Characters`):** A Data Table stores definitions for each unique character, including their **unique stat tracks**, color, and portrait.
- **Stat Management:** The player character correctly loads its stats and has a `ChangeStat` function to handle modifications.
- **Player Inventory:** The character has a functional inventory system. Kept Omen and Item cards are correctly added to the inventory and displayed on the HUD.
- **Rule-Accurate Movement:** Player movement is governed by "Movement Points" derived from their current Speed stat. Drawing any card correctly ends the movement phase for that turn.

### 2. Turn & Game State Management

- **Game State Machine (`EGameState`):** The Game Mode tracks the current game phase (`PreHaunt_Exploration`, `Haunt_HeroesTurn`, `Haunt_TraitorTurn`).
- **Haunt Turn Structure:** A basic turn manager is in place. An "End Turn" button correctly cycles the game state between the Heroes' and Traitor's turns once the Haunt begins.
- **Action Tracking:** A `bHasAttackedThisTurn` flag correctly enforces the rule that players can only attack once per turn.

### 3. Dice, Combat, & Damage

- **Custom Dice Rolling:** A `RollDice` function accurately simulates rolling any number of custom Betrayal dice.
- **Flexible Combat System:** A generic `ResolveAttack` function handles the core "dice-off," and a `TakeDamage` function applies `Physical` or `Mental` damage.
- **Player-Initiated Combat:** An `IA_Attack` input allows the player to initiate an attack, correctly disabled before the Haunt begins.
- **Interactive Combat & Damage:** The system presents UI choices to the player for abilities that modify combat or damage (e.g., using the **Ring** to attack with Sanity, or the **Skull** to take mental damage as physical).

### 4. Exploration & Room System

- **Data-Driven Rooms (`DT_RoomTiles`):** A fully populated Data Table defines all official rooms with their doors, windows, and card-drawing `IconType`.
- **Robust Room Deck Management:** At game start, separate, shuffled decks of rooms are created for each floor, excluding starting rooms. The system draws unique rooms and correctly removes globally unique tiles from all decks once drawn.
- **Player-Controlled Room Placement:** A full placement preview mode with live UI and visual feedback on placement validity based on door alignment.
- **Rotation-Aware Connections:** Placed rooms are logically connected using their correct rotated sides.

### 5. Card & Event System

- **Complete Data Foundation & Decks:** Data Tables and shuffled, depleting decks are set up for Omens, Events, and Items.
- **Component-Based Card Effects:** A clean, scalable system where each card's unique logic is contained in its own Actor Component.
- **Full Card Draw & Effect Loop:** The system correctly handles drawing a card, displaying its UI, waiting for player dismissal via an Event Dispatcher, and then executing the card's specific effect.
- **All Omen Cards Implemented:** The logic for all 13 Omen cards is complete.

### 6. The Haunt

- **Complete Haunt Reveal Sequence:** The game correctly tracks the `OmenCardCount`, triggers a Haunt Roll, looks up the specific Haunt in the `DT_HauntChart`, determines the Traitor, and displays the secret objectives.

### 7. User Interface (UI)

- **Polished Player HUD (`WBP_PlayerHUD`):**
  - Displays the character's portrait in a custom metallic frame.
  - Features custom-built **Stat Indicators** for each stat, with unique icons.
  - **Interactive Stat Tooltips:** Hovering over a stat displays the character's full stat track with the current value highlighted.
  - Displays the current room name on a custom-themed plaque.
  - Displays a dynamic inventory list.
- **Animated Omen Counter (`WBP_OmenCounter`):** A custom-themed UI element that displays the Omen count and plays a dynamic animation (glowing, swirling smoke, number morphing) when the count increases.
- **Modal Widgets:** Functional UI for Card Display, Haunt Briefings, and player choices.

## Up-to-Date Checklist

- `[x]` Project Setup & Version Control
- `[x]` Player Stats & Character System
- `[x]` Rule-Accurate Movement System
- `[x]` Custom Dice Rolling & Foundational Combat System
- `[x]` Room Data & Deck Management Systems
- `[x]` Player-Controlled Room Placement & Connection Logic
- `[x]` Card Data & Deck Management Systems
- `[x]` Component-Based Card Effect Architecture & All Omen Effects
- `[x]` Player Inventory System & HUD Display
- `[x]` Haunt Reveal & Traitor Selection Logic
- `[x]` Haunt Turn Structure (State Cycling)
- `[x]` All Foundational UI Widgets & Visual Polish Pass
- `[ ]` **Haunt Gameplay - Full Turn Management** (Handling individual hero turns within Heroes' phase) (Next Up!)
- `[ ]` Implement All Event & Item Card Effects
- `[ ]` Implement Player-Activated Abilities (e.g., Crystal Ball, Mask, Spirit Board)
- `[ ]` Implementation of all 50+ Haunt Scenarios
- `[ ]` Initial House Layout & Preventing Room Overlaps
- `[ ]` Top-Down Map Display
