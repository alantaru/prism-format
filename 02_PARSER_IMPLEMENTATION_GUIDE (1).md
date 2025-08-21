# Guia para Implementação de Parser .prism (Auditado pelo Protocolo Cerberus)

Este documento fornece diretrizes **obrigatórias** para desenvolvedores que implementam um parser para o formato `.prism` v3.1. Falhar em seguir estas diretrizes resultará em uma implementação não-conforme e propensa à corrupção de dados.

## 1. Paradigma de Parsing: Árvore de Sintaxe Abstrata (AST)

A única abordagem aceitável para parsear o formato `.prism` é através da construção de uma **Árvore de Sintaxe Abstrata (AST)**. Uma AST representa a estrutura hierárquica do documento em memória.

**ABORDAGENS INACEITÁVEIS:**
*   **Expressões Regulares Simples:** O uso de regex para extrair conteúdo de forma "gananciosa" é **expressamente proibido**. Esta abordagem falhará catastroficamente ao encontrar Diretivas aninhadas e é a causa raiz de bugs de corrupção de dados.
*   **Parsing Linha por Linha Simples:** Um parser que apenas processa o arquivo linha por linha sem entender a estrutura de blocos e a hierarquia do conteúdo é insuficiente.

## 2. Lógica de Parsing Mandatória

O parser deve operar como uma máquina de estados que constrói uma AST.

1.  **Divisão de Blocos de Aspecto:** O primeiro passo é dividir o arquivo pelos delimitadores de aspecto (`---`).
2.  **Identificação de Aspectos:** Para cada bloco, identifique o tipo de aspecto (`@@systemic`, `@@textual`, etc.).
3.  **Parsing de Aspectos `Chave:Valor` (`@@systemic`):** Estes podem ser parseados de forma simples, extraindo os pares `chave: valor`.
4.  **Parsing de Aspectos de Conteúdo (`@@textual`):** Este é o passo crítico.
    *   O conteúdo do aspecto textual DEVE ser processado por uma função recursiva que constrói a AST.
    *   **Tratamento de Capítulos:** A função deve primeiro dividir o conteúdo usando `@@chapter:` como delimitador. O texto antes da primeira ocorrência é o nó do capítulo "Descrição". Cada ocorrência subsequente cria um novo nó de capítulo.
    *   **Tratamento de Diretivas:** Dentro de cada capítulo, a função deve procurar por **Diretivas Prismma** `(> @tag: ... <)`.
    *   Ao encontrar o início de uma diretiva `(> @`, a função deve escanear o texto para encontrar o final correspondente `<)`, **respeitando o aninhamento**. Isso requer um contador de profundidade.
    *   O conteúdo *dentro* da diretiva encontrada deve ser passado de volta para a mesma função de parsing recursivamente, criando nós filhos na AST.
    *   O texto que não faz parte de uma diretiva é tratado como nós de parágrafo/texto simples.

## 3. Estrutura da AST de Saída (Exemplo)

Para um `.prism` textual, a AST resultante deve ter uma estrutura similar a esta:

**Entrada `.prism`:**
```prism
@@textual
Isso é um texto de descrição.
(> @shaper_note: Dica principal. (> @lore_suggestion: Dica aninhada. <) <)
@@chapter: Capítulo 2
Outro texto.
```

**Saída AST (Conceitual):**
```json
{
  "type": "root",
  "content": [
    {
      "type": "chapter",
      "meta": { "title": "Descrição" },
      "content": [
        { "type": "paragraph", "content": "Isso é um texto de descrição." },
        {
          "type": "directive",
          "meta": { "tag": "shaper_note" },
          "content": [
            { "type": "text", "content": "Dica principal." },
            {
              "type": "directive",
              "meta": { "tag": "lore_suggestion" },
              "content": [
                { "type": "text", "content": "Dica aninhada." }
              ]
            }
          ]
        }
      ]
    },
    {
      "type": "chapter",
      "meta": { "title": "Capítulo 2" },
      "content": [
        { "type": "paragraph", "content": "Outro texto." }
      ]
    }
  ]
}
```

## 4. Validação e Erros

O parser deve ser **implacavelmente estrito**.
*   **Obrigatoriedades:** Deve falhar com erros claros para qualquer desvio da especificação.
*   **Diretivas Não Fechadas:** Se uma diretiva for aberta com `(>` mas não fechada com `<)`, o parser DEVE lançar um erro de sintaxe.

A implementação de referência para esta lógica deve residir em `src/lib/prism-parser.ts`. Qualquer outra parte da aplicação que precise entender o conteúdo de um `.prism` DEVE usar este parser canônico.
