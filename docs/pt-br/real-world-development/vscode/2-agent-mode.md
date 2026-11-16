---
title: "Exercício 2 - Adicionar novas funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo as atualizações mais simples de um aplicativo normalmente exigem alterações em vários arquivos e operações como executar testes. Como desenvolvedor, seu fluxo de trabalho geralmente envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou faltando, fazer outra atualização... E por aí vai.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi desenvolvido para atuar com mais autonomia no IDE. Ele se comporta de maneira semelhante a um desenvolvedor: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os erros encontrados. Vamos explorar como você pode usar o modo de agente para adicionar novas funcionalidades ao seu site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente na nuvem foram criados para experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente na nuvem funciona como um programador parceiro, trabalhando de forma assíncrona como membro da equipe e interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar seu projeto, identificar arquivos relevantes e coordenar alterações;
- o modo de agente do GitHub Copilot pode implementar novos recursos na interface do usuário e na camada de dados;
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e editora. Você já adicionou um auxiliar para editoras no exercício anterior e agora concluirá o restante do trabalho na camada de dados, na interface do usuário e nos testes com o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer qualquer alteração, vamos explorar o site Tailspin Toys para entender suas funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com um tema voltado a desenvolvedores. Ele permite que os usuários listem jogos e vejam seus detalhes. O site é um único aplicativo Astro que renderiza suas páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM — não há uma API de backend separada nem uma estrutura de interface no lado do cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e qualquer interatividade é adicionada com um pequeno `<script>` do Astro, com escopo limitado e usando as APIs DOM padrão.

### Iniciar o site

Para facilitar a execução do site, foi disponibilizada uma tarefa `npm run dev`, que inicia o único servidor de desenvolvimento Astro. Você pode executá-la no GitHub Codespace seguindo estas etapas:

1. Volte ao codespace. Você deve estar na branch `main`, após o exercício anterior.
2. Abra uma nova janela de terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma nova branch para este trabalho de filtragem e mude para ela:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um banner indicando o URL, semelhante ao exemplo abaixo:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo for aberta solicitando que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Ao usar um codespace, selecionar um link para o URL localhost no terminal do Codespace redirecionará você automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para seu codespace, que agora hospeda o servidor Web!

### Explorar o site

Com o site em execução, você pode explorar suas funcionalidades. Os principais recursos do site incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você será direcionado a uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **Novo chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam por exercício — cada etapa indicará o que importa.

A implementação inicial do site é funcional, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do modelo, um backlog de issues foi criado automaticamente — peça ao GitHub Copilot para mostrar esses itens.

1. Na lista suspensa de agentes da visualização Chat, selecione **Agente**. O modo **Agente** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos de terminal e invoca ferramentas.

   ![Captura de tela mostrando o seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão indicando preferência por um modelo específico. Ao criar este workshop, usamos o Claude Sonnet 4.5 e, por isso, o incluímos nas instruções. Esperamos que as sugestões de código recebidas sejam relativamente consistentes e proporcionem uma boa experiência. No entanto, como os LLMs são probabilísticos, talvez as sugestões recebidas sejam diferentes das indicadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot informações sobre o backlog de issues enviando o seguinte prompt:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude-me a priorizá-los com base nos que serão mais úteis para o usuário.
   ```

4. Selecione **Continuar** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe que o Copilot também priorizou os itens para você, com base no que considera mais útil para o usuário.

## Revisar os arquivos de instruções

Antes de iniciar o agente para gerar o código, é uma boa hora para revisar o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como abordar a adição de funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais sobre como adicionar funcionalidades. Elas incluem:
   - uma visão geral da arquitetura;
   - princípios para design de componentes, testabilidade e acessibilidade;
   - links para arquivos de instruções específicos de vários tipos de arquivo, incluindo:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Os arquivos de instruções permitem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso permite reutilizar instruções, dividir conjuntos grandes em arquivos menores e mais gerenciáveis e fornecer exemplos e modelos ao Copilot.

## Implementar a funcionalidade de filtragem

Para concluir a filtragem, será necessário fazer pelo menos três atualizações distintas no aplicativo:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`);
- adicionar ou atualizar testes do comportamento de filtragem;
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de mesclar tudo à base de código. O modo de agente do Copilot pode fazer essas tarefas por você! Vamos adicionar essa funcionalidade.

1. Você pode continuar na conversa atual com o Copilot ou selecionar **Novo chat** para iniciar uma nova.
2. Selecione **Adicionar contexto**, **Instruções** e **ui** como arquivo de instruções.

   ![Captura de tela mostrando um exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agente** continua selecionado na lista suspensa de agentes da visualização Chat.

   ![Captura de tela mostrando o seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot para implementar a funcionalidade com base na issue correspondente do backlog, usando o seguinte prompt:

   ```plaintext
   Atualize o site para incluir filtros por editora e categoria com base nos requisitos da issue relacionada no backlog do GitHub. Já existe um auxiliar para editoras do exercício anterior; portanto, preserve e refine esse trabalho conforme necessário enquanto conclui o restante da camada de dados, da interface e dos testes. Confirme se todos os testes foram aprovados antes de concluir. O servidor já está em execução, então não é necessário iniciá-lo.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade desejada. Você deverá vê-lo localizar os auxiliares da camada de dados, a interface e os testes. Em seguida, ele começará a modificar os arquivos e executar os testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Você verá o Copilot realizar várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os botões **Manter** e **Desfazer** no editor de código. Quando o Copilot terminar, você verá **Manter** ou **Desfazer** para todas as alterações, portanto, não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continuar** para executar os testes.

   ![Captura de tela mostrando uma caixa de diálogo no painel do Copilot Chat solicitando a confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram algumas pausas e até falhas em alguns testes durante o processo. Não tem problema! O Copilot alterna entre a geração de código e os testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do Chat com o modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador com o site em execução. Explore a nova funcionalidade!
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Manter** na janela do Copilot Chat.

## Resumo e próximas etapas

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot pode implementar novos recursos na interface do usuário e na camada de dados;
- o modo de agente do Copilot pode explorar seu projeto, identificar arquivos relevantes e coordenar alterações;
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

Agora vamos [testar seu recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício bônus de exploração – Implementar paginação

À medida que a lista de jogos cresce, será necessário habilitar a paginação. Usando as habilidades que você aprendeu neste exercício, peça ao Copilot para atualizar o site e implementar a paginação. Considere o seguinte:

- siga as práticas recomendadas existentes, inclusive usando os arquivos de instruções atuais;
- pense em como implementar a paginação: se você permitirá que o usuário selecione o tamanho da página ou se ele será fixo;
- ao criar o prompt, forneça ao Copilot as orientações necessárias para implementar o que você deseja;
- talvez seja necessário iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot!

## Recursos

- [Modos ask, edit e agente do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
