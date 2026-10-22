---
title: "Lição 10 - Encerramento e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo de trabalho no Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e localizou a issue de filtragem criada para o workshop.
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e mesclou manualmente seu primeiro pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou no modo Autopilot e revisou no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e a usou para executar testes de unidade, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e o usou para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou a mudança completa de filtragem e autorizou o Agent Merge para o PR correspondente.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## O que você entregou

O workshop inclui dois marcos de PR, cada um em sua própria branch criada a partir de `main` atualizado:

1. **Avaliações por estrelas:** exiba o valor existente de `starRating` e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de qualidade:** implemente a filtragem, atualize as instruções e aplique-as ao recurso, personalize o relatório de `quality-checks`, crie um perfil de QA e inclua os testes correspondentes.

Do planejamento da filtragem até a abertura do PR, você usou a mesma conversa e branch. Reunimos esse trabalho em um PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: com testes automatizados, sua própria inspeção no navegador e a exploração do navegador pelo Copilot por meio do MCP. A skill `quality-checks` executou testes de unidade, lint e verificações de tipos e apresentou os resultados no novo formato. Antes da abertura do PR, o QA reuniu esses resultados com a revisão dos requisitos e da cobertura de testes.

Os testes adicionados devem fechar lacunas reais; uma execução de QA que não exija novos testes também pode estar correta. Revise o código e as evidências antes de autorizar a mesclagem e atualize as evidências afetadas depois de fazer mudanças.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot moldam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e adapte-as às necessidades da equipe. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as mudanças e os resultados reais das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** adequados para a tarefa. Use **Plan** para definir uma abordagem antes de começar, **Interactive** para acompanhar mudanças pontuais e **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e um modelo mais avançado para trabalhos complexos.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever claramente *o que* você quer desenvolver, *por que* e *como* influencia significativamente o resultado.

## Mais possibilidades

Você concluiu o fluxo de trabalho principal. Vale conhecer mais alguns recursos do CLI:

- `/review` para pedir ao agente de revisão de código que analise as mudanças.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar subtarefas independentes em paralelo.
- `/worktree` para isolar outra tarefa.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor forma de aprimorar o uso de qualquer ferramenta é continuar praticando. Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você pensa em desenvolver há anos. Compartilhe o que aprendeu com a equipe e aprenda com as experiências de outras pessoas. E, como sempre, explore a documentação.

Se quiser ampliar o Tailspin Toys pelo terminal, continue com a [série opcional Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
