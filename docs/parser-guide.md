# .prism Parser Implementation Guide

This document provides guidelines for developers who want to build a parser for the `.prism` format.

## 1. General Requirements

*   **Language:** The official Prismma parser will be implemented in Node.js for use in Server Actions, but a parser can be written in any language.
*   **Output:** The parser must produce a JSON data structure that represents the entity and its aspects, ready to be mapped to an item in DynamoDB.

## 2. Parsing Logic

The parser should follow a simple state machine logic:

1.  **Initial State:** Waiting for the entity header (`@entity`).
2.  **Reading Aspect Keys:** Inside an aspect block, reading `key: value` pairs.
3.  **Reading Multi-line Content:** In content reading mode for `@@textual` or `@@relations`.

### 2.1. Step-by-Step

1.  Read the file line by line.
2.  Ignore empty lines or lines starting with `#`.
3.  The first valid line MUST match the regex `^@entity:\s*([\w-]+)$`. Capture the entity ID.
4.  Any subsequent line that is `---` indicates a transition to a new aspect block.
5.  The first line of a new block MUST match the regex `^@@(\w+)(?:\((.*)\))?$`. Capture the aspect name and optional parameter.
6.  For aspects with `key: value` pairs, process each line with the regex `^([\w_]+):\s*(.*)$`.
7.  For `@@textual` and `@@relations`, after processing the initial keys, enter "content capture" mode until the next `---`.

## 3. Output JSON Structure (Example)

```json
{
  "id": "spark-to-create-king-lament",
  "aspects": {
    "systemic": {
      "title": "Proposal: Create the 'King's Lament' Entity",
      "owner": "@shaper-isaac",
      "status": "DRAFT",
      "access_view": "PUBLIC",
      "access_edit": "GUARDIANS",
      "tags": ["Lore", "Item", "Proposal"]
    },
    "proposal": [
      {
        "version": 1,
        "target_entities": ["@king-lament"],
        "proposal_status": "VOTING",
        "source_folio": "@chronicle-of-the-lonely-mountain",
        "discussion_thread": "@thread-discussion-king-lament",
        "content": "Justification: This sword is a central element in the 'Crown of Winter' Campaign and deserves to be canonized..."
      }
    ]
  }
}
```

## 4. Validation and Errors

The parser must be strict and return clear errors for:

*   Missing or malformed `@entity` header.
*   Missing `---` delimiter.
*   Malformed `@@` aspect header.
*   Missing required keys in an aspect.
*   Invalid data types (e.g., a non-integer for `version`).
