---
title: "Exercício 2 - Agente na nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que pouquíssimas organizações — se houver alguma — não enfrentem problemas com dívida técnica. Isso pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram paradas no backlog por falta de tempo para implementá-las. O agente na nuvem do GitHub Copilot foi criado para realizar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um PR em rascunho para revisão por um desenvolvedor. Isso permite delegar tarefas tediosas, acelerar o desenvolvimento e liberar os desenvolvedores para se concentrarem em questões mais importantes.

Você explorará os seguintes aspectos do agente na nuvem do Copilot:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de issues com escopo bem definido;
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. Os prestadores de serviço inicialmente contratados para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está completamente ausente. Como primeiro passo, a equipe gostaria de adicionar comentários de documentação TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo exibisse jogos relacionados — outros títulos da mesma categoria — para que as pessoas pudessem continuar explorando. Por enquanto, não é necessária uma implementação refinada; basta algo que a equipe possa usar para testes de aceitação da experiência do usuário. Isso está bloqueando o trabalho, mas há outras issues de prioridade mais alta no momento.

Esses são dois exemplos de tarefas que podem ser facilmente rebaixadas na lista de prioridades e que são ótimas candidatas para atribuir ao agente na nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona, permitindo que o desenvolvedor se concentre em outras tarefas e depois volte para revisar o trabalho do Copilot e confirmar se está tudo como esperado.
## Apresentação do agente na nuvem do GitHub Copilot

O [agente na nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode realizar tarefas em segundo plano, de forma semelhante a um desenvolvedor humano. Assim como no trabalho com uma pessoa, há várias maneiras de fazer isso, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que a issue é atribuída, o Copilot cria um pull request em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar na tarefa. Você pode acompanhar a sessão do Copilot enquanto ela está em andamento ou depois de concluída. Quando a solução proposta estiver pronta para revisão, o Copilot marcará você no pull request.
## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot — não é possível deixar de pensar no problema. Mesmo operações aparentemente simples se tornam complexas quando analisadas em detalhes. Por isso, [defina cuidadosamente o escopo das tarefas para o agente na nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente na nuvem do Copilot, examine, sem fazer alterações, os arquivos de instruções já incluídos neste repositório para que você possa perceber seus efeitos mais tarde.

Abra os arquivos a seguir na interface Web do GitHub para seu repositório ou em um codespace, caso já tenha um em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas para convenções de TypeScript e comentários de documentação TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica essas instruções a arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao agente na nuvem na próxima seção, observe se o pull request resultante contém comentários de documentação TSDoc, segue as convenções de TypeScript e inclui cabeçalhos nos arquivos. Essas características vêm desses arquivos de instruções. Voltaremos a isso ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente na nuvem do Copilot

Criar código, independentemente de quem esteja envolvido, normalmente exige um ambiente específico e a execução de alguns scripts de configuração para garantir que tudo esteja em ordem. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O agente na nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow especial de configuração][setup-workflow], definido no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar a trabalhar. Assim, ele terá acesso às ferramentas de desenvolvimento e dependências necessárias. Esse arquivo já foi configurado para facilitar o andamento do laboratório e permitir que você aprenda com essa experiência. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários do navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no menor nível necessário para *suas etapas*. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Se quiser clonar o repositório como parte das etapas de configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que os builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se o workflow é executado corretamente sem ter que esperar o Copilot iniciá-lo.

## Adicionar documentação

Embora todos entendam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação alguma. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novos desenvolvedores à equipe. Felizmente, o Copilot é muito bom em criar documentação, e esta é uma tarefa perfeita para atribuir ao agente na nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar a issue.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

    ```plaintext
    Nossa organização exige que funções e métodos incluam comentários de documentação TSDoc quando forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para garantir a inclusão de comentários de documentação quando eles esclarecerem o comportamento.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se um novo PR ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, você verá que o Copilot criou uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está criando o ambiente (executando o workflow mencionado anteriormente), analisando o projeto e determinando a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request para baixo. Você verá uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot está resolvendo o problema. Isso inclui explorar o código e entender seu estado, pausar para pensar e decidir o plano adequado e também criar código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente na nuvem do Copilot é permitir que ele execute tarefas de forma assíncrona, liberando você para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente na nuvem e, em seguida, voltando nossa atenção para escrever código para adicionar recursos ao aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes do jogo

Como já destacamos, uma das grandes vantagens do agente na nuvem do GitHub Copilot é a possibilidade de dividir o trabalho. Enquanto ele se concentra em um conjunto de tarefas, você pode trabalhar em outro. Talvez a seção de jogos relacionados solicitada pela equipe de design não leve muito tempo para ser implementada, mas você ainda poderia usar esse tempo em outras tarefas. Vamos atribuí-la ao agente na nuvem do Copilot!

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos exibindo jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário e fazer testes de aceitação. Estes são os requisitos:

   - Adicione um helper de acesso a dados em `src/lib/` que retorne outros jogos da mesma categoria de um determinado jogo, excluindo o próprio jogo.
   - Exiba uma seção "Jogos relacionados" na página de detalhes do jogo que use o helper.
   - Trate o caso em que um jogo não tenha jogos relacionados.
   - Crie testes de unidade para o novo helper.
   - Antes de criar o PR, garanta que todos os testes sejam aprovados.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá um conjunto de 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando diligentemente na sua segunda solicitação. O agente na nuvem do Copilot trabalha de maneira semelhante a um engenheiro de software, então não é necessário monitorá-lo ativamente. Basta revisar o trabalho quando ele estiver concluído. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximas etapas

Nesta lição, você explorou o [agente na nuvem do GitHub Copilot][copilot-agents]. Com o agente na nuvem, você pode atribuir issues ao Copilot para execução assíncrona. Você pode usá-lo para resolver dívidas técnicas, criar recursos ou ajudar a migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de issues com escopo bem definido;
- atribuir issues ao Copilot.

Com o agente na nuvem trabalhando em segundo plano, agora podemos nos concentrar em criar e usar agentes personalizados. O [agente na nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso às instruções personalizadas que exploramos nos módulos anteriores.

## Recursos

- [Sobre o agente na nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente na nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
