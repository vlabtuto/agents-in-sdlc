---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo de agente, ele trabalha de forma autônoma — explora sua base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real no seu computador, o Copilot Chat no VS Code mostra todas as chamadas de ferramentas e edições de arquivos à medida que ocorrem. Você pode revisar cada diff no próprio editor, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts complementares para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade;
- revisar e aceitar as edições de arquivos propostas;
- orientar o agente durante a sessão para adicionar um seletor de modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade que você usou no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design voltou com uma solicitação adicional — também gostaria de um seletor de modo claro para usuários que consideram o tema padrão escuro demais. Em vez de começar uma nova conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Monitorar o progresso do Copilot

A visualização Copilot Chat mostra cada etapa executada pelo agente — os arquivos que explora, os comandos que executa e as edições que propõe. Cada edição de arquivo aparece na conversa com um diff e os controles **Manter** e **Desfazer**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e os comandos de terminal cuja execução ele solicitar.
3. Quando o agente propuser uma edição de arquivo, o diff aparecerá na conversa:
    - Selecione **Manter** para aceitar a alteração.
    - Selecione **Desfazer** para reverter a alteração e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Arquivos alterados** na parte superior do chat para ver todo o conjunto de arquivos que o agente modificou nesta sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão (por exemplo, para executar um comando de terminal), o agente pausará e exibirá uma solicitação **Permitir**. Aprove apenas os comandos que você considera seguros.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Você verá uma mensagem final que resume as alterações realizadas.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, você ampliará a mesma conversa para adicionar um seletor de modo claro. Como você continuará na mesma sessão de chat, o agente manterá todo o contexto do trabalho anterior — incluindo os componentes que alterou, a abordagem de estilo usada e o padrão de armazenamento local que configurou.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt complementar:

    ```plaintext
    Bom trabalho! Agora, adicione um seletor de modo claro junto com o seletor de alto contraste. O modo claro deve seguir o mesmo padrão — persistir no armazenamento local e estar disponível na mesma área da interface. O usuário deve poder ativar o modo de alto contraste e o modo claro de forma independente.
    ```

2. O agente continuará o trabalho, explorando as alterações já feitas e propondo adições para o modo claro.
3. Revise cada nova edição assim que aparecer, como fez com o modo de alto contraste. Use **Manter** ou **Desfazer** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface dos seletores de uma maneira que você não quer —, você poderá interrompê-lo com o botão **Parar** ao lado do campo de entrada do chat e enviar uma mensagem complementar para esclarecer o pedido.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro seletor; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione atalhos de teclado para os dois seletores").

## Revisar o conjunto de alterações

Antes de confirmar o trabalho, faça uma revisão rápida de tudo o que o agente alterou.

1. Abra a visualização **Controle do Código-Fonte** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em testes relacionados.
3. Abra alguns dos arquivos alterados e examine os diffs. Confirme se os padrões de acessibilidade do agente personalizado estão presentes — atributos ARIA, navegação por teclado, HTML semântico e persistência usando o armazenamento local.
4. Quando estiver satisfeito, prepare e confirme as alterações no painel Controle do Código-Fonte. Você as publicará [na próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento — revisando cada edição de arquivo à medida que era aplicada, aceitando ou rejeitando alterações e enviando prompts complementares para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como monitorar o trabalho de um agente em tempo real;
- como aceitar e rejeitar edições individuais de arquivos;
- como orientar uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará seu trabalho de acessibilidade como um pull request e revisará as alterações que o agente personalizado local produziu durante este laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
