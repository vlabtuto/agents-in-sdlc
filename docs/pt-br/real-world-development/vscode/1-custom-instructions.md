---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se há informações de contexto que o Copilot deve conhecer —, você precisa garantir que esse contexto esteja acessível. Há várias maneiras de compartilhar informações específicas com o Copilot. Entre as principais estão os [arquivos de instruções][instruction-files], usados para fornecer orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções aplicados a caminhos específicos;
- gerar o primeiro conjunto de dados para filtragem (um auxiliar para editoras) com as instruções atuais;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- executar novamente o mesmo prompt e observar o código gerado adotar o novo padrão;
- confirmar as alterações nas instruções e no conjunto de filtragem na branch `main` para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões que você definiu. O Copilot é não determinístico. O objetivo deste exercício é observar a tendência de mudança de comportamento após a atualização das instruções, não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem um conjunto de diretrizes e requisitos para as práticas de desenvolvimento. Eles incluem:

- A camada de dados sempre precisa de testes de unidade.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- A documentação deve ser adicionada ao código na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever o que ele faz.

Com arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas permitem fornecer contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso poderoso ajuda a orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar as convenções de código e bibliotecas preferidas, além dos tipos de comentários que gostaria de incluir. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos, fornecendo contexto para cada tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas no repositório. Esse arquivo deve conter informações sobre o projeto — contexto relevante para a maioria das solicitações feitas ao Copilot Chat ou à CLI. Isso pode incluir a pilha de tecnologias, uma visão geral do que está sendo desenvolvido, práticas recomendadas e orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes para linguagens específicas (como TypeScript ou Astro) ou para tarefas como criar um componente de interface ou um novo conjunto de testes de unidade.

