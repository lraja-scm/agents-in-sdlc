---
title: "Lição 5 - Personalizar e usar uma skill de verificações de qualidade"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar não é apenas escrever código. Já validamos manualmente o funcionamento do código e usamos arquivos de instruções para garantir que ele siga nossos padrões. Mas e os testes, o lint e os outros aspectos da integração contínua (CI)?

Para esse tipo de tarefa, **skills de agente** são ideais. Elas ajudam o Copilot a entender como executar operações como essas.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

O Tailspin Toys usa a skill `quality-checks` para executar testes de unidade, lint e verificações de tipos. A equipe quer aprimorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis para tarefas, scripts executáveis e recursos de suporte, que o agente carrega sob demanda. Uma skill consiste, em sua forma mais simples, em uma pasta com o nome da skill e um arquivo Markdown chamado `SKILL.md`. O arquivo Markdown contém frontmatter com um nome e uma descrição da skill, além de uma visão geral do que ela faz e orientações sobre quando deve ser usada. A pasta também pode conter subpastas com scripts e outros recursos.

> [!NOTE]
> Uma skill não precisa incluir arquivos ou pastas adicionais. A skill `quality-checks` do Tailspin Toys contém apenas `SKILL.md`, pois usa os comandos já existentes no projeto.

As skills podem ficar na pasta `.github/skills` do projeto para que a equipe as compartilhe e reutilize como recursos do repositório, ou na pasta de skills do usuário em `~/.copilot/skills`.

## Explorar a skill

Explore a skill `quality-checks` criada pela equipe do Tailspin Toys para executar testes de unidade, lint e verificações de tipos.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description` no início do arquivo. A descrição ajuda o Copilot a decidir quando usar a skill.
3. Leia as instruções e observe como elas orientam o Copilot no processo de teste e lint.
4. Observe que a skill ainda não tem uma seção **Results output formatting**.

## Executar a skill antes de fazer uma mudança

As skills podem ser chamadas diretamente pelo Copilot CLI ou por meio de linguagem natural. Peça ao Copilot que execute as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Use o seguinte prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório apresentado ao final.

## Personalizar o relatório

O relatório deve informar o que foi executado, se houve sucesso e quais foram os resultados das ferramentas. Atualize a skill para gerar esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Executar a skill atualizada

Agora que a skill foi atualizada, veja o resultado. O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes de unidade, lint e verificações de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa mudança acompanhará a filtragem no PR do recurso. Em seguida, permita que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
