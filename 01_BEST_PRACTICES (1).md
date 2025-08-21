# Guia de Boas Práticas para .prism (v3.1)

Este guia oferece recomendações para criar arquivos `.prism` que sejam consistentes, legíveis e fáceis de manter, de acordo com a especificação v3.1 auditada pelo Protocolo Cerberus.

## 1. Nomenclatura de Entidades

*   **Siga a Especificação:** A especificação canônica do formato `.prism` define rigorosamente como os IDs devem ser formatados. É crucial seguir estas regras para garantir a integridade dos dados:
    *   **IDs de Entidade:** Para identificar outras entidades (`@entity`, `template_origin`, relações), use o formato `kebab-case` (minúsculas, com palavras separadas por hífen).
        *   **Bom:** `@entity: a-espada-longa-de-aco`
        *   **Ruim:** `@entity: A_Espada_Longa_de_Aco`
    *   **IDs de Usuário:** Para campos como `owner` ou `author`, use o `username` exato do usuário na plataforma, **prefixado com `@`**.
        *   **Bom:** `owner: @alantaru`
        *   **Ruim:** `owner: alantaru`

## 2. Ordem dos Aspectos

Para manter a consistência entre os arquivos, siga esta ordem para os blocos de aspecto:

1.  `@@systemic` (Sempre o primeiro)
2.  O aspecto primário da entidade (geralmente `@@textual` ou `@@visual`)
3.  `@@relations`
4.  Outros aspectos em ordem alfabética.

## 3. Uso de Diretivas Prismma

A sintaxe de comentário `<!-- ... -->` foi descontinuada. Use as **Diretivas Prismma** `(> @tag: ... <)` para todos os metadados e anotações.

*   **Use para o "Porquê":** Use diretivas para adicionar notas, explicações ou lembretes que não devem fazer parte do conteúdo canônico, mas são úteis para outros criadores (ou para você mesmo no futuro).
*   **Seja Intencional com as Tags:** Use a tag de diretiva apropriada para a sua anotação.
    *   **Bom:** `(> @template_hint: Descreva aqui a aparência do item. <)`
    *   **Bom:** `(> @lore_suggestion: Adicionar relação com @culto-da-noite para refletir a corrupção da lâmina. <)`
*   **Preservação é Chave:** Lembre-se que um editor de `.prism` deve preservar as Diretivas durante o salvamento. Elas são parte do arquivo-fonte e essenciais para a manutenção.

## 4. Estruturando Textos com Capítulos

*   **Descrição Primeiro:** Sempre coloque o conteúdo descritivo geral da sua entidade no início do `@@textual`. Este conteúdo será tratado como o capítulo "principal" ou a descrição.
*   **Use Capítulos para Seções:** Para textos mais longos, use `@@chapter: Título do Capítulo` para dividir o conteúdo em seções lógicas, como "História", "Aparência", "Poderes", etc. Isso melhora drasticamente a legibilidade e a navegação.

## 5. Tags

*   **Use o Formato `Chave:Valor`:** Para tags que representam uma categoria e um valor, use o formato `Chave:Valor` (ex: `raridade:lendaria`).
*   **Consistência é Chave:** Use as mesmas chaves e valores para conceitos similares, conforme o Guia de Tags Canônicas.

## 6. Markdown no TextualAspect

*   **Siga o CommonMark:** Use a formatação padrão do Markdown a seu favor: cabeçalhos (`#`, `##`), listas (`-`, `*`), ênfase (`*itálico*`, `**negrito**`) para estruturar seu texto.
*   **Links Internos:** Sempre que mencionar outra entidade no corpo do texto, use o formato de link canônico do Markdown: `[Nome da Entidade](/nexus/id-da-entidade)`.

## 7. Templates e Proveniência

*   **Rastreabilidade:** Se uma entidade for criada a partir de um template (uma entidade com a tag `is_template:true`), o campo `template_origin` no `@@systemic` DEVE ser preenchido com a ID da entidade de template. Isso é crucial para a manutenção e rastreabilidade da lore.
    *   **Exemplo:** `template_origin: @template-para-personagens-heroicos`
