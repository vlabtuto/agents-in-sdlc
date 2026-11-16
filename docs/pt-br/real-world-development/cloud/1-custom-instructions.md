---
title: "Exercício 1 - Instruções personalizadas (agente na nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente na nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se há informações de contexto que o Copilot deve conhecer —, esse contexto precisa estar disponível. Os [arquivos de instruções][instruction-files] fornecem essas orientações para que o Copilot entenda não apenas *o que* você quer que ele faça, mas também *como* quer que faça.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo definido por caminho;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos ambientes do VS Code e da CLI, aqui você não executará um prompt *antes/depois*. O agente na nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante neste ambiente, ao revisar os pull requests produzidos pelo agente na nuvem.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem um conjunto de diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes de unidade.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- A documentação deve ser adicionada ao código na forma de comentários de documentação TSDoc.
- Um bloco de comentários deve ser adicionado ao início de cada arquivo para descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Esse recurso permite orientar o Copilot para que suas sugestões e trechos de código sejam mais relevantes. Você pode especificar convenções de codificação, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos, fornecendo contexto para cada tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações sobre o projeto — contexto relevante para a maioria das solicitações de chat ou da CLI enviadas ao Copilot. Isso pode incluir a pilha de tecnologias, uma visão geral do que está sendo desenvolvido, práticas recomendadas e orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes para linguagens específicas, como TypeScript ou Astro, ou para tarefas como criar um componente de interface do usuário ou novos testes de unidade.

> [!NOTE]
> Ao trabalhar no IDE, os arquivos de instruções são usados apenas para gerar código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente na nuvem do Copilot usam tanto o arquivo de instruções do repositório quanto os arquivos `*.instructions.md` (com frontmatter `applyTo`) ao gerar código.
>
> Por fim, o Copilot [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma discussão completa sobre como criar arquivos de instruções está além do escopo deste workshop. No entanto, os exemplos do projeto demonstram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções em `copilot-instructions.md` focadas em orientações para o projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e padrões gerais de codificação.
- Use arquivos `*.instructions.md` para fornecer orientações específicas para tipos de arquivo (testes de unidade, componentes Astro, camada de dados) ou tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve (ou não deve) ser escrito.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira certa de usar IA. Com a experimentação, você descobrirá o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, talvez você encontre instruções para vários tipos de tarefa, incluindo [atualizações de interface do usuário][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de uma forma diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI) — a lição da interface que você está usando vai destacar esse recurso quando for relevante.
>
> Está procurando modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
## Explorar os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório — há um arquivo principal, `copilot-instructions.md`, e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Observe também as orientações aninhadas sobre **GitHub Actions Workflows** em **Code standards**. Essas informações se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e examine seu conteúdo. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo — ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript (por exemplo, um que corresponda a `**/*.test.ts`) será incluído.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções (como `unit-tests.instructions.md`) e arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis, além de indicar ao Copilot exemplos para seguir ao gerar código. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários de documentação TSDoc e cabeçalhos de comentários nos arquivos.
## Adicionar um novo padrão para o repositório

Agora você fará a única edição deste exercício: adicionar uma regra válida para todo o projeto, determinando que a documentação seja incluída no código na forma de comentários TSDoc e cabeçalhos de comentários no nível do arquivo. O agente na nuvem aplicará essa regra quando trabalhar nas issues que você atribuir a ele mais adiante neste ambiente.

Antes de editar, prepare uma branch de trabalho (o agente na nuvem lerá suas instruções a partir da branch de destino da issue, mas você fará um commit organizado das alterações):

1. No terminal do codespace, crie e alterne para uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como mencionado anteriormente, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de codificação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar próxima à linha 27. Observe como ela documenta os padrões de codificação do projeto — mas ainda não há uma regra para documentação no código, motivo pelo qual o helper gerado não tinha comentários de documentação.
3. Adicione as linhas de Markdown a seguir logo abaixo dos padrões existentes para orientar o Copilot a adicionar cabeçalhos de comentários nos arquivos e comentários de documentação TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientações gerais, ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo no nível do repositório é o lugar certo para padrões válidos para todo o projeto, como a regra dos comentários de documentação que você acabou de adicionar.
## Fazer commit, enviar por push e mesclar a atualização das instruções

O agente na nuvem lê os arquivos de instruções da branch de destino da issue. Quando você atribuir uma issue ao Copilot no próximo exercício, o Copilot criará uma branch a partir de `main`. Portanto, suas alterações nas instruções precisam chegar à `main` para que o agente na nuvem possa usá-las.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch por push:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente na nuvem lerá essas instruções quando trabalhar nas issues atribuídas a ele no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente na `main` durante este workshop, você pode ignorar a etapa da branch e fazer commit direto na `main`. A etapa da branch foi incluída para que o workshop reproduza o fluxo de trabalho usado em um projeto real.
## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e adicionou um novo padrão para todo o repositório a `.github/copilot-instructions.md`. Você verá esse padrão aplicado nos pull requests que o agente na nuvem produzir ao longo deste ambiente.

Em seguida, [atribua sua primeira issue ao agente na nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
