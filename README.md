# Betrayal at House on the Hill - Unreal Engine 5 Adaptation

This project is a digital adaptation of the popular board game "Betrayal at House on the Hill," being developed in Unreal Engine 5. The primary goal is to create a visually engaging and atmospheric multiplayer experience.

**Repository:** [https://github.com/PupRiku/BetrayalGame.git](https://github.com/PupRiku/BetrayalGame.git)

## Current Project Status (As of June 2025)

The project features a complete gameplay loop for the "first act" of the game and a fully functional transition into the "second act." All foundational systems are in place, including a data-driven framework for characters, rooms, and all card types. **All Omen cards have been implemented**, featuring a robust, scalable system for immediate effects, passive bonuses, and player-activated abilities. A foundational combat system has been built to handle attacks, defense, and damage, laying the groundwork for the Haunt phase of the game.

## Features Implemented:

### 1. Player Stats & Character System

- **Data-Driven Characters (`DT_Characters`):** A Data Table stores definitions for each unique character, including their **unique stat tracks** and color.
- **Stat Management:** The player character correctly loads its stats and has a `ChangeStat` function to handle modifications from game events, which are reflected on the Player HUD.
- **Player Inventory:** The character has a functional inventory system. Kept Omen and Item cards are correctly added to the inventory and displayed on the HUD.

### 2. Dice, Combat, & Damage

- **Custom Dice Rolling:** A `RollDice` function accurately simulates rolling any number of custom Betrayal dice.
- **Flexible Combat Functions:**
  - A generic **`ResolveAttack`** function in the Game Mode handles the core "dice-off" between an attacker and a defender, capable of using different stats for attack and defense.
  - A **`TakeDamage`** function on the character correctly applies `Physical` or `Mental` damage to the corresponding stats (Might or Sanity).
- **Player-Initiated Combat:**
  - An `IA_Attack` input action allows the player to initiate an attack.
  - The `MakeAttackRoll` function on the character serves as the "brain" for an attack, checking the character's inventory for passive bonuses (e.g., the **Spear** adding +2 dice to Might attacks) before calling `ResolveAttack`.
- **Interactive Choices:** The system now supports player choices during combat events. When attacking with the **Ring**, a UI widget (`WBP_AttackChoice`) appears, allowing the player to choose between a Might or Sanity attack. A similar UI (`WBP_DamageChoice`) is used for defensive choices like the **Skull**.

### 3. Exploration & Room System

- **Data-Driven Rooms (`DT_RoomTiles`):** A fully populated Data Table defines all official rooms.
- **Robust Room Deck Management:** At game start, separate, shuffled decks of rooms are created for each floor, excluding starting tiles. The system draws unique rooms and correctly handles removing globally unique tiles from all decks once drawn.
- **Player-Controlled Room Placement:** A full placement preview mode with live UI and visual feedback on placement validity. The player can rotate the preview, and the confirmation logic correctly validates and connects the rotated room.

### 4. Card & Event System

- **Complete Data Foundation:** Data Tables for Omens, Events, and Items are set up and populated.
- **Card Deck Management:** All three card decks are initialized, shuffled, and drawn from correctly.
- **Component-Based Card Effects:** A clean, scalable system where each card's unique logic is contained in its own Actor Component (`BP_CardEffect_Base` children).
- **Full Card Draw & Effect Loop:** The system correctly handles drawing a card, displaying its UI, waiting for player dismissal via an Event Dispatcher, and then executing the card's specific effect.
- **All Omen Cards Implemented:** The logic for all 13 Omen cards is complete, covering a wide variety of effect types: immediate stat changes (Book, Holy Symbol), companions (Girl, Madman, Dog), passive weapons (Spear), passive gear (Ring), combat events (Bite), and player-activated abilities (Crystal Ball, Mask, Spirit Board).

### 5. The Haunt

- **Complete Haunt Reveal Sequence:** The game correctly tracks the `OmenCardCount`, triggers a Haunt Roll, looks up the specific Haunt in the `DT_HauntChart` based on the Omen/Room combo, retrieves the full `FHauntData`, determines the Traitor, and displays the secret objectives to the Traitor and Heroes.

### 6. User Interface (UI)

- **`WBP_PlayerHUD`:** Displays character stats, current room name, and a dynamic inventory list.
- **`WBP_CardDisplay` & `WBP_HauntBriefing`:** UI for displaying cards and secret Haunt rules.
- **`WBP_DamageChoice` & `WBP_AttackChoice`:** UI for handling player choices during game events.

## Up-to-Date Checklist

- `[x]` Project Setup & Version Control
- `[x]` Player Stats & Character System (Data, Init, Modification)
- `[x]` Custom Dice Rolling Logic & Foundational Combat System
- `[x]` Room Data & Deck Management Systems
- `[x]` Player-Controlled Room Placement & Connection Logic
- `[x]` Card Data & Deck Management Systems
- `[x]` Component-Based Card Effect Architecture
- `[x]` **All Omen Card Effects Implemented**
- `[x]` Player Inventory System
- `[x]` Haunt Reveal & Traitor Selection Logic
- `[x]` UI for HUD, Cards, and Briefings
- `[ ]` **Haunt Turn Structure (Heroes' Turn, Traitor's Turn)** (Next Up!)
- `[ ]` Implement All Event & Item Card Effects
- `[ ]` Implement Player-Activated Abilities (e.g., Crystal Ball, Mask, Spirit Board)
- `[ ]` Turn Management System (Tracking turn phases)
- `[ ]` House Layout Graph & Pathfinding (for companion/monster movement)
- `[ ]` Physical Items & Companions as Actors in the World
- `[ ]` Implementation of all 50+ Haunt Scenarios
- `[ ]` Initial House Layout & Preventing Room Overlaps
- `[ ]` Top-Down Map Display
- `[ ]` Multiplayer Functionality
