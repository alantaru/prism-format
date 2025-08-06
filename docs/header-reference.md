# Prism Header Reference Guide

This document provides a quick reference for all the special headers, tags, and delimiters used in the `.prism` format.

## 1. Entity Header (`@entity:`)

This is the mandatory first line of any `.prism` file. It declares the unique, machine-readable identifier for the entity.

*   **Syntax:** `@entity: {entity-id}`
*   **Purpose:** To uniquely identify the entity across the entire system.
*   **Rules:**
    *   Must be the first non-comment line.
    *   The ID should use `kebab-case` (e.g., `the-red-sword`).
*   **Example:**
    ```prism
    @entity: common-river-stone
    ```

## 2. Block Delimiter (`---`)

This delimiter is used to separate the main blocks of a `.prism` file: the entity header from the first aspect, and each subsequent aspect from the next.

*   **Syntax:** `---`
*   **Purpose:** To provide a clear and unambiguous separation between aspect blocks.
*   **Rules:**
    *   Must be on its own line.
*   **Example:**
    ```prism
    @entity: some-entity
    ---
    @@systemic
    title: Some Entity
    ---
    @@textual
    version: 1
    ```

## 3. Aspect Header (`@@{aspect_name}`)

This header marks the beginning of an aspect block. It defines which facet of the entity is being described (e.g., its system data, its visual representation, its relationships).

*   **Syntax:** `@@{aspect_name}` or `@@{aspect_name}({parameter})`
*   **Purpose:** To declare the type of an aspect block.
*   **Rules:**
    *   Must be the first line after a block delimiter (`---`).
    *   Some aspects, like `@@textual`, can take an optional parameter in parentheses.
*   **Example:**
    ```prism
    ---
    @@systemic
    title: An Entity
    ---
    @@textual(en-US)
    version: 1
    ```

## 4. Chapter Header (`@@chapter:`)

This is a special header used exclusively inside a `@@textual` aspect block to divide long-form text into named chapters.

*   **Syntax:** `@@chapter: {Chapter Title}`
*   **Purpose:** To structure narrative content within a single `textual` aspect.
*   **Rules:**
    *   Only valid inside a `@@textual` block.
*   **Example:**
    ```prism
    @@textual(en-US)
    version: 1

    @@chapter: The Beginning
    It was a dark and stormy night...

    @@chapter: The Climax
    Suddenly, a shot rang out!