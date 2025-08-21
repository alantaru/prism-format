# Especificação do Formato de Hipertexto .prism (v3.1)

**Autor:** O Oráculo de Prismma
**Status:** Definitivo e Auditado (Protocolo Cerberus)

## Sumário

1.  [Filosofia](#1-filosofia)
2.  [Estrutura Fundamental](#2-estrutura-fundamental)
3.  [Especificação dos Aspectos](#3-especificação-dos-aspectos)
    -   [3.1. SystemicAspect (`@@systemic`)](#31-systemicaspect-systemic)
    -   [3.2. TextualAspect (`@@textual`)](#32-textualaspect-textual)
    -   ... (outros aspectos)
4.  [Tipos de Dados](#4-tipos-de-dados)
5.  [Diretivas Prismma (Metadados Estruturados)](#5-diretivas-prismma-metadados-estruturados)
6.  [Exemplo Completo](#6-exemplo-completo)
7.  [Histórico de Versões](#7-histórico-de-versões)

---

## 1. Filosofia

O formato `.prism` é um **superset do Markdown**, projetado para ser a espinha dorsal do universo Prismma. Ele segue três princípios:

*   **Fundamentado em Markdown:** A sintaxe completa do Markdown (CommonMark) é parte integrante e válida. As notações do `.prism` (como `@@aspecto` e as Diretivas) operam em um nível meta e **NÃO DEVEM** conflitar com a sintaxe Markdown padrão.
*   **Legibilidade Humana:** A sintaxe é otimizada para ser lida e escrita por humanos com o mínimo de ruído visual.
*   **Unificado e Extensível:** Um único formato capaz de representar qualquer tipo de lore, permitindo a adição de novos `Aspectos` no futuro.

## 2. Estrutura Fundamental

### 2.1. Codificação
Um arquivo `.prism` DEVE ser codificado em **UTF-8**.

### 2.2. Sintaxe de Blocos

*   **Cabeçalho da Entidade (`@entity`):** A primeira linha não-comentário DEVE ser `@entity: {id-unico-da-entidade}`.
*   **Delimitador de Bloco (`---`):** Três hifens em uma linha própria separam os blocos de `Aspecto`.
*   **Cabeçalho de Aspecto (`@@`):** A primeira linha de um novo bloco de Aspecto, no formato `@@{nome_do_aspecto}` ou opcionalmente `@@{nome_do_aspecto}({parametro})`.

## 3. Especificação dos Aspectos

Cada `Aspecto` representa uma faceta de uma `Entidade`.

### 3.1. SystemicAspect (`@@systemic`)
Contém metadados essenciais para a gestão e governança da entidade. **Obrigatório para todas as entidades.**

| Chave            | Tipo de Dado | Obrigatório | Descrição                                                 |
| :--------------- | :----------- | :---------- | :-------------------------------------------------------- |
| `version`        | Integer      | Sim         | O número da versão canônica da entidade. É a fonte única da verdade para o versionamento. |
| `schema_version` | String       | Sim         | A versão da especificação `.prism` usada. Ex: `"3.1"`.      |
| `title`          | String       | Sim         | O nome principal e legível da entidade.                   |
| `type`           | Enum         | Sim         | O tipo principal da entidade. A lista canônica é: `Conceito`, `Criatura`, `Faccao`, `Historia`, `Item`, `Local`. |
| `owner`          | ID           | Sim         | O ID do usuário ou circle "dono" da entidade.             |
| `status`         | Enum         | Sim         | Estado da entidade. Valores: `CANONICAL`, `DRAFT`, `PENDING`, `REJECTED`. |
| `access_view`    | Enum         | Sim         | Permissão de leitura. Valores: `PUBLIC`, `LOGGED_IN`, `PRIVATE`. |
| `access_edit`    | Enum         | Sim         | Permissão de edição. Valores: `OWNER`, `CIRCLE_MEMBERS`, `GUARDIANS`.|
| `primary_aspect` | Enum         | Não         | Indica qual aspecto deve ser exibido com destaque. Valores: `textual`, `visual`, `spatial`.|
| `tags`           | String List  | Não         | Lista de tags descritivas, separadas por vírgula, usadas para classificação detalhada (ex: `subtipo:espada`, `raridade:unica`, `is_template:true`). |
| `template_origin`| ID           | Não         | O ID da entidade marcada como `is_template:true` a partir da qual esta foi criada. **Obrigatório** se a entidade foi criada a partir de uma entidade com a tag `is_template:true`. |

### 3.2. TextualAspect (`@@textual` ou `@@textual(lang)`)
Contém conteúdo de texto rico. O cabeçalho pode opcionalmente incluir um código de idioma (ex: `pt-BR`).

**Conteúdo:** Após o cabeçalho, todo o texto até o próximo delimitador é o conteúdo em formato **Markdown (CommonMark)**, que pode incluir **Diretivas Prismma**.

*   **Capítulos (`@@chapter`):** O conteúdo pode ser dividido em seções usando a sintaxe `@@chapter: Título do Capítulo` em sua própria linha.
    *   **Capítulo de Descrição (Implícito):** Qualquer conteúdo de texto que exista *antes* da primeira declaração `@@chapter:` é considerado o capítulo principal ou a descrição da entidade. Um parser `.prism` **DEVE** preservar este conteúdo como o primeiro capítulo.
    *   **Múltiplos Capítulos:** Um `TextualAspect` pode conter zero ou mais declarações `@@chapter:`. Cada uma inicia um novo capítulo.

*   **Links Internos:** Referências a outras entidades DEVEM usar a sintaxe de link relativa padrão do Markdown, apontando para la rota unificada `/nexus/`: `[Texto do Link](/nexus/id-da-entidade)`.

(Definições de outros aspectos como `@@visual` e `@@relations` seguem aqui)

---

## 4. Tipos de Dados
*   **String:** Texto simples em uma única linha.
*   **String List:** Uma lista de strings, separadas por vírgula.
*   **ID:** String que identifica outra entidade ou usuário, prefixada com `@`. Formato `kebab-case` para entidades e `username` para usuários.
*   **Enum:** String que deve corresponder a um dos valores pré-definidos.
*   **URL:** Um Uniform Resource Locator válido.
*   **Integer:** Um número inteiro.
*   **Markdown:** Bloco de texto que segue a especificação CommonMark.

---

## 5. Diretivas Prismma (Metadados Estruturados)

Para evitar conflitos com a sintaxe HTML e Markdown, o formato `.prism` define uma sintaxe única e canônica para metadados e anotações chamada **Diretiva**.

### 5.1. Sintaxe
A sintaxe de uma Diretiva é: `(> @tag: conteúdo <)`

*   **Delimitadores:** `(>` e `<)` são usados para encapsular a diretiva. Esta sintaxe não conflita com nenhum elemento padrão de Markdown ou HTML.
*   **Tag da Diretiva:** Uma tag prefixada com `@` (ex: `@template_hint`) define o propósito da diretiva.
*   **Conteúdo:** O conteúdo da diretiva é o texto que se segue à tag.

### 5.2. Aninhamento (Nesting)
As diretivas **PODEM** ser aninhadas umas dentro das outras.

*   **Exemplo:** `(> @shaper_note: Lembre-se que (> @lore_suggestion: a espada pode estar amaldiçoada <) e observe a reação. <)`
*   **Requisito de Parsing:** Um parser `.prism` **DEVE** ser capaz de lidar com o aninhamento. Parsers baseados em expressões regulares simples são insuficientes e considerados uma violação desta especificação. A implementação correta **DEVE** utilizar uma máquina de estados ou construir uma Árvore de Sintaxe Abstrata (AST) para garantir que a estrutura aninhada seja preservada sem perda de dados.

### 5.3. Tags de Diretiva Canônicas
*   `@template_hint`: Uma instrução para criadores que utilizam a entidade como um template.
*   `@shaper_note`: Uma nota direcionada a um "World Shaper" (Mestre de Jogo).
*   `@lore_suggestion`: Uma sugestão não-canônica para expansão da lore.

---

## 6. Exemplo Completo

```prism
@entity: lamento-do-rei
---
@@systemic
version: 3
schema_version: "3.1"
title: Lamento do Rei
type: Item
owner: @shaper-isaac
status: CANONICAL
access_view: PUBLIC
access_edit: GUARDIANS
tags: subtipo:espada, raridade:unica
---
@@textual(pt-BR)
Uma espada lendária forjada nas lágrimas de um [Rei Caído](/nexus/rei-caido). Esta é a descrição principal da entidade.

(> @template_hint: Descreva aqui os poderes da espada. Considere adicionar uma diretiva aninhada de sugestão de lore. (> @lore_suggestion: A espada poderia ter uma consciência? <) <)

@@chapter: A Forja
A lâmina foi mergulhada no fogo estelar...
```

## 7. Histórico de Versões
*   **v3.1:** Introduzida a sintaxe de Diretivas Prismma `(> @tag: ... <)` como substituição canônica para os comentários. Reforçada a exigência de parsers baseados em AST para lidar com aninhamento. A sintaxe `<!-- ... -->` foi descontinuada. Adicionada definição explícita para o tratamento de capítulos.
*   **v3.0:** Versão inicial que consolidou o modelo de `Aspectos`.
