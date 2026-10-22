---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece na sua máquina e em tempo real, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições de arquivos à medida que ocorrem. Você pode revisar cada diff em linha, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts complementares para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade;
- revisar e aceitar as edições propostas nos arquivos;
- orientar o agente durante a sessão para adicionar um controle do modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design fez uma solicitação adicional: também gostaria de um controle do modo claro para usuários que consideram o tema padrão escuro demais. Em vez de iniciar uma conversa nova, você orientará a sessão atual para ampliar o trabalho.

## Acompanhar o progresso do Copilot

A exibição Copilot Chat mostra cada etapa executada pelo agente: os arquivos que ele explora, os comandos que executa e as edições que propõe. Cada edição aparece em linha no chat, com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat usada no Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar as chamadas de ferramentas. Você verá leituras e edições de arquivos e os comandos de terminal que o agente solicita executar.
3. Quando o agente propuser uma edição, o diff aparecerá em linha:
   - selecione **Keep** para aceitar a alteração;
   - selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed**, na parte superior do chat, para ver todos os arquivos alterados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão, por exemplo, para executar um comando de terminal, o agente pausará e exibirá a solicitação **Allow**. Aprove somente os comandos que você considerar seguros.

5. Aguarde o agente concluir a implementação do modo de alto contraste. Você deverá ver uma mensagem final resumindo as alterações.
## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar um controle do modo claro. Como continuará na mesma sessão de chat, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilos e o padrão de armazenamento local usado.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt complementar:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente retomará o trabalho, explorando as alterações feitas e propondo atualizações para o modo claro.
3. Revise cada nova edição à medida que ela aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface dos controles de uma forma que você não quer —, pare-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem complementar para esclarecer o pedido.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar do zero. Envie prompts complementares para solicitar ajustes, como "simplifique o CSS — use variáveis CSS em vez de duplicar seletores"; corrigir o rumo, como "não adicione um terceiro controle; integre o modo claro ao painel de configurações existente"; ou ampliar o escopo, como "adicione também atalhos de teclado para os dois controles".
## Revisar o conjunto de alterações

Antes de fazer commit do trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações em componentes Astro, estilos e testes relacionados.
3. Abra alguns dos arquivos alterados e examine os diffs. Confirme se a implementação segue os padrões de acessibilidade do agente personalizado, incluindo atributos de Accessible Rich Internet Applications (ARIA), navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações pelo painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou as edições à medida que eram feitas, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como acompanhar o trabalho do agente em tempo real;
- como aceitar e rejeitar edições individuais em arquivos;
- como orientar uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações feitas pelo agente personalizado local durante este laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
