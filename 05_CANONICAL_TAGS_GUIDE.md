# Guia de Tags Canônicas do .prism (v3.0)

Este documento fornece uma lista canônica de chaves e valores recomendados para serem usados dentro do aspecto `@@systemic` dos arquivos `.prism`. O uso dessas diretrizes ajuda a manter a consistência e a capacidade de descoberta em todo o universo de lore.

Embora o sistema permita flexibilidade, a adesão a esta taxonomia é altamente recomendada para a clareza da plataforma.

## Taxonomia: Tipos e Tags

A classificação de uma entidade segue uma hierarquia:

1.  **`type`**: O tipo fundamental e governado da entidade. É um campo obrigatório e de alto nível.
2.  **`tags`**: Um conjunto de descritores de formato livre (`chave:valor`) para adicionar detalhes e nuances, incluindo subtipos (ex: `subtipo:cidade`) ou características booleanas (ex: `is_template:true`).

---

## 1. Tipos de Entidade (`type`)

A chave `type` é fundamental e deve estar presente em todas as entidades. Ela define a natureza básica da entidade. A lista de tipos canônicos é governada e só deve ser expandida através de um processo formal.

*   **`Conceito`**: Para ideias abstratas, regras ou princípios de lore (ex: "Magia", "As Sete Luzes").
*   **`Criatura`**: Para qualquer ser senciente, seja um personagem de jogador (PC), um NPC ou um monstro.
*   **`Faccao`**: Para qualquer grupo organizado, como uma guilda, um reino ou um culto.
*   **`Historia`**: Para conteúdo narrativo, como uma crônica, um fólio ou um registro de missão.
*   **`Item`**: Para qualquer objeto inanimado, como uma espada, uma poção ou uma chave.
*   **`Local`**: Para qualquer lugar geográfico, de uma pequena sala a um planeta inteiro.

## 2. Tags Comuns (`tags`)

Tags usam o formato `chave:valor` para adicionar detalhes e descritores. Todos os valores devem seguir o formato `kebab-case`.

### Tags de Classificação
*   **`subtipo`**: Fornece uma classificação mais específica dentro de um `type`. Estes são exemplos comuns e não uma lista exaustiva.
    *   Para `type: Local`: `subtipo:vila`, `subtipo:cidade`, `subtipo:floresta`
    *   Para `type: Item`: `subtipo:arma`, `subtipo:armadura`, `subtipo:pocao`
    *   Para `type: Criatura`: `subtipo:personagem-jogador`, `subtipo:npc-comerciante`, `subtipo:monstro-errante`

### Tags de Metadados
*   **`is_template`**: Uma tag booleana (`is_template:true` ou `is_template:false`) que indica se a entidade deve ser usada como um modelo para a criação de outras.
    *   **Nota:** As entidades geradas a partir de um template devem referenciá-lo usando o campo `template_origin` no `@@systemic`.

### Outras Chaves de Tags Comuns
*   **`raridade`**: Para itens, indica sua prevalência (ex: `raridade:comum`, `raridade:rara`, `raridade:unica`).
*   **`poder`**: Descreve o nível de poder de um item ou criatura (ex: `poder:alto`, `poder:baixo`).
*   **`papel`**: Descreve a função de uma criatura (ex: `papel:curandeiro`, `papel:artesao`).
*   **`cultura`**: Associa uma entidade a uma cultura específica (ex: `cultura:povo-da-vila`).
*   **`bioma`**: Para locais, descreve o ambiente ecológico (ex: `bioma:floresta`, `bioma:deserto`).
*   **`populacao`**: Para locais, dá uma ideia do número de habitantes (ex: `populacao:pequena`, `populacao:grande`).
*   **`alinhamento`**: Descreve a inclinação moral ou ética de uma criatura ou facção (ex: `alinhamento:misterioso`).

