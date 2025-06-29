# Betrayal at House on the Hill - Unreal Engine 5 Adaptation

This project is a digital adaptation of the popular board game "Betrayal at House on the Hill," being developed in Unreal Engine 5. The primary goal is to create a visually engaging and atmospheric multiplayer experience.

**Repository:** [https://github.com/PupRiku/BetrayalGame.git](https://github.com/PupRiku/BetrayalGame.git)

## Current Project Status (As of June 2025)

The project has achieved a complete gameplay loop for the "first act" of the game and a fully functional transition into the "second act." All foundational systems are in place, including a data-driven framework for characters, rooms, and all card types. **All 13 Omen, 22 Item, and 45 Event cards have been implemented**, featuring a robust, scalable system for immediate effects, passive bonuses, and interactive player choices. The core logic for player death, turn management, and combat is now in place, setting the stage for implementing the full Haunt scenarios.

## Features Implemented:

### 1. Player & Character Systems

- **Data-Driven Characters (`DT_Characters`):** A Data Table stores definitions for each unique character, including their **unique stat tracks**, color, and portrait.
- **Stat Management:** The player character correctly loads its stats and has robust functions to `ChangeStat` (relative amounts) and `RestoreStatToStart`.
- **Player Inventory:** The character has a functional inventory system. Kept Omen and Item cards are correctly added to the inventory and displayed on a dynamic, animated slide-out HUD panel.
- **Inventory Interaction:** A full UI flow allows players to select items in their inventory to trigger "Use", "Drop", "Trade", and "Equip" actions. The logic for dropping items (removing from player, adding to room), picking them up, and initiating trades is functional.
- **Rule-Accurate Movement:** Player movement is governed by "Movement Points" derived from their current Speed stat. Drawing any card correctly ends the movement phase for that turn.
- **Status Effects:** The character can be affected by persistent status effects like "Immobilized" (from Debris/Webs), "Soiled", and "Darkness", which apply specific penalties and have unique cure conditions.
- **Player Death:** Logic is in place within the `ChangeStat` function to detect a fatal amount of damage during the Haunt and trigger a `Die` event, which correctly removes the player from the game.

### 2. Turn & Game State Management

- **Game State Machine (`EGameState`):** The Game Mode tracks the current game phase (`PreHaunt_Exploration`, `Haunt_HeroesTurn`, `Haunt_TraitorTurn`).
- **Haunt Turn Structure:** A robust turn manager is in place. An "End Turn" button correctly cycles through each individual Hero's turn before passing control to the Traitor, and then back again.
- **Action Tracking:** The system tracks and enforces "once per turn" rules for actions like attacking and using specific items.

### 3. Dice, Combat, & Damage

- **Custom Dice Rolling:** A `RollDice` function accurately simulates rolling any number of custom Betrayal dice and can be modified by item effects.
- **Flexible Combat Functions:** A generic `ResolveAttack` function handles the core "dice-off."
- **Damage Allocation & Player Choice:** A `TakeDamage` pipeline correctly pre-calculates if damage is lethal. If not, it presents a UI (`WBP_DamageAllocation`) for the player to strategically distribute incoming Physical or Mental damage between their stats.
- **Player-Initiated Combat:** An `IA_Attack` input allows the player to initiate an attack, correctly disabled before the Haunt begins.
- **Stealing:** After a successful, high-damage physical attack, the attacker is correctly presented with the choice to deal damage or attempt to steal a valid item.

### 4. Exploration & Room System

- **Data-Driven Rooms (`DT_RoomTiles`):** A fully populated Data Table defines all official rooms with their doors, windows, and card-drawing `IconType`.
- **Interactable World Objects:** The game supports persistent, interactable objects spawned in rooms by events (e.g., Closet Door, Locked Safe, Secret Passages), with a dedicated player input to use them.
- **Programmatic Starting Layout:** At game start, the `CreateStartingLayout` function correctly builds the multi-floor starting area of the house (Entrance Hall, Foyer, etc.).
- **Pathfinding (Foundation):** A `BP_HouseGraphManager` correctly maps all room connections, and a `FindShortestPath` function using a Breadth-First Search algorithm is implemented. Helper functions to get distance and find the nearest explorer are also in place.

### 5. Card & Event System

- **Complete Data Foundation & Decks:** Data Tables and shuffled, depleting decks are set up for Omens, Events, and Items.
- **Component-Based Card Effects:** A clean, scalable system where each card's unique logic is contained in its own Actor Component.
- **Full Card Draw & Effect Loop:** The system correctly handles drawing a card, displaying its UI, waiting for player dismissal via an Event Dispatcher, and then executing the card's specific effect.
- **All Omen, Item, & Event Cards Implemented:** The logic for all card types has been implemented, covering a massive range of effects.

### 6. The Haunt

- **Complete Haunt Reveal Sequence:** The game correctly tracks the `OmenCardCount`, triggers a Haunt Roll, looks up the specific Haunt in the `DT_HauntChart`, determines the Traitor, and displays the secret objectives.
- **Monster System (Foundation):** A `BP_Monster_Base` class has been created with stats, inventory, and logic for being stunned and taking a turn. The Game Mode contains a system to manage the "Monster Turn" phase.

### 7. User Interface (UI)

- **Polished & Interactive HUD:** A custom-themed UI displays the character's portrait, interactive stat tooltips, a room name plaque, an animated slide-out inventory, and dynamic status effect icons.
- **Animated Omen Counter:** A custom-themed UI element that displays the Omen count and plays a dynamic animation.
- **Modal Widgets:** A full suite of functional UI for Card Display, Haunt Briefings, and complex player choices.

## Up-to-Date Checklist

- `[x]` Project Setup & Version Control
- `[x]` Player Stats & Character System
- `[x]` Rule-Accurate Movement System
- `[x]` Custom Dice Rolling & Foundational Combat System (including Stealing)
- `[x]` Room Data & Deck Management Systems
- `[x]` Player-Controlled Room Placement & Connection Logic
- `[x]` Card Data & Deck Management Systems
- `[x]` Component-Based Card Effect Architecture & All Card Effects
- `[x]` Player Inventory System & Full UI Interactivity
- `[x]` Haunt Reveal & Traitor Selection Logic
- `[x]` Haunt Turn Structure (Individual Hero Turns)
- `[x]` Player Death Mechanic
- `[x]` All Foundational UI Widgets & Visual Polish Pass
- `[ ]` **Implement a Full Haunt Scenario ("The Mummy Walks")** (Next Up!)
- `[ ]` Implement Special Room Rules
- `[ ]` Testing and Reworking Complex Card Abilities (Pathfinding for Dog, etc.)
- `[ ]` Build Visual Room Geometry
- `[ ]` Add AI Controlled "players"
- `[ ]` Multiplayer Functionality
