# Prism Format

A unified and extensible hypertext specification for lore universes.

## What is Prism?

The `.prism` format was designed based on three core principles:

*   **Unified and Extensible:** A single format capable of representing any type of lore (characters, locations, stories, items), allowing for the addition of new `Aspects` in the future.
*   **Human-Readable:** The syntax is optimized to be read and written by humans with minimal visual noise.
*   **Parsing Efficiency:** The block structure is simple and strict, facilitating computational analysis and validation.

## Quick Example

Here is how a simple entity, a "common river stone," is represented:

```prism
@entity: common-river-stone
---
@@systemic
title: Common River Stone
owner: @aegis-ai
status: CANONICAL
access_view: PUBLIC
tags: Type:Item, Rarity:Common
---
@@textual(en-US)
version: 1

A smooth, gray stone, found in abundance in any riverbed. It has no magical properties, but it can be useful for building something or as a makeshift weapon.
```

## Repository Structure

*   `docs/`: Contains the complete technical documentation for the format, including the [full specification](docs/specification.md), [best practices](docs/best-practices.md), implementation guides, and more.
*   `examples/`: Contains a variety of `.prism` example files that demonstrate different use cases of the format.

## How to Contribute

We encourage community contributions! If you have ideas for improving the format, found a bug in the specification, or want to add a new example, please read our [contribution guide](CONTRIBUTING.md).

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.