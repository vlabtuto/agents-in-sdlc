---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem usando o modo de agente. Antes de abrir um pull request, confirme se ele realmente funciona no navegador. Em vez de navegar pelo aplicativo manualmente, você conectará o **servidor MCP do Playwright** e permitirá que o Copilot controle um navegador real para testar o recurso — depois, publicará sua branch e abrirá o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot com novas ferramentas;
- adicionar o servidor MCP do Playwright ao seu espaço de trabalho;
- pedir ao Copilot que use o servidor para testar manualmente seu recurso de filtragem em um navegador;
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando pode ir além do editor. O Model Context Protocol (MCP) é como o Copilot faz isso — um padrão que permite ao agente conversar com ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo de agente e como ele interage com contexto, LLM e ferramentas — incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) oferece aos agentes de IA uma maneira de se comunicar com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Isso permite que eles acessem informações atualizadas (usando recursos) e realizem ações em seu nome (usando ferramentas).

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e os serviços externos. O servidor MCP é responsável por gerenciar a comunicação entre o agente de IA e as ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes NPM. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP bastante conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a um conjunto de APIs para gerenciar repositórios do GitHub. Ele permite que o agente de IA execute ações como criar novos repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador usando o Playwright. Ele permite que o agente de IA execute ações como acessar páginas Web, preencher formulários e selecionar botões.

Há muitos outros servidores MCP disponíveis que fornecem acesso a diferentes ferramentas e recursos. O GitHub hospeda um [registro MCP](https://github.com/mcp) para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do seu projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use apenas servidores MCP em que você confia e tenha cautela ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` já vem com o modelo do projeto, por isso o Copilot pôde ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], portanto não há nada para instalar localmente — o VS Code se conecta a ele por HTTP, e você se autentica no GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicionar o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, exatamente o que você precisa para testar seu recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github` para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia no seu computador executando `npx @playwright/mcp@latest`. A opção `--headless` instrui o Playwright a executar o navegador sem uma janela visível, o que é necessário dentro de um codespace, onde não há uma área de trabalho para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar mais tarde que falta um navegador, peça para executar `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda** — na primeira vez que o Copilot precisar das ferramentas de um servidor, o VS Code o iniciará e solicitará que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Iniciar** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar se a configuração está correta.
2. Se o VS Code pedir que você confirme se confia no servidor, revise a configuração e escolha confiar nele para que o servidor possa iniciar.

Iniciá-lo manualmente é opcional — se você pular essa etapa, o VS Code iniciará o servidor (e pedirá que você confirme se confia nele) na próxima etapa, quando o Copilot precisar das ferramentas.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real para controlar. Em vez de navegar pelo aplicativo para verificar seu trabalho, o agente pode abrir uma página, navegar, aplicar filtros e apresentar os resultados — depois, pode resumir o que observou. É a maneira mais rápida de confirmar se um recurso funciona como esperado sem sair da conversa.

Internamente, o servidor MCP do Playwright trabalha com a [árvore de acessibilidade da página][playwright-mcp-server], não com capturas de tela. Isso significa que o agente analisa elementos estruturados e rotulados (botões, links, itens de lista), como a tecnologia assistiva faz — assim, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar o recurso de filtragem que você acabou de criar:

```text
Usando o servidor MCP do Playwright, abra um navegador no aplicativo em execução em http://localhost:4321 e verifique o novo recurso de filtragem de jogos:

1. Acesse a página de jogos e anote quantos jogos estão listados.
2. Aplique um filtro de categoria e confirme se a lista é atualizada para mostrar apenas jogos dessa categoria.
3. Limpe o filtro e aplique um filtro de editora. Confirme se a lista é atualizada para mostrar apenas jogos dessa editora.
4. Combine um filtro de categoria e um de editora e confirme se os resultados correspondem aos dois filtros.

Relate o que você observou em cada etapa e destaque qualquer comportamento inesperado.
```

O Copilot abrirá um navegador por meio do servidor MCP do Playwright, seguirá cada etapa e informará o que encontrou. Compare o resumo com os critérios de aceitação da issue — se algo parecer errado, faça perguntas complementares ou peça ao Copilot para corrigir o código antes de abrir um pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para este teste. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador — se ele informar que falta um navegador, peça para executar `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Agora que você confirmou que o recurso funciona, pode abrir um pull request (PR) para que sua equipe revise o trabalho. A primeira etapa é publicar a branch `filtering-vscode`.

1. Acesse o painel **Controle do Código-Fonte** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de confirmação usando o botão **Brilho**.

   ![Captura de tela do painel Controle do Código-Fonte mostrando as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publicar** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando do GitHub (CLI). Mas, como você já está trabalhando com o GitHub Copilot, vamos deixar que ele crie o PR! Ele pode localizar a issue relevante e criar um PR associado a ela.

1. Acesse o painel Copilot Chat e selecione **Novo chat** para iniciar uma nova sessão.
2. Confirme se **Agente** está selecionado na lista suspensa de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot para criar um PR:

   ```plaintext
   Encontre no repositório a issue relacionada à filtragem por categoria e editora. Crie um novo pull request para a branch atual e associe-o à issue correta.
   ```

4. Quando necessário, selecione **Continuar** para permitir que o Copilot execute as tarefas necessárias para coletar informações e realizar operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar-se no GitHub — siga as instruções para permitir o acesso.
5. Observe o Copilot pesquisar as issues, localizar a correta e criar o PR.
6. Selecione o link gerado pelo Copilot para revisar seu pull request, mas **não o mescle ainda**.

## Resumo e próximas etapas

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot com novas ferramentas;
- adicionou o servidor MCP do Playwright ao seu espaço de trabalho;
- usou o servidor para testar manualmente seu recurso de filtragem em um navegador antes de publicá-lo;
- publicou sua branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso — mas o MCP é apenas uma das maneiras de ampliar os recursos do Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
