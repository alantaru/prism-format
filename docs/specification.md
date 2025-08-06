# .prism Hypertext Format Specification (v2.1)

**Author:** The Oracle of Prismma
**Status:** Official

## Table of Contents

1.  [Introduction](#1-introduction)
2.  [File Structure](#2-file-structure)
3.  [Detailed Syntax](#3-detailed-syntax)
4.  [Aspect Specification](#4-aspect-specification)
5.  [Detailed Data Types](#5-detailed-data-types)
6.  [Complete Example](#6-complete-example)
7.  [Version History](#7-version-history)
8.  [Quick Reference Guides](./)
    *   [Header Reference Guide](header-reference.md)
    *   [Canonical Tags Guide](tags-guide.md)

---

## 1. Introduction

### 1.1. Design Philosophy

*   **Unified and Extensible:** A single format capable of representing any type of lore, allowing for the addition of new `Aspects` in the future.
*   **Human-Readable:** The syntax is optimized to be read and written by humans with minimal visual noise.
*   **Parsing Efficiency:** The block structure is simple and strict, facilitating computational analysis and validation.

### 1.2. Terminology

*   **Entity:** A unique concept or object in the Prismma universe (a character, a place, a sword, a story). Represented by a `.prism` file.
*   **Aspect:** A facet or property of an Entity (its text, its image, its relations).
*   **Parser:** The program that reads a `.prism` file and converts it into a data structure (JSON) for use on the platform.

## 2. File Structure

### 2.1. Character Encoding

A `.prism` file MUST be encoded in **UTF-8**.

### 2.2. Comments

A line that starts with the `#` (hash) character is a comment. Comments are ignored by the parser and can appear anywhere.

## 3. Detailed Syntax

### 3.1. Entity Header

*   **Syntax:** `@entity: {id}`
*   **Description:** The first non-comment line of a `.prism` file MUST be the entity header. It declares the unique identifier of the entity.

### 3.2. Block Delimiter

*   **Syntax:** `---`
*   **Description:** Three hyphens on their own line separate the blocks.

### 3.3. Aspect Header

*   **Syntax:** `@@{aspect_name}` or `@@{aspect_name}({parameter})`
*   **Description:** The first line of an Aspect Block. It declares the aspect's type.

## 4. Aspect Specification

### 4.1. SystemicAspect (`@@systemic`)
Contains essential metadata for entity management.

| Key            | Data Type   | Required | Description                                                                 |
| :------------- | :---------- | :------- | :-------------------------------------------------------------------------- |
| `title`        | String      | Yes      | The main, human-readable name of the entity.                                |
| `owner`        | ID          | Yes      | The ID of the user or circle that "owns" the entity for management purposes. For art, `@@visual` has an `author` field for the original creator. |
| `status`       | Enum        | Yes      | State: `CANONICAL`, `DRAFT`, `REJECTED`.                                    |
| `access_view`  | Enum        | Yes      | Read permission: `PUBLIC`, `LOGGED_IN`, `PRIVATE`.                          |
| `access_edit`  | Enum        | Yes      | Edit permission: `OWNER`, `CIRCLE_MEMBERS`, `GUARDIANS`.                    |
| `impact_level` | Enum        | No       | Impact level: `LOCAL`, `REGIONAL`, `GLOBAL`. Default: `LOCAL`.              |
| `primary_aspect`| Enum       | No       | Indicates which aspect should be prominently displayed. Ex: `textual`, `visual`. Makes rendering explicit. |
| `tags`         | String List | No       | List of agnostic tags, separated by commas.                                 |

### 4.2. TextualAspect (`@@textual`)
Contains rich text content.

*   **Header Parameter:** `(language_code)` - The language code (e.g., `pt-BR`, `en-US`).

| Key       | Data Type | Required | Description                             |
| :-------- | :-------- | :------- | :-------------------------------------- |
| `version` | Integer   | Yes      | The active version number of this aspect. |

**Content:** After the keys, all text until the next delimiter is the content in **Markdown** format.

#### 4.2.1. Chapters (Optional)

For long texts, like "Folios," the content can be divided into chapters using the `@@chapter: <Chapter Title>` tag.

**Example:**

```
@@textual
version: 1

@@chapter: The Beginning

Once upon a time, in a faraway land...

@@chapter: The Adventure

... the journey began.
```

### 4.3. VisualAspect (`@@visual`)
Link to the visual representation of the entity.

| Key         | Data Type | Required | Description                                    |
| :---------- | :-------- | :------- | :--------------------------------------------- |
| `version`   | Integer   | Yes      | The active version number of this aspect.      |
| `url`       | URL       | Yes      | The link to the file in Cloud Storage.         |
| `media_type`| String    | Yes      | The MIME type of the file (e.g., `image/png`). |
| `author`    | ID        | Yes      | The ID of the user who created the art.        |
| `license`   | String    | Yes      | The usage license for the art (e.g., `CC BY-NC-SA 4.0`).|

### 4.4. SpatialAspect (`@@spatial`)
Defines the geographical presence of the entity.

| Key        | Data Type | Required | Description                                    |
| :--------- | :-------- | :------- | :--------------------------------------------- |
| `version`  | Integer   | Yes      | The active version number of this aspect.      |
| `shard`    | ID        | Yes      | The ID of the Shard where the entity is located. |
| `geometry` | GeoJSON   | Yes      | A GeoJSON representation of the object.        |

### 4.5. TemporalAspect (`@@temporal`)
Defines the temporal presence of the entity.

| Key               | Data Type     | Required | Description                                    |
| :---------------- | :------------ | :------- | :--------------------------------------------- |
| `version`         | Integer       | Yes      | The active version number of this aspect.      |
| `start_date`      | ISO 8601 Date | Yes      | Start date of the event/validity.              |
| `end_date`        | ISO 8601 Date | No       | End date (optional).                           |
| `precision`       | Enum          | No       | Precision: `YEAR`, `MONTH`, `DAY`. Default: `DAY`. |
| `recurrence_rule` | String        | No       | Recurrence rule (iCalendar RRULE format).      |

### 4.6. RelationalAspect (`@@relations`)
Defines the connections of this entity with others.

| Key       | Data Type | Required | Description                             |
| :-------- | :-------- | :------- | :-------------------------------------- |
| `version` | Integer   | Yes      | The active version number of this aspect. |

**Content:** After the keys, a list of relations, one per line, in the format `{relationship_type}: {target_entity_id}`.

### 4.7. ProposalAspect (`@@proposal`)
Represents a proposal (Spark) to create or modify other entities.

| Key                 | Data Type | Required | Description                                    |
| :------------------ | :-------- | :------- | :--------------------------------------------- |
| `version`           | Integer   | Yes      | The active version number of this aspect.      |
| `target_entities`   | ID List   | Yes      | IDs of the entities to be modified.            |
| `proposal_status`   | Enum      | Yes      | Status: `VOTING`, `APPROVED`, `REJECTED`.      |
| `source_folio`      | ID        | No       | ID of the Folio that originated this Narrative Spark. |
| `discussion_thread` | ID        | No       | ID of the forum thread where the Spark is discussed. |

**Content:** After the keys, a **Markdown** text block with the justification and proposed changes.

### 4.8. AdaptationAspect (`@@adaptation`)
Represents an adaptation of an existing entity.

| Key               | Data Type | Required | Description                                    |
| :---------------- | :-------- | :------- | :--------------------------------------------- |
| `version`         | Integer   | Yes      | The active version number of this aspect.      |
| `target_entity`   | ID        | Yes      | ID of the entity to be adapted.                |
| `adaptation_type` | Enum      | Yes      | Adaptation type: `TRANSLATION`, `REMIX`, `REIMAGINING`. |

**Content:** After the keys, a **Markdown** text block with the justification and proposed changes.

### 4.9. CommentAspect (`@@comment`)
Represents a comment on an entity.

| Key              | Data Type | Required | Description                                    |
| :--------------- | :-------- | :------- | :--------------------------------------------- |
| `version`        | Integer   | Yes      | The active version number of this aspect.      |
| `author`         | ID        | Yes      | ID of the comment's author.                    |
| `target_entity`  | ID        | Yes      | ID of the entity to be commented on.           |
| `target_aspect`  | Enum      | Yes      | Aspect of the entity to be commented on.       |
| `target_chapter` | ID        | No       | ID of the chapter to be commented on.          |

**Content:** After the keys, a **Markdown** text block with the comment's content.

### 4.10. TemplateAspect (`@@template`)
Represents a template for creating new entities.

| Key           | Data Type | Required | Description                             |
| :------------ | :-------- | :------- | :-------------------------------------- |
| `version`     | Integer   | Yes      | The active version number of this aspect. |
| `name`        | String    | Yes      | Name of the template.                   |
| `description` | String    | No       | Description of the template.            |

**Content:** After the keys, a **Markdown** text block with the template's content.

## 5. Detailed Data Types

*   **String:** Simple text on a single line.
*   **Integer:** An integer number.
*   **Enum:** A value from a predefined list (e.g., `CANONICAL`).
*   **ID:** A string that identifies another entity or user, prefixed with `@` (e.g., `@king-lament`).
*   **List (String/ID):** A comma-separated list of values.
*   **Markdown:** A multi-line text block that follows Markdown syntax.
*   **URL:** A Uniform Resource Locator.
*   **GeoJSON:** A JSON object formatted according to the GeoJSON specification.
*   **ISO 8601 Date:** A date/time in the `YYYY-MM-DDTHH:MM:SSZ` format.

## 6. Complete Example

```prism
@entity: king-lament
---
@@systemic
title: King's Lament
owner: @shaper-isaac
status: CANONICAL
access_view: PUBLIC
access_edit: GUARDIANS
primary_aspect: visual
tags: Rarity:Unique, Power:High, Type:Sword, Artifact
---
@@textual(en-US)
version: 3

@@chapter: The Beginning

Once upon a time, in a faraway land...

@@chapter: The Adventure

... the journey began.
---
@@visual
version: 1
url: gs://prismma-assets/swords/king-lament-v1.png
thumbnail_url: gs://prismma-assets/swords/king-lament-v1_thumb.webp
media_type: image/png
author: @legendary_artist
license: CC BY-NC-SA 4.0
---
@@temporal
version: 1
start_date: 1452-03-15T00:00:00Z
precision: DAY
---
@@relations
version: 4
DiscoveredBy: @elara-the-adventurer
ForgedWith: @starmetal
RestingPlace: @tomb-of-the-forgotten-queen
---
@@adaptation
version: 1
target_entity: @king-lament
adaptation_type: REMIX

This is an adaptation of the King's Lament sword, with a new design and powers.
---
@@comment
version: 1
author: @curious-reader
target_entity: @king-lament
target_aspect: textual
target_chapter: @the-beginning

This is a comment about the beginning of the story.
---
@@template
version: 1
name: Legendary Sword
description: A template for creating new legendary swords.

@@systemic
title: New Legendary Sword
owner: @creator
status: DRAFT
access_view: PUBLIC
access_edit: OWNER
primary_aspect: visual
tags: Rarity:Legendary, Power:High, Type:Sword
---
@@visual
version: 1
url: gs://prismma-assets/swords/new-legendary-sword.png
thumbnail_url: gs://prismma-assets/swords/king-lament-v1_thumb.webp
media_type: image/png
author: @creator
license: CC BY-NC-SA 4.0
```

## 7. Version History

*   **v2.1 (Current):** Added `source_folio` and `discussion_thread` to `ProposalAspect`.
*   **v2.0:** Added support for chapters, adaptations, and comments.
*   **v1.3:** Added a complete example and more details to the specification.
*   **v1.2:** Added the optional `primary_aspect` field to `SystemicAspect` to make UI rendering explicit. Clarified the distinction between `owner` and `author`.
*   **v1.1:** Added concepts of Ownership/Access, Aspect Versioning, and the `ProposalAspect`.
*   **v1.0:** Initial proposal of the format.
