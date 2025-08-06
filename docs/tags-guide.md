# Prism Canonical Tags Guide

This document provides a canonical list of recommended tags to be used within the `@@systemic` aspect of `.prism` files. Using these tags helps maintain consistency and discoverability across the entire lore universe.

While the format allows for any tag, adhering to this guide is highly recommended.

## Tag Format

The preferred format for tags is `Key:Value` (e.g., `Type:Character`). This structured approach is more robust for queries and filtering than single, unstructured tags.

## Canonical Tag List

### Type
The `Type` key is fundamental and should be present in almost every entity. It defines the basic nature of the entity.

*   **`Type:Character`**: For any sentient being, whether player-controlled or an NPC.
*   **`Type:Location`**: For any geographical place, from a small room to an entire planet.
*   **`Type:Item`**: For any inanimate object, such as a sword, a potion, or a key.
*   **`Type:Faction`**: For any organized group, such as a guild, a kingdom, or a cult.
*   **`Type:Story`**: For narrative content, like a chronicle, a folio, or a quest log.
*   **`Type:Concept`**: For abstract ideas, rules, or lore principles (e.g., "magic", "the-seven-lights").

### Subtype
The `Subtype` key provides a more specific classification within a `Type`.

*   **`Subtype:NPC`**: (For `Type:Character`) A non-player character.
*   **`Subtype:Village`**: (For `Type:Location`) A small settlement.
*   **`Subtype:City`**: (For `Type:Location`) A large urban center.
*   **`Subtype:Forest`**: (For `Type:Location`) A wooded area.
*   **`Subtype:Gang`**: (For `Type:Faction`) A small, often illicit, group.
*   **`Subtype:Kingdom`**: (For `Type:Faction`) A large, sovereign territory.

### Rarity
The `Rarity` key is used for items to indicate their prevalence.

*   **`Rarity:Common`**
*   **`Rarity:Uncommon`**
*   **`Rarity:Rare`**
*   **`Rarity:Legendary`**
*   **`Rarity:Unique`**

### Other Common Keys

*   **`Power`**: Describes the power level of an item or character (e.g., `Power:High`, `Power:Low`).
*   **`Role`**: Describes the function of a character (e.g., `Role:Healer`, `Role:Artisan`).
*   **`Culture`**: Associates an entity with a specific culture (e.g., `Culture:VillageFolk`).
*   **`Biome`**: For locations, describes the ecological environment (e.g., `Biome:Forest`, `Biome:Desert`).
*   **`Population`**: For locations, gives an idea of the number of inhabitants (e.g., `Population:Small`, `Population:Large`).
*   **`Alignment`**: Describes the moral or ethical leaning of a character or faction (e.g., `Alignment:Mysterious`, `Alignment:LawfulGood`).