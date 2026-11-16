---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios no VS Code, você precisa preparar tudo. Você criará sua própria cópia do repositório Tailspin Toys, iniciará um [codespace][codespaces] para trabalhar e confirmará que o GitHub Copilot Chat está funcionando no editor.

## Configurar o repositório do laboratório

Para criar uma cópia do repositório onde ficará o código que você desenvolverá, você criará uma instância a partir do [modelo][template-repository]. A nova instância conterá todos os arquivos necessários para o laboratório, e você a usará durante os exercícios.

1. Em uma nova janela do navegador, acesse o [modelo do Tailspin Toys][tailspin-template].
2. Crie sua própria cópia do repositório selecionando o botão **Usar este modelo** na página do repositório do laboratório. Em seguida, selecione **Criar um novo repositório**.

    ![Botão Usar este modelo](../../../_images/ex0-use-template.png)

3. Se você estiver fazendo o workshop como parte de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções dos mentores. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Configurações do modelo do repositório](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório que você criou (**nome-da-organização-ou-usuário/nome-do-repositório**), pois você precisará consultá-lo mais adiante no laboratório.

> [!NOTE]
> **Seu backlog está pronto**
>
> Ao criar o repositório a partir do modelo, um backlog de issues do GitHub será criado automaticamente. Você trabalhará com essas issues durante o workshop — não é necessário criar nenhuma.

## Criar um codespace

Em seguida, você usará um codespace para concluir os exercícios do laboratório.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento baseado em nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um IDE completo com suporte a várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório que você acabou de criar.
2. Selecione o botão verde **Código**.

    ![Selecione o botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e selecione o botão **+** para criar um novo Codespace.

    ![Criar um novo codespace](../../../_images/ex0-create-codespace.png)

A criação do codespace levará alguns minutos, embora ainda seja muito mais rápida do que instalar todos os serviços manualmente! Enquanto isso, você pode explorar outros recursos do GitHub Copilot, que veremos a seguir.

> [!CAUTION]
> Você voltará ao codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi criado para ser executado em um codespace ou em um [contêiner de desenvolvimento local][dev-containers]. Ambos garantem que o ambiente tenha todos os pré-requisitos instalados para uma experiência tranquila. Se preferir executá-lo localmente, abra o repositório clonado no VS Code e selecione **Reabrir no contêiner** quando solicitado — o VS Code criará o mesmo contêiner de desenvolvimento usado pelo codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar o GitHub Copilot Chat e o modo de agente

Para acessar o modo de agente do GitHub Copilot Chat, você precisa ter a extensão GitHub Copilot Chat instalada no IDE. Ela já deve estar instalada se você estiver usando um GitHub Codespace.

> [!TIP]
> Se a extensão GitHub Copilot Chat não estiver instalada, você poderá [instalá-la pelo Visual Studio Code Marketplace][copilot-chat-extension]. Outra opção é abrir a visualização **Extensões** no Visual Studio Code, pesquisar **GitHub Copilot Chat** e selecionar **Instalar**.

Depois de instalar a extensão, talvez seja necessário autenticar sua conta do GitHub para ativá-la.

1. Volte ao codespace.
2. Se o Copilot Chat ainda não estiver visível à direita do editor, selecione o ícone **Copilot Chat** na parte superior do codespace.
3. Digite uma mensagem, como "Hello world", na janela do Copilot Chat e pressione <kbd>Enter</kbd>. Isso deve ativar o Copilot Chat.
4. Se você ainda não estiver autenticado, poderá receber uma solicitação para entrar na sua conta do GitHub. Siga as instruções para autenticar.

    ![Exemplo de solicitação de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Após a autenticação, a janela do Copilot Chat deverá aparecer.

## Resumo

Parabéns, você criou uma cópia do repositório do laboratório! Você também iniciou a criação do seu codespace, que usará quando começar a escrever código.

## Próxima etapa

Vamos começar a usar o Copilot. Continue para [Exercício 1 - Instruções personalizadas][next-lesson], onde você ensinará ao Copilot as convenções do seu projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
