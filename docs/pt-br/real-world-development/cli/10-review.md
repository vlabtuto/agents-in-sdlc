---
title: "Lição 10 - Conclusão e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em todo o fluxo de trabalho contínuo da Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem já incluída no repositório.
- adicionou classificações por estrelas, revisou o resultado no navegador usando o encaminhamento de porta e fez merge manual do seu primeiro pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou no modo Autopilot e revisou no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e a usou para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor Playwright Model Context Protocol (MCP) e o usou para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar os requisitos, a cobertura, os resultados da skill e as evidências do navegador.
- revisou toda a alteração de filtragem e autorizou o Agent Merge para o PR da filtragem.
- explorou comandos slash para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch baseada na `main` atualizada:

1. **Classificações por estrelas:** exibição do `starRating` existente e de um estado explícito que indique quando um jogo não tem classificação.
2. **Filtragem e fluxo de qualidade:** implementação da filtragem; atualização das instruções e aplicação delas à funcionalidade; personalização do relatório `quality-checks`; criação de um perfil de QA; e inclusão dos testes correspondentes.

Do planejamento da filtragem até a abertura do PR, você usou a mesma conversa e branch. Reunimos esse trabalho em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, sua própria inspeção no navegador e a exploração do navegador pelo Copilot via MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos, e apresentou os resultados no novo formato. Antes do PR, o QA reuniu esses resultados com uma revisão dos requisitos e da cobertura dos testes.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que não exija novos testes também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de qualquer alteração.

## Boas práticas

O contexto e as ferramentas que você fornece ao Copilot moldam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e ajuste-as conforme as necessidades da equipe mudarem. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações e os resultados reais das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use **Plan** para pensar em uma abordagem antes de começar, **Interactive** para acompanhar de perto alterações específicas e **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e um modelo mais capaz para trabalhos complexos.

O contexto continua tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer criar, *por que* e *como* altera significativamente o resultado.

## Explore outros recursos

Você concluiu o fluxo de trabalho principal. Confira mais alguns recursos da CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar tarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa separada.
- `/delegate` para enviar uma tarefa ao agente na nuvem do Copilot.

## Próximos passos

A melhor maneira de se aprimorar com qualquer ferramenta é continuar usando-a! Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você imagina há anos, mas ainda não conseguiu criar. Compartilhe o que aprendeu com sua equipe e aprenda com as experiências dos colegas. E, como sempre, explore a documentação.

Se quiser ampliar o Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente na nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalize o Copilot CLI][customize-cli]
- [Gerencie pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
