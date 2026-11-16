---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que adicionasse documentação ao código, criasse um recurso de jogos relacionados para a equipe de design iterar e implementasse recursos de acessibilidade, incluindo controles para os modos de alto contraste e claro. Vamos explorar as alterações no código e, se necessário, enviar comentários ao Copilot para melhorar o trabalho.

### Cenário

Como já destacamos várias vezes, os fundamentos do design de software e do DevOps não mudam com a chegada da IA generativa. Sempre queremos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.
## Segurança e agente na nuvem do GitHub Copilot

Como o agente na nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, foram impostas algumas restrições de segurança para garantir que tudo permaneça seguro. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente na nuvem é executado no GitHub Actions, onde cria um ambiente separado e efêmero para trabalhar.
- Os workflows do GitHub Actions precisam da aprovação de uma pessoa antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.
## Revisar a documentação gerada

Vamos começar explorando o primeiro pull request (PR) gerado pelo agente na nuvem do GitHub Copilot: o PR que adiciona documentação ao código. Você fará isso usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao explorar o PR, talvez você veja um aviso informando que o GitHub Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o acesso do Copilot a recursos externos é limitado por padrão, inclusive a chamadas para servidores MCP externos. Se quiser, você pode [personalizar ou desativar o firewall do agente na nuvem do Copilot][agent-firewall].

1. Volte ao seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título parecido com **Add missing documentation** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde até que o Copilot conclua o trabalho. Isso pode levar alguns minutos. Enquanto espera, você pode fazer uma pausa ou refletir sobre tudo o que aprendeu até agora.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os comentários de documentação TSDoc recém-criados e outras documentações. As alterações exatas variam.

    Ao examinar as alterações, procure comentários de documentação TSDoc, convenções de TypeScript e cabeçalhos de comentários. Eles vêm dos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que está tudo certo, volte à guia **Conversation** e role a página para baixo.
7. Você verá um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Você verá os workflows serem adicionados à fila na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode marcar o Copilot em comentários — assim como faria com outros membros da equipe — no pull request ou em comentários em linha no código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não podemos fornecer um texto prescritivo para solicitar alterações. Veja algumas ideias:

- Adicionar cabeçalhos de comentários ao início de cada arquivo de código com uma breve descrição do que ele faz.
- Adicionar comentários de documentação TSDoc a arquivos TypeScript e Astro.
- Criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e marque **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão de documentação que você gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Depois que o Copilot concluir as alterações, você verá um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Você pode continuar iterando até ficar satisfeito. Quando estiver tudo pronto, converta o PR de rascunho em pronto para revisão e faça o merge na branch principal.

![Converter o PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Vamos voltar ao PR que o Copilot gerou para resolver a issue sobre exibir jogos relacionados na página de detalhes do jogo.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título parecido com **Show related games on the game details page** ou outro título mais específico.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que está tudo certo, volte à guia **Conversation** e role a página para baixo.
6. Você verá um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Você verá os workflows serem adicionados à fila na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você também pode alternar para essa branch no seu Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute os seguintes comandos (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode trabalhar de forma iterativa com o agente na nuvem do Copilot para solicitar atualizações. Por exemplo, você pode pedir ao Copilot que ajuste a quantidade de jogos relacionados exibidos ou garanta a inclusão de cabeçalhos de comentários e comentários de documentação TSDoc (lembre-se: essa tarefa foi atribuída **antes** de você atualizar as instruções personalizadas). Assim como antes, você pode fazer essas solicitações adicionando um novo comentário na guia **Conversation**. O Copilot verá o comentário e iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados usando o agente personalizado de acessibilidade. Este PR deve incluir tanto o modo de alto contraste atribuído no Exercício 3 quanto o modo claro solicitado na central de controle no Exercício 4.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título parecido com **Add high contrast mode to website** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde até que o Copilot conclua o trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - os componentes de interface para alternar entre os modos;
   - o uso do armazenamento local para manter as preferências do usuário;
   - as alterações de CSS ou de estilo nos modos de alto contraste e claro;
   - os atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.);
   - o código JavaScript/TypeScript que gerencia a alternância entre os modos.

6. Depois de revisar as atualizações e confirmar que está tudo certo, volte à guia **Conversation** e role a página para baixo.
7. Você verá um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Você verá os workflows serem adicionados à fila na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro único serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode alternar para essa branch no seu Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os seguintes comandos (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador para garantir que funcionem como esperado e mantenham as configurações após recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot para implementar esses recursos seguindo as práticas recomendadas de acessibilidade. Se notar algum problema ou oportunidade de melhoria na acessibilidade, você pode marcar **@copilot** em um comentário para solicitar atualizações, assim como fez nos PRs anteriores.

## Exercício opcional — continuar delegando

O agente na nuvem é mais útil quando você pode delegar itens reais do backlog e se concentrar em outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no seu repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário para manifestar interesse de apoiadores na página de detalhes do jogo.
- Implementar paginação na página de lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o ambiente de trabalho do agente na nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas incluídos neste repositório** para ver como eles influenciam o trabalho do agente na nuvem.
- **Atribuiu issues ao agente na nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona das tarefas.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando controles para os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como central de controle** para monitorar e orientar a sessão de acessibilidade em andamento.
- **Revisou e iterou nos pull requests do agente na nuvem**, marcando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximas etapas

Você concluiu o ambiente de trabalho do agente na nuvem. Se quiser continuar explorando, os outros ambientes complementam o que você praticou aqui:

- 🖥️ **[Ambiente do VS Code](../../vscode/)** — explore o modo agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Ambiente da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal usando os modos Plan e Autopilot, habilidades de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` de volta ao agente na nuvem.

No seu próprio repositório, experimente estas ideias:

- Atribua ao agente na nuvem uma issue de refatoração ou de cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente na nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente na nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
