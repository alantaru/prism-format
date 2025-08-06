# .prism Best Practices Guide

This guide offers recommendations for creating `.prism` files that are consistent, readable, and easy to maintain.

## 1. Entity Naming

*   **Use `kebab-case`:** For entity IDs, prefer the `kebab-case` format (lowercase, with words separated by hyphens). It is readable and URL-friendly.
    *   **Good:** `@entity: the-long-steel-sword`
    *   **Bad:** `@entity: The Long Steel Sword`
*   **Be Descriptive, But Concise:** The ID should give a clear idea of what the entity is.

## 2. Aspect Order

To maintain consistency across files, follow this order for aspect blocks:

1.  `@@systemic`
2.  The primary aspect of the entity (usually `@@textual` or `@@visual`)
3.  `@@relations`
4.  `@@spatial`
5.  `@@temporal`
6.  Other custom aspects
7.  `@@proposal` (if applicable)

## 3. Comments

*   **Use Comments for the "Why":** Don't describe what the code already says. Explain the *reason* behind a design decision.
    *   **Good:** `# Added relation with @cult-of-the-night to reflect the blade's corruption in the Shadow Era.`
    *   **Bad:** `# Adds a relation.`

## 4. Tags

*   **Use the `Key:Value` Format:** For tags that represent a category and a value, use the `Key:Value` format (e.g., `Rarity:Legendary`).
*   **Consistency is Key:** Use the same keys and values for similar concepts. Refer to the [Canonical Tags Guide](tags-guide.md) for the recommended list of tags.

## 5. Relations

*   **Clarity in Relations:** Use relation names that are clear and unambiguous (e.g., `ForgedWith` instead of just `MadeOf`).

## 6. Markdown in TextualAspect

*   **Use Formatting to Your Advantage:** Use headings (`###`), lists (`-` or `*`), and emphasis (`*italics*`, `**bold**`) to structure your text and make it more readable.
*   **Internal Links:** Whenever you mention another entity, use the Markdown link format with the entity ID: `[Entity Name](@entity-id)`.

## 7. Proposals (Sparks)

*   **Narrative Sparks:** If your proposal originates from a story or chronicle, always include the `source_folio` key in the `@@proposal` to maintain lore traceability.