> [!NOTE]
> Ao trabalhar no IDE, os arquivos de instruções são usados apenas para a geração de código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, a CLI do Copilot e o agente na nuvem do Copilot usam tanto o arquivo de instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> Por fim, o Copilot [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está além do escopo deste workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha o conteúdo de `copilot-instructions.md` focado nas orientações do projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e padrões gerais de código.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo (testes de unidade, componentes Astro, camada de dados) ou tarefas específicas.
- Use linguagem natural. Mantenha as orientações claras. Inclua exemplos de como o código deve — e não deve — ser escrito.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira de usar IA. Com a experimentação, você descobrirá o que funciona melhor para o seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, talvez você encontre instruções para vários tipos de tarefa, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de um jeito diferente (por exemplo, **Configurar Chat → Gerar instruções do agente** no VS Code ou `/init` na CLI do Copilot) — a lição da interface que você está usando explicará quando isso for relevante.
>
> Procurando modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório — há um arquivo principal, `copilot-instructions.md`, e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Observações para o agente**, **Padrões de código**, **Scripts** e **Estrutura do repositório**. Essas orientações se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` na parte superior — ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript correspondente a `**/*.test.ts`, por exemplo, será incluído.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções (como `unit-tests.instructions.md`) e arquivos existentes no projeto. Os caminhos são relativos ao arquivo de instruções, e não à raiz do repositório. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis, além de indicar ao Copilot exemplos a seguir ao gerar código.

> [!NOTE]
> A seção **Requisitos de formatação de código** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Criar uma branch para nossas alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para nossas alterações.

1. Volte ao codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma nova branch e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **Novo chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam por exercício — cada etapa indicará o que importa.

Para observar o impacto das instruções personalizadas, comece enviando um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para ver a diferença.

1. Feche as guias do editor que estiverem abertas de exercícios anteriores para que o Copilot considere apenas o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar ao Copilot onde o auxiliar deve ficar.
3. Na lista suspensa de agentes da visualização Chat, selecione **Agente** para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela mostrando o seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o seguinte prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um auxiliar de acesso a dados que retorne uma lista de todas as editoras, com o nome e a ID de cada uma. Aplique as alterações ao arquivo.
   ```

5. O Copilot explorará o projeto e aplicará atualizações de código, muitas vezes abrangendo o arquivo auxiliar e os testes.
6. Observe que o auxiliar proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras — isso vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (que se aplica a `src/lib/*.ts`).
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como as respostas do Copilot são probabilísticas, talvez ele adicione comentários de documentação mesmo sem essa orientação. Se isso acontecer, tudo bem — o objetivo continua sendo observar a melhoria de *consistência* após a atualização das instruções.

## Adicionar um novo padrão para o repositório

Como destacado anteriormente, `.github/copilot-instructions.md` foi criado para fornecer informações do projeto ao Copilot. Vamos documentar os padrões de código do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Requisitos de formatação de código**, que deve estar perto da linha 27. Observe como ela documenta os padrões de código do projeto — mas ainda não há nenhuma regra para a documentação no código, por isso o auxiliar gerado não tinha comentários de documentação.
3. Adicione as linhas de Markdown a seguir logo abaixo dos padrões existentes para orientar o Copilot a adicionar cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Toda função exportada deve ter um comentário TSDoc que descreva sua finalidade, seus parâmetros e seu valor de retorno.
   - Antes das importações ou de qualquer código, adicione ao arquivo um bloco de comentários que explique sua finalidade.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientações gerais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais do projeto, como a regra dos comentários de documentação que você acabou de adicionar.

## Executar novamente o prompt e observar a mudança

1. Volte ao Copilot Chat e selecione **Novo chat** para limpar o histórico.
2. Selecione `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um auxiliar de acesso a dados que retorne uma lista de todas as editoras, com o nome e a ID de cada uma. Aplique as alterações ao arquivo.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Auxiliares de acesso a dados de editoras para a plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus IDs e nomes.
    *
    * @param db - O cliente de banco de dados Drizzle.
    * @returns Uma promessa que é resolvida com uma matriz de objetos de editoras.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão do projeto e aplicá-lo a código real que servirá de base para o próximo exercício.

## Confirmar as instruções e enviar a branch

Arquivos de instruções são como qualquer outro recurso do projeto: eles são gerenciados com o mesmo controle de versão que você usaria para qualquer outro item. Então, vamos confirmar as alterações e enviar a branch ao repositório.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. No terminal, confirme se as alterações nas instruções e no auxiliar estão presentes executando:

   ```bash
   git status
   ```

3. No terminal, prepare e confirme as alterações nas instruções e todos os arquivos que o Copilot modificou para a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras atualizações de suporte (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. No terminal, envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar um pull request

Com a branch enviada, devemos criar um pull request e vinculá-lo à issue sobre os padrões de documentação no seu backlog. Poderíamos fazer isso manualmente, mas o Copilot pode agir em nosso nome usando as ferramentas do GitHub já conectadas ao projeto. Vamos pedir ao Copilot para encontrar a issue, criar um PR que a encerre e mesclá-lo.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot atuará no GitHub por você. O modelo do projeto já conecta as ferramentas do GitHub ao seu espaço de trabalho, então não é necessário configurar nada — na primeira vez que o Copilot usar uma delas, o VS Code poderá solicitar que você entre no GitHub. Siga as instruções para permitir o acesso. Você aprenderá como essa conexão funciona (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agente** está selecionado no seletor de agentes.
3. Peça ao Copilot para encontrar a issue relacionada à atualização dos arquivos de instruções e criar um PR a partir da branch atual, descrevendo tanto as atualizações das instruções quanto a nova base do auxiliar para editoras:

   ```
   Encontre a issue relacionada à atualização do arquivo de instruções. Crie um novo PR a partir da branch atual, destaque que o PR encerra essa issue e mencione também que adicionamos a base do auxiliar para editoras, que será usada no próximo trabalho de filtragem.
   ```

4. O Copilot começará a localizar a issue e criar o PR.
5. Quando o Copilot solicitar permissão para executar ações no GitHub em seu nome, revise o comando e selecione **Permitir**, conforme apropriado.
6. Depois que o PR for criado, peça ao Copilot que o mescle e retorne sua branch para `main` com o seguinte prompt:

   ```
   Mescle o PR em main. Depois, volte para main localmente e faça pull do código mais recente para deixar tudo atualizado.
   ```

7. Quando o Copilot solicitar permissão para executar ações no GitHub e comandos do shell em seu nome, revise o comando e selecione **Permitir**, conforme apropriado.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot!

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o que o Copilot produz com as instruções *atuais*;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- executar novamente o mesmo prompt e observar o código proposto adotar o novo padrão;
- confirmar as instruções e a base do auxiliar na branch `main` para que o próximo exercício possa usá-las.

Em seguida, você colocará essas instruções em prática no [modo de agente][next-lesson], enquanto o Copilot adiciona um novo recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
